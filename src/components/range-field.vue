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
  setup(props) {
    const formValue = ref([null, null]);
    const onInput1 = function(e: any) {
      formValue.value.splice(0, 1, e.target.value);
      preCheckAndSetFieldValue();
    }
    const onInput2 = function(e: any) {
      formValue.value.splice(1, 1, e.target.value);
      preCheckAndSetFieldValue();
    }
    const preCheckAndSetFieldValue = function () {
      const isInvalid = formValue.value.some(v => {
        if (!v) {
          return true;
        }
        const number = Number(v);
        if (isNaN(number)) {
          return true;
        }
        return false;
      })
      if (isInvalid) return;
      setValue(formValue);
    }
    const { value, errorMessage, setValue } = useField(props.name, (value, ctx) => {
      debugger
      return '';
    });
    return {
      formValue,
      errorMessage,
      onInput1,
      onInput2,
    };
  },
};
</script>