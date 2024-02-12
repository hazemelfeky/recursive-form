<script setup>
import { ref } from "vue";
import SharedSelect from "./components/SharedSelect.vue";

const form = ref({
  main: null,
});

const mainOptions = [
  { value: "cars", name: "Cars" },
  { value: "Motocycles", name: "Motocycles" },
  { value: "Banana", name: "Banana" },
];

const handleSubmit = () => {
  console.log(form.value);
};
</script>

<template>
  <form @submit.prevent="handleSubmit">
    <h1>Form</h1>
    <SharedSelect
      :form="form"
      name="main"
      label="label*"
      :options="mainOptions"
      required
    />
    <button>Submit</button>
  </form>
</template>
