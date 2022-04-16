<template>
  <div class="cont">
    <div class="light-group">
      <h1 class="light-group__title">Red</h1>
      <div v-for="(field, index) in redFields" :key="field.key" style="display: flex">
        <range-field :name="rangeFieldName('red', index)"></range-field>
        <button @click="redPush([null, null])">+</button>
        <button @click="redRemove(index)">-</button>
      </div>
    </div>

    <div class="light-group">
      <h1 class="light-group__title">Green</h1>
      <div v-for="(field, index) in greenFields" :key="field.key" style="display: flex">
        <range-field :name="rangeFieldName('green', index)"></range-field>
        <button @click="greenPush([null, null])">+</button>
        <button @click="greenRemove(index)">-</button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { useForm, useFieldArray } from 'vee-validate';
import { computed, defineComponent, provide } from 'vue';
import RangeField from './components/range-field.vue';

export default defineComponent({
  name: 'App',
  components: {
    RangeField
  },
  setup() {
    const { meta } = useForm({
      initialValues: {
        reds: [
          [null, null]
        ],
        greens: [
          [null, null]
        ]
      },
    });
    provide('formMeta', meta);
    const { remove: redRemove, push: redPush, fields: redFields } = useFieldArray('reds');
    const { remove: greenRemove, push: greenPush, fields: greenFields } = useFieldArray('greens');
    const rangeFieldName = function (type: 'red' | 'yellow' | 'green', index: number) {
      return `${type}s[${index}]`;
    }
    return {
      redRemove,
      redPush,
      rangeFieldName,
      redFields,

      greenRemove,
      greenPush,
      greenFields,
    }
  }
});
</script>

<style>
.cont {
  display: flex;
}
.light-group {

}
.light-group__title {

}
</style>
