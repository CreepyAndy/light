<template>
  <div v-for="(field, idx) in fields" :key="field.key" style="display: flex">
    <range-field :name="`red[${idx}]`"></range-field>
    <button>+</button>
  </div>
</template>

<script lang="ts">
import { useForm, useFieldArray } from 'vee-validate';
import { defineComponent } from 'vue';
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
    return {
      fields
    }
  }
});
</script>

<style>

</style>
