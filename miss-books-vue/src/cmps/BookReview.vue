<template>
  <section class="book-review-cmp">
    <h2>Reviews</h2>
    <!-- <section v-if="reviews" class="book-reviews">
      Old reviews: {{ reviews.length }}
    </section> -->
    <ul v-if="reviews" class="book-reviews clean-list">
      <review-preview
        v-for="(currReview, idx) in reviews"
        :key="idx"
        :review="currReview"
      />
    </ul>
    <section v-if="newReview" class="add-review">
      <form autocomplete="off" @submit.prevent="saveReview" class="flex column">
        <label>Name: </label>
        <input
          v-model="newReview.name"
          ref="review-name"
          type="text"
          name="name"
          placeholder="Reader's Name"
        />
        <label>Rate: </label>
        <select name="rate" v-model.number="newReview.rate">
          <option value="" disabled selected>Rate</option>
          <option value="1">1</option>
          <option value="2">2</option>
          <option value="3">3</option>
          <option value="4">4</option>
          <option value="5">5</option>
        </select>
        <label>Read At: </label>
        <input v-model="newReview.readAt" type="date" name="date" />
        <label>Review: </label>
        <input v-model="newReview.freeTxt" type="text" name="freeTxt" />
        <button>Save</button>
      </form>
    </section>
  </section>
</template>

<script>
import { bookService } from '@/services/bookService.js'
import ReviewPreview from '@/cmps/ReviewPreview.vue'

export default {
  props: ['bookReviews'],
//   props: ['reviews'],
  data() {
    return {
      reviews: this.bookReviews,
      newReview: null,
    }
  },
  created() {
    this.newReview = this.getEmptyReview()
  },
  mounted() {
    this.$refs['review-name'].focus()
  },
  methods: {
    getEmptyReview() {
      return { name: '', rate: '', readAt: '', freeTxt: '' }
    },
    saveReview() {
      this.$emit('addReview', this.newReview)
      this.newReview = this.getEmptyReview()
      this.$refs['review-name'].focus()
    },
  },
  components: {
    ReviewPreview,
  },
}
</script>