<template>
  <q-page class="flex flex-center">
    <q-card class="my-card q-pa-md">
      <SelectBaseProv
        v-model="modelProv"
        :optionValue="optionProv"
        title="Provinsi"
        @filter-fn="fetchProvinces"
        @clear="
          () => {
            modelProv = '';
          }
        "
      />

      <SelectBaseCity
        v-model="modelCity"
        :disable="Object.keys(modelProv).length == 0"
        :optionValue="optionCity"
        title="Kota"
        @filter-fn="fetchCity"
        @clear="
          () => {
            modelCity = '';
          }
        "
      />

      <q-card-actions vertical align="right">
        <q-btn to="/city" flat dense>Cek Semua Kota</q-btn>
      </q-card-actions>
    </q-card>
  </q-page>
</template>

<script setup>
import { ref } from "vue";
import { api } from "boot/axios";

import SelectBaseProv from "src/components/SelectBase.vue";
import SelectBaseCity from "src/components/SelectBase.vue";

const modelProv = ref("");
const modelCity = ref("");

const optionProv = ref([]);
const optionCity = ref([]);

const fetchProvinces = () => {
  api
    .get("/api/provinces.json")
    .then((res) => {
      optionProv.value = res.data;
    })
    .catch(() => {
      console.log("error");
    });
};

const fetchCity = () => {
  api
    .get(`/api/regencies/${modelProv.value?.id}.json`)
    .then((res) => {
      optionCity.value = res.data;
    })
    .catch(() => {
      console.log("error");
    });
};
</script>

<style lang="scss">
.my-card {
  width: 20rem;
}
</style>
