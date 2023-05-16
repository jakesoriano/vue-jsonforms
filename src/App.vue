<script setup lang="ts">
import { JsonForms } from '@jsonforms/vue';
import { vanillaRenderers } from '@jsonforms/vue-vanilla';

import { customRenderers } from './jsonforms/renderers'

import * as schema from './jsonforms/schemas/schema.json';
import * as uischema from './jsonforms/schemas/uischema.json';

const renderers: Object = Object.freeze([
  ...vanillaRenderers,
  ...customRenderers,
]);

</script>

<template>
  <div>
    <h1>Your Profile</h1>
    <div class="form">
      <JsonForms
        :renderers="renderers"
        :schema="schema"
        :uischema="uischema"
      />
    </div>
    <button class="button">Next</button>
  </div>
</template>
<style>
.form {
  margin-top: 28px;
  min-width: 682px;
}
.button {
  background-color: #213547;
  color: #ffffff;
  padding: 8px 14px;
  border-radius: 8px;
  transition: background-color 0.3s ease-in-out;
}

.button:hover {
  background-color: #385b7a;
}

.horizontal-layout {
  display: flex;
  flex-direction: row;
}

.horizontal-layout-item {
  flex: 1;
}

.horizontal-layout-item:last-of-type {
  margin-left: 8px;
}
</style>