<script setup>
import { ref, onMounted } from "vue";
import data from "./data.js";
import SharedSelect from "./components/SharedSelect.vue";
import TableDate from "./components/TableDate.vue";

const apiKey = "3%o8i}_;3D4bF]G5@22r2)Et1&mLJ4?$@+16";
const submited = ref(false);
const categories = ref([]);
const subCategories = ref([]);
const form = ref({});
const tableData = ref([]);

onMounted(async () => {
  await getCategories();
});

// Show table on submit
const handleSubmit = () => {
  submited.value = true;
  tableData.value = Object.entries(form.value);
};

// Global function to handle request
const getRequest = async (url) => {
  try {
    const res = await fetch(url, {
      headers: {
        "Content-Type": "application/json",
        "private-key": apiKey,
      },
    });
    const data = await res.json();
    return data;
  } catch (error) {
    console.error("There was a problem with the fetch operation:", error);
  }
};

const getCategories = async () => {
  const res = await getRequest(
    "https://staging.mazaady.com/api/v1/get_all_cats"
  );
  categories.value = res.data.categories;
};

const onSelectCategories = (event) => {
  // Set subcategories options to children of main category
  const children = categories.value.find(
    (el) => el.name == event.target.value
  ).children;
  subCategories.value = children;
};
</script>

<template>
  <div class="app">
    <form v-if="!submited" @submit.prevent="handleSubmit">
      <h1>Form</h1>
      <SharedSelect
        :form="form"
        name="category"
        label="Main Category*"
        :options="categories"
        @change="onSelectCategories"
        required
      />
      <SharedSelect
        :form="form"
        name="subcategory"
        label="Sub category*"
        :options="subCategories"
        required
        :disabled="!form.category"
      />
      <fieldset v-if="form.subcategory">
        <SharedSelect
          :form="form"
          name="process-type"
          label="Process Type"
          :options="data"
          has-others
        />
        <SharedSelect
          :form="form"
          name="brand"
          label="Brand"
          :options="data"
          has-others
        />
        <SharedSelect
          :form="form"
          name="model"
          label="Model"
          :options="data"
          has-others
          :disabled="!form.brand"
        />
        <SharedSelect
          :form="form"
          name="type"
          label="Type"
          :options="data"
          has-others
          :disabled="!form.model"
        />
        <SharedSelect
          :form="form"
          name="transmission"
          label="Transmission Type"
          :options="data"
          has-others
        />
      </fieldset>
      <button>Submit</button>
    </form>
    <div v-else>
      <TableDate :data="tableData" />
      <button @click="submited = false">Back To From</button>
    </div>
  </div>
</template>
