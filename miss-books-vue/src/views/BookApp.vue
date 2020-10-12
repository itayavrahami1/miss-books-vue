<template>
  <section v-if="books" class="book-app">
    <book-filter @filtered="setFilter" />
    <book-list :books="booksToShow" @onBook="chooseBook"/>
  </section>
  <div v-else class="loading flex justify-center align-center">
      <img src="@/assets/img/loading.svg" />
    </div>
</template>

<script>
import { bookService } from '@/services/bookService.js'
import BookList from '@/cmps/BookList.vue'
import BookFilter from '@/cmps/BookFilter.vue'

export default {
  data() {
    return {
      books: null,
      filterBy: {
        title: '',
        min: 0,
        max: Infinity,
      },
    }
  },
  async created() {
    const books = await bookService.query()
    this.books = books
  },
  methods: {
    setFilter(filterBy) {
      this.filterBy = filterBy
      this.filterBy.max = this.filterBy.max || Infinity;
    },
    chooseBook(bookId){
      this.$router.push(`/book/${bookId}`)
    }
  },
  computed: {
    booksToShow() {
      const filterForRegEx = (this.filterBy.title.length) ? `${this.filterBy.title}\w*`:''
      const filterRegEx = new RegExp(filterForRegEx, 'i')

      return this.books.filter((book) => {
        return filterRegEx.test(book.title) && book.listPrice.amount > this.filterBy.min && book.listPrice.amount < this.filterBy.max
      })
    },
  },
  components: {
    BookList,
    BookFilter,
  },
}
</script>