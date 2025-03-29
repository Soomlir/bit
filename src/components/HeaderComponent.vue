<script setup>
import { ref } from "vue";

const props = defineProps({
  loading: {
    type: Boolean,
    default: false,
  },
});

const albumIds = ref("");
const emit = defineEmits();

const searchPhotos = () => {
  const ids = albumIds.value.split(" ").filter(id => id.trim() !== "");

  if (ids.length > 0) {
    emit("search", ids);
  } else {
    emit("search", []);
  }
};
</script>

<template>
  <header class="header">
    <h1 class="header__heading">Bit - Test task</h1>
    <div class="header__wrapper">
      <input
        class="header__search"
        type="search"
        v-model="albumIds"
        placeholder="Введите ID альбомов (через пробел)"
      />
      <button
        class="header__button"
        type="button"
        @click="searchPhotos"
        :disabled="props.loading"
      >
        <span v-if="props.loading">Загрузка...</span>
        <span v-else>Поиск</span>
      </button>
    </div>
  </header>
</template>

<style scoped>
.header {
  margin-bottom: 20px;
}

.header__wrapper {
  display: flex;
  align-items: stretch;
}

.header__heading {
  font-size: 40px;
}

.header__search {
  padding: 5px;
  width: 350px;
}

.header__button {
  width: 70px;
}

.header__button[disabled] {
  cursor: not-allowed;
  opacity: 0.5;
}
</style>
