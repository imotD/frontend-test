<template>
  <q-page class="flex flex-center">
    <q-card class="my-card q-pa-md">
      <SelectBaseCity
        v-model="modelCity"
        :optionValue="optionCity"
        title="Semua Kota"
        @filter-fn="fetchCity"
        @clear="
          () => {
            modelCity = '';
          }
        "
      />
      <q-card-actions vertical align="left">
        <q-btn to="/" flat dense>Kembali</q-btn>
      </q-card-actions>
    </q-card>
  </q-page>
</template>

<script setup>
import { ref } from "vue";
import { axios } from "boot/axios";
import SelectBaseCity from "src/components/SelectBase.vue";

const modelCity = ref("");
const optionCity = ref([]);

const fetchCity = () => {
  axios
    .get("https://65866566468ef171392e36a7.mockapi.io/city")
    .then((res) => {
      optionCity.value = res.data;
    })
    .catch(() => {
      console.log("error");
    });
};
</script>

<style lang="scss" scoped></style>
