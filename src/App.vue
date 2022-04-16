<template>
  <div class="cont">
    <div class="cont__color-group">
      <div v-for="(field, index) in redFields" :key="field.key" style="display: flex">
        <range-field :name="rangeFieldName('red', index)"></range-field>
        <button @click="redPush([null, null])">+</button>
        <button @click="redRemove(index)">-</button>
      </div>
    </div>

    <!-- <div class="cont__color-group">
      <div v-for="(field, index) in fields" :key="field.key" style="display: flex">
        <range-field :name="rangeFieldName('yellow', index)"></range-field>
        <button @click="onAdd">+</button>
      </div>
    </div> -->
    
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
        reds: [
          [2, 4]
        ],
      },
    });
    const { remove: redRemove, push: redPush, fields: redFields } = useFieldArray('reds');
    const rangeFieldName = function (type: 'red' | 'yellow' | 'green', index: number) {
      return `${type}s[${index}]`;
    }
    return {
      redFields,
      redRemove,
      redPush,
      rangeFieldName,
    }
  }
});
</script>

<style>

</style>
