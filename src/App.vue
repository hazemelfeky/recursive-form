<script setup>
import { ref } from "vue";
import data from "./data.js";
import SharedSelect from "./components/SharedSelect.vue";

const form = ref({
  category: null,
  subcategory: null,
});

const handleSubmit = () => {
  console.log(form.value);
};
</script>

<template>
  <form @submit.prevent="handleSubmit">
    <h1>Form</h1>
    <SharedSelect
      :form="form"
      name="category"
      label="Main Category*"
      :options="data"
      required
    />
    <SharedSelect
      :form="form"
      name="subcategory"
      label="Sub category*"
      :options="data"
      required
      :disabled="!form.category"
    />
    <fieldset v-if="form.subcategory">
      <SharedSelect
        :form="form"
        name="type"
        label="Process Type"
        :options="data"
        has-others
      />
      <!-- <SharedSelect
        :form="form"
        name="subcategory"
        label="Sub category"
        :options="data"
      />
      <SharedSelect
        :form="form"
        name="subcategory"
        label="Sub category"
        :options="data"
      />
      <SharedSelect
        :form="form"
        name="subcategory"
        label="Sub category"
        :options="data"
      /> -->
    </fieldset>
    <button>Submit</button>
  </form>
</template>
