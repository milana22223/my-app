<template>
  <div class="filters bg-light p-4 rounded-3 mb-4">
    <div class="row g-3 align-items-end">
      <div class="col-md-4">
        <label class="form-label"><i class="bi bi-search me-1"></i>Поиск</label>
        <input
          v-model="searchQuery"
          type="text"
          class="form-control"
          placeholder="Название или автор..."
        />
      </div>
      
      <div class="col-md-3">
        <label class="form-label"><i class="bi bi-funnel me-1"></i>Фильтр</label>
        <select v-model="filter" class="form-select">
          <option value="all">Все книги</option>
          <option value="unread">Непрочитанные</option>
          <option value="read">Прочитанные</option>
          <option value="favorite">⭐ Избранные</option>
        </select>
      </div>
      
      <div class="col-md-3">
        <label class="form-label"><i class="bi bi-sort-down me-1"></i>Сортировка</label>
        <select v-model="sortBy" class="form-select">
          <option value="date">По дате</option>
          <option value="title">По названию</option>
          <option value="rating">По рейтингу</option>
        </select>
      </div>
      
      <div class="col-md-2">
        <div class="stats text-center">
          <div class="fs-4 fw-bold text-primary">{{ total }}</div>
          <div class="small text-muted">всего</div>
        </div>
      </div>
    </div>
    
    <div class="stats-bar mt-3 pt-3 border-top">
      <div class="row text-center">
        <div class="col-4">
          <span class="badge bg-success">{{ completed }}</span>
          <span class="ms-2 small">Прочитано</span>
        </div>
        <div class="col-4">
          <span class="badge bg-warning text-dark">{{ total - completed }}</span>
          <span class="ms-2 small">Осталось</span>
        </div>
        <div class="col-4">
          <span class="badge bg-danger">{{ favorites }}</span>
          <span class="ms-2 small">Избранные</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps(['filter', 'books', 'sortBy'])
defineEmits(['update:filter', 'update:sortBy'])

const searchQuery = defineModel('searchQuery')
const filter = defineModel('filter')
const sortBy = defineModel('sortBy')

const total = computed(() => props.books.length)
const completed = computed(() => props.books.filter(b => b.completed).length)
const favorites = computed(() => props.books.filter(b => b.favorite).length)
</script>

<style scoped>
.filters { 
  border: 1px solid #dee2e6; 
}
.stats-bar {
  background: rgba(255,255,255,0.5);
  border-radius: 8px;
}
</style>