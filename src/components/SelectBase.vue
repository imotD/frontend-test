<template>
  <q-card-section>
    <q-select
      outlined
      transition-show="jump-up"
      transition-hide="jump-up"
      v-model="model"
      :options="optionValue"
      :label="title"
      :disable="disable"
      clearable
      clear-icon="close"
      @clear="emit('clear')"
      @filter="filter"
      option-label="name"
    />
  </q-card-section>
</template>

<script setup>
import { defineProps, defineEmits, ref, watch } from "vue";

const props = defineProps({
  title: String,
  optionValue: {
    type: Array,
    default: [],
  },
  disable: Boolean,
  modelValue: {
    type: [Object, String],
    default: "",
  },
});

const model = ref(props.modelValue);

watch(
  () => props.modelValue,
  (newValue) => {
    model.value = newValue;
  }
);

watch(
  () => model.value,
  (newValue) => {
    emit("update:modelValue", newValue);
  }
);

const emit = defineEmits(["clear", "filterFn", "update:modelValue"]);

const filter = (val, update) => {
  setTimeout(() => {
    update(() => {
      emit("filterFn");
    });
  }, 1000);
};
</script>

<style lang="scss" scoped></style>
