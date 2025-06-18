---
title: MySQL-可重复读和幻读的关系
date: 2025-06-18 11:48:15
tags: MySQL
---

## 可重复读的"快照"到底是什么？

在 MySQL 的可重复读隔离级别下：

1. 事务开始时，会创建一个"一致性视图"（快照）
2. **这个快照只针对已经存在的记录**，不是整个数据库的完整快照
3. 对于快照中的记录，事务期间看到的数据是一致的（所以叫"可重复读"）

## 幻读是怎么发生的？

幻读特指**新插入的行**导致的读取不一致。举个例子：

### 测试场景

```sql
-- 准备表和数据
CREATE TABLE account (id INT PRIMARY KEY, name VARCHAR(50), balance DECIMAL(10,2));
INSERT INTO account VALUES (1, '张三', 1000), (2, '李四', 2000);

-- 事务A
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
START TRANSACTION;
-- 第一次查询：只有张三和李四
SELECT * FROM account WHERE balance > 1500;  -- 结果：李四(2000)

-- 事务B（此时插入新数据）
INSERT INTO account VALUES (3, '王五', 3000);
COMMIT;

-- 事务A
-- 第二次同样的查询：出现了王五(幻读)
SELECT * FROM account WHERE balance > 1500;  -- 结果：李四(2000)和王五(3000)
COMMIT;
```

### 为什么会出现幻读？

1. **快照范围限制**：快照只包含事务开始时已经存在的记录，不包含之后插入的新记录
2. **查询条件变化**：当你的查询条件能匹配到新插入的行时，这些行就会"突然出现"
3. **与不可重复读的区别**：
   - 不可重复读：针对同一记录的多次读取结果不同
   - 幻读：查询结果集的行数变化（多了"幻影行"）

## MySQL 如何解决幻读？

虽然标准 SQL 的可重复读允许幻读，但 MySQL 的 InnoDB 引擎通过**间隙锁(Gap Lock)**在可重复读级别下也避免了幻读：

```sql
-- 事务A
START TRANSACTION;
-- 使用SELECT FOR UPDATE加锁，会锁定balance>1500的范围
SELECT * FROM account WHERE balance > 1500 FOR UPDATE;  -- 加间隙锁

-- 事务B尝试插入
INSERT INTO account VALUES (3, '王五', 3000);  -- 会被阻塞，直到事务A提交
```

## 关键总结

1. **快照 ≠ 完整快照**：只保证已存在记录的一致性，不保证新插入的记录
2. **幻读本质**：是查询结果集的行数变化，不是单条记录内容变化
3. **MySQL 的特殊处理**：通过间隙锁在可重复读级别也避免了幻读（这是 MySQL 的增强）
4. **标准与实现的区别**：按照 SQL 标准，可重复读允许幻读，但 MySQL 的实现更严格

可以这样记忆：可重复读保证的是"看到的不会变"，但不保证"看不到的不会来"。
