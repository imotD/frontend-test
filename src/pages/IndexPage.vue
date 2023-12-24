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
            modelProv = null;
          }
        "
      />

      <SelectBaseCity
        v-model="modelCity"
        :disable="!modelProv"
        :optionValue="optionCity"
        title="Kota"
        @filter-fn="fetchCity"
        @clear="
          () => {
            modelCity = null;
          }
        "
      />

      <q-card-actions vertical align="right">
        <q-btn to="/city" flat dense>Cek Semua Kota</q-btn>
      </q-card-actions>
    </q-card>
  </q-page>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { api } from "boot/axios";

import SelectBaseProv from "src/components/SelectBase.vue";
import SelectBaseCity from "src/components/SelectBase.vue";

interface Province {
  id: string;
  name: string;
}

interface City {
  id: string;
  province_id: string;
  name: string;
}

const modelProv = ref<Province | null>(null);
const modelCity = ref<City | null>(null);

const optionProv = ref<Province[]>([]);
const optionCity = ref<City[]>([]);

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
