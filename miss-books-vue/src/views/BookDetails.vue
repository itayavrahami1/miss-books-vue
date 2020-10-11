<template>
  <section class="book-details flex">
    <section class="book-img">
      <img :src="book.thumbnail" alt="" />
    </section>
    <section v-if="book" class="book-info">
      <h3>{{ book.title }}</h3>
      <h2>Price: {{ priceToShow }}</h2>
      <ul class="authors clean-list">
        Authors:
        <li v-for="(author, idx) in book.authors" :key="idx">{{ author }}</li>
      </ul>
      <h5>Number Of Pages: {{ book.pageCount }} - {{ pageCountMsg }}</h5>
      <h5>Published At:{yearOfPublication} {this.publicationAtMsg}</h5>
    </section>
  </section>
</template>

<script>
import { bookService } from '@/services/bookService.js'
import { utilService } from '@/services/utils.js'

export default {
  data() {
    return {
      book: null,
    }
  },
  async created() {
    const bookId = this.$route.params.id
    this.book = await bookService.getById(bookId)
  },
  computed: {
    priceToShow() {
      return utilService.getPrice(
        this.book.listPrice.amount,
        this.book.listPrice.currencyCode
      )
    },
    pageCountMsg() {
      const numOfPages = this.book.pageCount
      var msg = ''
      if (numOfPages > 500) {
        msg += 'Long reading'
      } else if (numOfPages <= 500 && numOfPages > 200) {
        msg += 'Decent reading'
      } else if (numOfPages < 100) {
        msg += 'Light reading'
      }
      return msg
    },
  },
}
</script>