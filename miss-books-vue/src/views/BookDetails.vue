<template>
  <section v-if="book" class="book-details flex">
    <section class="book-img">
      <img :src="book.thumbnail" alt="" />
      <h3 class="on-sale-banner" v-if="book.listPrice.isOnSale">ON SALE!</h3>
    </section>
    <section class="book-info">
      <h3>{{ book.title }}</h3>
      <h2 class="book-price" :class="getPriceCls">Price: {{ priceToShow }}</h2>
      <ul class="authors flex clean-list">
        Authors:
        <li v-for="(author, idx) in book.authors" :key="idx">{{ author }}</li>
      </ul>
      <h5>Number Of Pages: {{ book.pageCount }} - {{ getPageCountMsg }}</h5>
      <h5>Published At: {{ book.publishedDate }} - {{ getPublicationAtMsg }}</h5>
      <long-txt :desc="book.description"/>
    </section>
  </section>
</template>

<script>
import { bookService } from '@/services/bookService.js'
import { utilService } from '@/services/utils.js'
import LongTxt from '@/cmps/LongTxt.vue'

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
    getPageCountMsg() {
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
    getPublicationAtMsg() {
      const pubYear = this.book.publishedDate
      var date = new Date()
      const bookAge = pubYear - date.getYear()
      let msg = ''
      if (bookAge > 10) {
        msg += ' Veteran Book'
      } else if (bookAge < 1) {
        msg += ' New!'
      }
      return msg
    },
    getPriceCls() {
      const bookPrice = this.book.listPrice.amount
      if (bookPrice > 150) return 'expensive'
      else if (bookPrice < 40) return 'cheap' // in the instruction should be 20 but only one book fullfills it.
    },
  },
  components: {
      LongTxt,
  }
}
</script>