<template>
  <component
    :is="containerComponent"
    :class="className"
    :style="style"
    :data-tmagic-id="config.id"
    :config="{ ...config, [IS_DSL_NODE_KEY]: false }"
  ></component>
</template>

<script lang="ts">
import { defineComponent, inject, type PropType } from 'vue-demi';

import type TMagicApp from '@tmagic/core';
import { IS_DSL_NODE_KEY, type MPageFragment } from '@tmagic/core';
import { registerNodeHooks, useComponent, useComponentStatus, useNode } from '@tmagic/vue-runtime-help';

export default defineComponent({
  name: 'tmagic-page-fragment',

  props: {
    config: {
      type: Object as PropType<MPageFragment>,
      required: true,
    },
    model: {
      type: Object,
      default: () => ({}),
    },
  },

  setup(props) {
    const app = inject<TMagicApp>('app');
    const node = useNode(props, app);
    registerNodeHooks(node);

    const containerComponent = useComponent({ componentType: 'container', app });
    const { style, className } = useComponentStatus(props);
    return {
      style,
      className,
      containerComponent,
      IS_DSL_NODE_KEY,
    };
  },
});
</script>
