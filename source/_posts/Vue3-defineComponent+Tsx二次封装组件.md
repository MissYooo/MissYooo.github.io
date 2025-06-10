---
title: Vue3-defineComponent+Tsx二次封装组件
date: 2025-06-10 20:39:22
tags: Vue3
---

Button.tsx

```tsx
import { computed, defineComponent, useTemplateRef } from "vue";
import { Button } from "ant-design-vue";
import buttonProps from "ant-design-vue/es/button/buttonTypes";

// Props
export const oButtonProps = () => ({
  ...buttonProps(),
});

// Slots
export const oButtonSlots = () => ({
  ...Button.slots,
});

// Expose
export type OButtonExpose = {
  aButtonRef: InstanceType<typeof Button> & {
    blur: () => void;
    focus: () => void;
  };
};

// InstanceType
export type OButtonInstance = InstanceType<typeof OButton> & OButtonExpose;

// Component
export const OButton = defineComponent({
  name: "OButton",
  props: oButtonProps(),
  slots: oButtonSlots(),
  setup(props, { attrs, slots, expose }) {
    const classes = computed(() => ["hd-btn", attrs.class]);
    const aButtonRef = useTemplateRef("a-button");
    expose({
      aButtonRef,
    });
    return () => (
      <Button {...props} class={classes.value} ref="a-button">
        {slots}
      </Button>
    );
  },
});
```
