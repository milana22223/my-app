<template>
  <form @submit.prevent="handleSubmit" class="add-form bg-light p-4 rounded-3 mb-4">
    <h5 class="mb-3"><i class="bi bi-plus-circle me-2"></i>Добавить книгу</h5>
    
    <div class="row g-3">
      <div class="col-md-6">
        <label class="form-label">Название *</label>
        <input
          v-model="formData.title"
          type="text"
          class="form-control"
          placeholder="Война и мир"
          required
        />
      </div>
      
      <div class="col-md-6">
        <label class="form-label">Автор *</label>
        <input
          v-model="formData.author"
          type="text"
          class="form-control"
          placeholder="Лев Толстой"
          required
        />
      </div>
      
      <div class="col-md-4">
        <label class="form-label">Жанр *</label>
        <select v-model="formData.genre" class="form-select" required>
          <option value="">Выберите жанр</option>
          <option value="Роман">Роман</option>
          <option value="Фантастика">Фантастика</option>
          <option value="Детектив">Детектив</option>
          <option value="Научная">Научная</option>
          <option value="Поэзия">Поэзия</option>
          <option value="Биография">Биография</option>
        </select>
      </div>
      
      <div class="col-md-8">
        <label class="form-label">Обложка (URL)</label>
        <input
          v-model="formData.cover"
          type="url"
          class="form-control"
          placeholder="https://example.com/cover.jpg"
        />
      </div>
      
      <div class="col-12">
        <label class="form-label">Описание</label>
        <textarea
          v-model="formData.description"
          class="form-control"
          rows="3"
          placeholder="Краткое описание..."
        ></textarea>
      </div>
      
      <div class="col-12">
        <button type="submit" class="btn btn-primary w-100">
          <i class="bi bi-plus-lg me-2"></i>Добавить
        </button>
      </div>
    </div>
  </form>
</template>

<script setup>
import { reactive } from 'vue'

const emit = defineEmits(['add-book'])

const formData = reactive({
  title: '',
  author: '',
  genre: '',
  cover: '',
  description: ''
})

const handleSubmit = () => {
  emit('add-book', { ...formData })
  formData.title = ''
  formData.author = ''
  formData.genre = ''
  formData.cover = ''
  formData.description = ''
}
</script>

<style scoped>
.add-form { 
  border: 1px solid #dee2e6; 
}
.form-control:focus,
.form-select:focus {
  border-color: #667eea;
  box-shadow: 0 0 0 0.25rem rgba(102, 126, 234, 0.25);
}
</style>