<template>
  <form @submit="onSubmit" novalidate>
    <div v-for="(field, idx) in fields" :key="field.key">
      <Field :name="`links[${idx}].url`" type="url" />
      <button type="button" @click="remove(idx)">Remove</button>
    </div>
    <button type="button" @click="push({ url: '' })">Add</button>
    <button>Submit</button>
  </form>
</template>
<script>
import { Field, useForm, useFieldArray } from 'vee-validate';
export default {
  components: {
    Field,
  },
  setup() {
    const { handleSubmit } = useForm({
      initialValues: {
        links: [{ id: 1, url: 'https://github.com/logaretm' }],
      },
    });
    const { remove, push, fields } = useFieldArray('links');
    const onSubmit = handleSubmit(values => {
      console.log(JSON.stringify(values, null, 2));
    });
    return {
      fields,
      push,
      remove,
      onSubmit,
    };
  },
};
</script>