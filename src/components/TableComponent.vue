<script setup>
const props = defineProps({
  photosList: {
    type: Array,
    default: () => [],
  },
});

const emit = defineEmits();

const handleSort = (column) => {
  emit("sort", column);
};
</script>

<template>
  <div class="main__table-container">
    <table class="main__table">
      <thead>
        <tr>
          <th @click="handleSort('id')">id</th>
          <th @click="handleSort('albumId')">albumId</th>
          <th @click="handleSort('title')">title</th>
          <th @click="handleSort('url')">url</th>
          <th @click="handleSort('thumbnailUrl')">thumbnailUrl</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in photosList" :key="item.id">
          <td :title="item.id">{{ item.id }}</td>
          <td :title="item.albumId">{{ item.albumId }}</td>
          <td :title="item.title">{{ item.title }}</td>
          <td><a :href="item.url" target="_blank" :title="item.url">{{ item.url }}</a></td>
          <td><a :href="item.thumbnailUrl" target="_blank" :title="item.thumbnailUrl">{{ item.thumbnailUrl }}</a></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
.main__table-container {
  overflow: auto; 
  border: 1px solid #dddddd;
  border-radius: 8px;
}

.main__table {
  max-width: 600px;
  max-height: 400px;
  width: 100%; 
  border-collapse: collapse;
  table-layout: fixed;
}

th, td {
  padding: 5px;
  text-align: left;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  width: 20%;
}

.sticky-header {
  position: sticky;
  top: 0;
  background-color: #f1f1f1;
  z-index: 10;
  border-top: 2px solid #dddddd;
}

th {
  font-weight: bold;
  color: #333333;
}

td {
  color: #666666;
}

tbody tr:nth-child(even) {
  background-color: #f9f9f9;
}

tbody tr:hover {
  background-color: #eaeaea;
}

.skeleton {
  display: flex;
  flex-direction: column;
}

.skeleton-row {
  height: 40px;
  background-color: #e0e0e0;
  margin-bottom: 10px;
  border-radius: 4px;
  animation: skeleton 1.5s infinite linear;
}

@keyframes skeleton {
  0% {
    background-color: #e0e0e0;
  }
  50% {
    background-color: #dcdcdc;
  }
  100% {
    background-color: #e0e0e0;
  }
}
</style>
