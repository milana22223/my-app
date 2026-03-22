<template>
  <div class="app container py-4">
    <!-- Шапка -->
    <header class="text-center mb-5 p-4 bg-gradient rounded-3 shadow">
      <!-- 🔹 Убран класс text-white, добавлен стиль для черного текста -->
      <h1 class="display-4 mb-2 text-dark">📚 Менеджер книг</h1>
      <p class="lead text-dark">Vue 3 + Bootstrap 5</p>
    </header>
    
    <main class="bg-white p-4 rounded-3 shadow-sm">
      <AddBookForm @add-book="addBook" />
      
      <BookFilters
        v-model:searchQuery="searchQuery"
        v-model:filter="currentFilter"
        v-model:sortBy="sortBy"
        :books="books"
      />
      
      <div v-if="filteredBooks.length === 0" class="empty-state text-center py-5">
        <i class="bi bi-book display-1 text-muted"></i>
        <p class="mt-3 text-muted fs-5">Книги не найдены</p>
        <p class="text-muted">Добавьте книгу или измените параметры поиска</p>
      </div>
      
      <div v-else class="row g-4">
        <div
          v-for="book in filteredBooks"
          :key="book.id"
          class="col-md-6 col-lg-4"
        >
          <BookCard
            :book="book"
            @toggle="toggleBook(book.id)"
            @delete="deleteBook(book.id)"
            @rate="rateBook(book.id, $event)"
            @toggle-favorite="toggleFavorite(book.id)"
          />
        </div>
      </div>
    </main>
    
    <footer class="text-center mt-4 text-muted small">
      Практическое занятие 15 | Vue 3 + Bootstrap 5
    </footer>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
import AddBookForm from './components/AddBookForm.vue'
import BookFilters from './components/BookFilters.vue'
import BookCard from './components/BookCard.vue'

const books = ref([])
const savedBooks = localStorage.getItem('books')
if (savedBooks) {
  books.value = JSON.parse(savedBooks)
}

const currentFilter = ref('all')
const searchQuery = ref('')
const sortBy = ref('date')

watch(books, (newBooks) => {
  localStorage.setItem('books', JSON.stringify(newBooks))
}, { deep: true })

const addBook = (bookData) => {
  const newBook = {
    id: Date.now(),
    ...bookData,
    completed: false,
    rating: 0,
    favorite: false,
    createdAt: new Date().toISOString()
  }
  books.value.push(newBook)
}

const toggleBook = (id) => {
  const book = books.value.find(b => b.id === id)
  if (book) {
    book.completed = !book.completed
    if (!book.completed) book.rating = 0
  }
}

const toggleFavorite = (id) => {
  const book = books.value.find(b => b.id === id)
  if (book) book.favorite = !book.favorite
}

const rateBook = (id, rating) => {
  const book = books.value.find(b => b.id === id)
  if (book && book.completed) book.rating = rating
}

const deleteBook = (id) => {
  if (confirm('Удалить книгу?')) {
    books.value = books.value.filter(b => b.id !== id)
  }
}

const filteredBooks = computed(() => {
  let result = books.value
    .filter(book => {
      if (currentFilter.value === 'unread') return !book.completed
      if (currentFilter.value === 'read') return book.completed
      if (currentFilter.value === 'favorite') return book.favorite
      return true
    })
    .filter(book => {
      if (!searchQuery.value) return true
      const q = searchQuery.value.toLowerCase()
      return book.title.toLowerCase().includes(q) ||
             book.author.toLowerCase().includes(q)
    })
  
  if (sortBy.value === 'date') {
    result.sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt))
  } else if (sortBy.value === 'title') {
    result.sort((a, b) => a.title.localeCompare(b.title, 'ru'))
  } else if (sortBy.value === 'rating') {
    result.sort((a, b) => b.rating - a.rating)
  }
  
  return result
})
</script>

<style>
body {
  background: #f8f9fa;
  font-family: system-ui, -apple-system, sans-serif;
  margin: 0;
  padding: 0;
}
.app { 
  max-width: 1200px; 
}
.bg-gradient {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}
.empty-state i { 
  opacity: 0.5; 
}
</style>