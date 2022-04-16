<template>
  <div>
    <input type="number" v-model="value[0]" />
    ~
    <input type="number" v-model="value[1]" />
    <span style="color: red">{{ errorMessage }}</span>
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
    const { value, errorMessage, setValue } = useField<[null|number, null|number]>(props.name, (value, ctx) => {
      return '';
    });
    // const preCheckAndSetFieldValue = function () {
    //   const isInvalid = value.value.some(v => {
    //     if (!v) {
    //       return true;
    //     }
    //     const number = Number(v);
    //     if (isNaN(number)) {
    //       return true;
    //     }
    //     return false;
    //   })
    //   if (isInvalid) return;
    // }
    
    return {
      errorMessage,
      value
    };
  },
};
</script>