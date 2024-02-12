<script setup>
import { ref } from "vue";
import data from "./data.js";
import SharedSelect from "./components/SharedSelect.vue";
import TableDate from "./components/TableDate.vue";

const submited = ref(false);
const form = ref({
  category: null,
  subcategory: null,
});
const tableData = ref([]);

const handleSubmit = () => {
  submited.value = true;
  tableData.value = Object.entries(form.value);
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
