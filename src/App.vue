<script setup>
import { ref, onMounted } from "vue";
import SharedSelect from "./components/SharedSelect.vue";
import TableDate from "./components/TableDate.vue";

const apiKey = "3%o8i}_;3D4bF]G5@22r2)Et1&mLJ4?$@+16";
const submited = ref(false);
const categories = ref([]);
const subCategories = ref([]);
const form = ref({});
const tableData = ref([]);
const properties = ref([]);

onMounted(async () => {
  await getCategories();
});

// Show table on submit
const handleSubmit = () => {
  submited.value = true;
  const newObject = {};

  for (const key in form.value) {
    const value = form.value[key];
    if (value && typeof value === "object") {
      // Extract the 'name' property value if it exists
      newObject[key] = value.name;
    } else {
      newObject[key] = value;
    }
  }
  tableData.value = Object.entries(newObject);
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

const getProperties = async (id) => {
  const res = await getRequest(
    `https://staging.mazaady.com/api/v1/properties?cat=${id}`
  );
  return res.data;
  // categories.value = res.data.categories;
};

const getOptionsChild = async (id) => {
  // console.log("getOptionsChild");
  const res = await getRequest(
    `https://staging.mazaady.com/api/v1/get-options-child/${id}`
  );
  return res.data;
  // categories.value = res.data.categories;
};

const onSelectCategories = () => {
  // Set subcategories options to children of main category
  const children = categories.value.find(
    (el) => el.id == form.value.category.id
  ).children;
  subCategories.value = children;
};

const onSelectSubategories = async () => {
  // Set subcategories options to children of main category
  const id = form.value.subcategory.id;
  const props = await getProperties(id);
  properties.value = props;
};

const onSelectProperty = async (property) => {
  // Get children properties of this property
  const id = form.value[property.slug].id;
  const res = await getOptionsChild(id);
  form.value[property.slug].properties = res;
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
        @change="onSelectSubategories"
      />
      <SharedSelect
        v-for="property in properties"
        :key="property.id"
        :form="form"
        :name="property.slug"
        :label="property.name"
        :options="property.options"
        @change="onSelectProperty(property, $event)"
        has-others
      />
      <button>Submit</button>
    </form>
    <div v-else>
      <TableDate :data="tableData" />
      <button @click="submited = false">Back To From</button>
    </div>
  </div>
</template>
