<template>
  <v-text-field
    :class="$style.input"
    :label="control.label"
    :rules="rules"
    :hint="control.description"
  />
</template>

<script lang="ts">
  import {
    ControlElement,
    JsonFormsRendererRegistryEntry,
    rankWith,
    isStringControl,
    and
  } from '@jsonforms/core';
  import { defineComponent } from 'vue';
  import { rendererProps, useJsonFormsControl, RendererProps } from '@jsonforms/vue';
  import { useVanillaControl } from '@jsonforms/vue-vanilla';

  const inputRenderer = defineComponent({
    name: 'input-renderer',
    data () {
      const isEmail: boolean = this.control.uischema?.format === 'email';
      let rules: Array<any> = [
        (value: string) => !!value || 'This field is required.',
        (value: string) => value.length > 1 || 'Minimum of 2 characters.'
      ];
      if (isEmail) {
        rules = [
          ...rules,
          (value: string) => {
            const pattern: RegExp = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
            return pattern.test(value) || 'Invalid e-mail.'
          }
        ];
      }
      return {
        rules
      }
    },
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
    tester: rankWith(2, and(isStringControl))
  };
</script>

<style module>
  .input {
    margin-bottom: 8px;
    text-align: left;
  }
</style>
