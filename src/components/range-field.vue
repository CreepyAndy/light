<template>
  <div>
    <input type="number" @input="onInput1" />
    ~
    <input type="number" @input="onInput2" />
    <span>{{ errorMessage }}</span>
  </div>
</template>
<script lang="ts">
import { useField } from 'vee-validate';
import { ref } from 'vue';
export default {
  props: {
    name: {
      type: String,
      required: true
    }
  },
  setup() {
    const _value = ref([null, null]);
    const onInput1 = function(e: any) {
      _value.value.splice(0, 1, e.target.value);
      preCheckAndSetFieldValue();
    }
    const onInput2 = function(e: any) {
      _value.value.splice(1, 1, e.target.value);
      preCheckAndSetFieldValue();
    }
    const preCheckAndSetFieldValue = function () {
      const isInvalid = _value.value.some(v => {
        if (v === '' || !v) {
          return true;
        }
        const number = Number(v);
        if (isNaN(number)) {
          return true;
        }
        return false;
      })
      if (isInvalid) return;
      setValue(_value);
    }
    const { value, errorMessage, setValue } = useField('red0', (value, ctx) => {
      return '';
    });
    return {
      _value,
      errorMessage,
      onInput1,
      onInput2,
    };
  },
};
</script>