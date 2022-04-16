<template>
  <div v-for="(field, index) in fields" :key="field.key" style="display: flex">
    <range-field :name="rangeFieldName('red', index)"></range-field>
    <button @click="onAdd">+</button>
  </div>
</template>

<script lang="ts">
import { useForm, useFieldArray } from 'vee-validate';
import { computed, defineComponent } from 'vue';
import RangeField from './components/range-field.vue';

export default defineComponent({
  name: 'App',
  components: {
    RangeField
  },
  setup() {
    useForm({
      initialValues: {
        reds: [{}],
      },
    });
    const { remove, push, fields } = useFieldArray('reds');
    const rangeFieldName = function (type: 'red' | 'yellow' | 'green', index: number) {
      return `${type}s${index}`;
    }
    const onAdd = function () {
      push([null, null])
    }
    return {
      fields,
      rangeFieldName,
      onAdd
    }
  }
});
</script>

<style>

</style>
