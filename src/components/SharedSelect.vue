<script setup>
import { ref } from "vue";

const props = defineProps({
  form: { type: Object, default: () => ({}) },
  name: { type: String, default: "value" },
  label: { type: String, default: "value" },
  itemName: { type: String, default: "name" },
  itemValue: { type: String, default: "name" },
  options: { type: Array, default: () => [] },
  hasOthers: { type: Boolean, default: false },
});

const emit = defineEmits();

const selectValue = ref({});

const handleChange = (e) => {
  const val = e.target.value;
  if (val != "") props.form[props.name] = selectValue.value;
};
</script>
<template>
  <div class="input-container">
    <label :for="name">
      <div>{{ label }}</div>
      <select :id="name" v-model="selectValue" @change="handleChange">
        <option
          v-for="option in options"
          :key="option[itemName]"
          :value="option"
        >
          {{ option[itemValue] }}
        </option>
        <option value="" v-if="hasOthers">Others</option>
      </select>
      <input type="text" v-if="selectValue == ''" v-model="form[name]" />
    </label>
    <SharedSelect
      has-others
      :form="form"
      v-for="property in form[name]?.properties"
      :key="property.id"
      :name="property.slug"
      :label="property.name"
      :options="property.options"
      @change="$emit('get-options-child')"
    />
  </div>
</template>
