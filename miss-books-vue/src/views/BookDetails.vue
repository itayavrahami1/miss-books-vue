<template>
  <section v-if="book" class="book-details-cmp">
    <section class="book-details flex">
      <section class="book-img">
        <img :src="book.thumbnail" alt="" />
        <span v-if="book.listPrice.isOnSale" class="on-sale-banner"
          >On sale</span
        >
      </section>
      <book-details-info :book="book" />
      <section class="book-details-btns flex column">
        <button>
          <router-link class="clean-link" :to="`/book`">Back</router-link>
        </button>
        <button @click="removeBook">Remove</button>
      </section>
    </section>
    <book-review :bookReviews="book.reviews" @addReview="addReview" />
  </section>
</template>

<script>
import { bookService } from '@/services/bookService.js'
import eventBus from '@/services/eventBusService.js'
import LongTxt from '@/cmps/LongTxt.vue'
import BookDetailsInfo from '@/cmps/BookDetailsInfo.vue'
import BookReview from '@/cmps/BookReview.vue'

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
  components: {
    LongTxt,
    BookDetailsInfo,
    BookReview,
  },
  methods: {
    async removeBook() {
      try {
        await bookService.remove(this.book._id)
        eventBus.$emit('showUserMsg', {
          txt: `The book "${this.book.title}" was successefuly removed`,
          type: 'success',
        })
      } catch (err) {
        eventBus.$emit('showUserMsg', {
          txt: `failed to removed`,
          type: 'fail',
        })
      } finally {
        this.$router.push('/book')
      }
    },
    async addReview(review) {
      try {
        await bookService.addReview(this.book._id, review)
        eventBus.$emit('showUserMsg', {
          txt: `Review was successefuly added`,
          type: 'success',
        })
      } catch (err) {
        eventBus.$emit('showUserMsg', {
          txt: `Can't add review`,
          type: 'fail',
        })
      }
    },
  },
}
</script>