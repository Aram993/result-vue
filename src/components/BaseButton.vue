<template>
  <button @click="newNumber++">{{ text }} {{ number }} {{ newNumber }} {{ fixedNumber }}</button>
</template>
<script setup>
import { computed, ref } from 'vue';

  const props = defineProps({
    text: {
      type: String,
      // required: true,
      // default: "Значение по умолчанию"
      validator(value) {
        return ["Добавить", "Удалить"].includes(value)
      }
    },
    number: [Number, String]
  });
  const newNumber = ref(props.number);
  const fixedNumber = computed(()=> props.number.toFixed(5));
  console.log(props)
</script>
