<script setup>
defineProps({
  form: { type: Object, default: () => ({}) },
  name: { type: String, default: "value" },
  label: { type: String, default: "value" },
  itemName: { type: String, default: "value" },
  itemValue: { type: String, default: "value" },
  options: { type: Array, default: () => [] },
});
</script>
<template>
  <div>
    <label :for="name">
      <div>{{ label }}</div>
      <select :id="name" v-model="form[name]" v-bind="$attrs">
        <option
          v-for="option in options"
          :key="option[itemName]"
          :value="option[itemName]"
        >
          {{ option[itemValue] }}
        </option>
      </select>
    </label>
  </div>
</template>
