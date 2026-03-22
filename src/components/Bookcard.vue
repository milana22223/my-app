<template>
  <div class="card h-100 shadow-sm book-card bg-white" :class="{
    'border-success': book.completed,
    'border-danger': book.favorite
  }">
    <div 
      v-if="book.cover" 
      class="card-img-top" 
      :style="{ 
        backgroundImage: `url(${book.cover})`,
        backgroundSize: 'cover',
        backgroundPosition: 'center',
        minHeight: '200px'
      }"
    ></div>
    <div v-else class="card-img-top bg-secondary d-flex align-items-center justify-content-center" style="min-height: 200px;">
      <i class="bi bi-book text-white display-4"></i>
    </div>
    
    <div class="card-body">
      <div class="d-flex justify-content-between align-items-start mb-2">
        <h5 class="card-title mb-0 text-dark">{{ book.title }}</h5>
        <button
          @click="$emit('toggle-favorite')"
          class="btn btn-link p-0 border-0"
          :class="book.favorite ? 'text-danger' : 'text-muted'"
          title="Избранное"
        >
          <i :class="book.favorite ? 'bi bi-heart-fill' : 'bi bi-heart'" class="fs-5"></i>
        </button>
      </div>
      
      <p class="card-text text-muted mb-1">
        <i class="bi bi-person me-1"></i>{{ book.author }}
      </p>
      
      <span class="badge bg-secondary mb-2">{{ book.genre }}</span>
      
      <p v-if="book.description" class="card-text small text-muted">
        {{ book.description }}
      </p>
      
      <div v-if="book.completed" class="rating mb-2">
        <span
          v-for="star in 5"
          :key="star"
          @click="$emit('rate', star)"
          class="star"
          :class="star <= book.rating ? 'text-warning' : 'text-muted'"
        >
          <i :class="star <= book.rating ? 'bi bi-star-fill' : 'bi bi-star'"></i>
        </span>
        <span class="ms-2 small">({{ book.rating }}/5)</span>
      </div>
    </div>
    
    <div class="card-footer bg-white border-0 d-flex gap-2">
      <button
        @click="$emit('toggle')"
        :class="['btn', 'flex-grow-1', book.completed ? 'btn-outline-success' : 'btn-success']"
      >
        <i :class="book.completed ? 'bi bi-check-lg' : 'bi bi-book'"></i>
        {{ book.completed ? 'Прочитано' : 'Читать' }}
      </button>
      <button @click="$emit('delete')" class="btn btn-outline-danger">
        <i class="bi bi-trash me-1"></i>Удалить
      </button>
    </div>
  </div>
</template>

<script setup>
defineProps(['book'])
defineEmits(['toggle', 'delete', 'rate', 'toggle-favorite'])
</script>

<style scoped>
.book-card {
  transition: transform 0.2s, box-shadow 0.2s;
}
.book-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 25px rgba(0,0,0,0.15) !important;
}
.star {
  cursor: pointer;
  font-size: 1.2em;
  transition: transform 0.2s;
}
.star:hover { 
  transform: scale(1.2); 
}
</style>