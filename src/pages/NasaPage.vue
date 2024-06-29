<template>
  <div class="page q-pa-md q-gutter-md">
    <div class="filter">
      <NasaFilter />
    </div>
    <div class="list">
      <NasaList />
    </div>
  </div>
</template>
<style scoped>
.page {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
}

.filter {
  flex: 0 0 300px;
  margin-right: 16px;
}

.list {
  flex: 1;
}
</style>
<script>
import NasaFilter from "src/components/nasa/NasaFilter.vue";
import NasaList from "src/components/nasa/NasaList.vue";
export default {
  name: "NasaPage",
  components: {
    NasaFilter,
    NasaList,
  },
};
</script>
