<template>
  <div class="magic-ui-iterator-container">
    <Container v-for="(item, index) in configs" :key="index" :config="item"></Container>
  </div>
</template>

<script lang="ts" setup>
import { computed } from 'vue';

import { type MContainer, NodeType } from '@tmagic/schema';
import { useApp } from '@tmagic/vue-runtime-help';

import Container from '../../container';

const props = withDefaults(
  defineProps<{
    config: MContainer & {
      type: 'iterator-container';
      iteratorData: any[];
      dsField: string[];
      itemConfig: {
        layout: string;
        style: Record<string, string | number>;
      };
    };
    model?: any;
  }>(),
  {
    model: () => ({}),
  },
);

const { app } = useApp({
  config: props.config,
  methods: {},
});

const configs = computed(() => {
  let { iteratorData = [] } = props.config;

  if (!Array.isArray(iteratorData)) {
    iteratorData = [];
  }

  if (app?.platform === 'editor' && !iteratorData.length) {
    iteratorData.push({});
  }

  return iteratorData.map((itemData) => ({
    items:
      app?.dataSourceManager?.compliedIteratorItems(itemData, props.config.items, props.config.dsField) ??
      props.config.items,
    id: '',
    type: NodeType.CONTAINER,
    style: {
      ...props.config.itemConfig.style,
      position: 'relative',
      left: 0,
      top: 0,
    },
  }));
});
</script>
