<template>
  <div style="margin: 10px">
    <input type="number" v-model="value[0]" :disabled="!editable"/>
    ~
    <input type="number" v-model="value[1]" :disabled="!editable"/>
    <span style="color: red; display: block">{{ errorMessage }}</span>
  </div>
</template>
<script lang="ts">
import { useField } from 'vee-validate';
import { computed, inject, ref } from 'vue';
export default {
  props: {
    name: {
      type: String,
      required: true
    }
  },
  setup(props: any) {
    const formMeta = inject('formMeta') as any;
    const { value, errorMessage, meta } = useField<[''|number, ''|number]>(props.name, (targetRange: (number|'')[], ctx: any) => {
      if (targetRange.every(input => input === '')) {
        return true
      }
      if (targetRange.some(input => input === '')) {
        return '请输入合法的范围';
      }
      const form = JSON.parse(JSON.stringify(ctx.form)); // remove reactivity
      const type: 'reds' | 'greens' = ctx.field.substring(0, ctx.field.indexOf('['));
      const index = Number(ctx.field.substring(ctx.field.indexOf('[') + 1, ctx.field.indexOf(']')));
      form[type].splice(index, 1);
      // const reducedFields = Object.values(form).reduce((a, c) => (a as any).concat(c) , []);
      let duplicatedResult;
      for (let [type, values] of Object.entries(form)) {
        values = (values as number[][]).filter(v => v[0] && v[1]);
        const duplicatedIndex = (values as number[][])
          .map(v => rangeFill(v[0], v[1]))
          .findIndex(existingValues => existingValues.some(v => rangeFill((targetRange as number[])[0], (targetRange as number[])[1]).includes(v)));
        if (duplicatedIndex !== -1) {
          duplicatedResult = {
            type,
            index: duplicatedIndex
          }
          break;
        }
      }
      if (duplicatedResult) {
        return `与${duplicatedResult.type}的第${duplicatedResult.index + 1}项有重叠`
      }
      return true;
    });
    const editable = computed(() => {
      if (!meta.valid) {
        return true;
      }

      return formMeta.value.valid
    })
    
    return {
      errorMessage,
      value,
      editable
    };
  },
};

function rangeFill(start: number, end: number) {
  return Array(Math.abs(end - start) + 1).fill(0).map((_, idx) => Math.min(start, end) + idx);
}


const doValidate = function(target: number[], others: number[][]) {
  const existingValues = new Set();

  others.map(o => rangeFill(o[0], o[1])).forEach(filledArray => {
    filledArray.forEach(val => existingValues.add(val))
  })

  return rangeFill(target[0], target[1]).some(targetVal => existingValues.has(targetVal))
}
</script>