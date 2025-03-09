<script setup>
import { ref, onMounted, computed } from "vue";
import HeaderComponent from "./components/HeaderComponent.vue";
import TableComponent from "./components/TableComponent.vue";
import FooterComponent from "./components/FooterComponent.vue";

const photosData = ref([]);
const loading = ref(false);
const currentPage = ref(1);
const limit = 400; 
const albumIds = ref([]); 
const sortBy = ref("id"); 
const sortDirection = ref("asc");


const fetchPhotos = async () => {
  loading.value = true;
  const albumFilter = albumIds.value.length > 0
    ? albumIds.value.map(id => `albumId=${id}`).join("&")
    : ""; 

  const url = `https://jsonplaceholder.typicode.com/photos?${albumFilter}&_limit=${limit}&_start=${(currentPage.value - 1) * limit}`;
  const response = await fetch(url);
  const data = await response.json();
  photosData.value.push(...data);
  loading.value = false;
};

const handleScroll = (event) => {
  const container = event.target;
  if (container.scrollTop + container.clientHeight >= container.scrollHeight && !loading.value) {
    currentPage.value++;
    fetchPhotos();
  }
};

const handleSearch = (ids) => {
  albumIds.value = ids; 
  photosData.value = [];
  currentPage.value = 1;
  fetchPhotos();
};

const sortTable = (column) => {
  if (sortBy.value === column) {
    sortDirection.value = sortDirection.value === "asc" ? "desc" : "asc";
  } else {
    sortBy.value = column;
    sortDirection.value = "asc";
  }
};

const sortedPhotosList = computed(() => {
  return [...photosData.value].sort((a, b) => {
    let compareA = a[sortBy.value];
    let compareB = b[sortBy.value];

    if (typeof compareA === "string") {
      compareA = compareA.toLowerCase();
      compareB = compareB.toLowerCase();
    }

    if (compareA < compareB) {
      return sortDirection.value === "asc" ? -1 : 1;
    }
    if (compareA > compareB) {
      return sortDirection.value === "asc" ? 1 : -1;
    }
    return 0;
  });
});

onMounted(() => {
  fetchPhotos();
});
</script>

<template>
  <HeaderComponent :loading="loading" @search="handleSearch" />
  <TableComponent :photosList="sortedPhotosList" @sort="sortTable" />
  <FooterComponent />
</template>

<style scoped>
.main__table-container {
  max-width: 600px;
  max-height: 600px;
  overflow: auto;
  padding: 20px;
  margin: 0 auto;
}
</style>
