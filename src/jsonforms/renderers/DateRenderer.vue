<template>
  <v-text-field
    :class="$style.input"
    :label="control.label"
    :hint="control.description"
    type="date"
  />
</template>

<script lang="ts">
  import {
    ControlElement,
    JsonFormsRendererRegistryEntry,
    rankWith,
    isDateControl,
    and
  } from '@jsonforms/core';
  import { defineComponent } from 'vue';
  import { rendererProps, useJsonFormsControl, RendererProps } from '@jsonforms/vue';
  import { useVanillaControl } from '@jsonforms/vue-vanilla';


  const inputRenderer = defineComponent({
    name: 'date-renderer',
    props: {
      ...rendererProps<ControlElement>()
    },
    setup (props: RendererProps<ControlElement>) {
      return useVanillaControl(useJsonFormsControl(props), target => target.value || undefined);
    }
  });

  export default inputRenderer;

  export const entry: JsonFormsRendererRegistryEntry = {
    renderer: inputRenderer,
    tester: rankWith(3, and(isDateControl))
  };
</script>

<style module>
  .input {
    margin-bottom: 8px;
    text-align: left;
  }
</style>

