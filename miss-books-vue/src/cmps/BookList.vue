<template>
  <section class="book-list-sec">
    <h3>Library</h3>
    <ul class="book-list clean-list">
      <li v-for="currBook in books" :key="currBook._id">
        <BookPreview :book="currBook" />
      </li>
    </ul>
  </section>
</template>

<script>
import BookPreview from "@/cmps/BookPreview.vue";

export default {
  props: ["books"],
  components: {
      BookPreview,
  }
};
</script>