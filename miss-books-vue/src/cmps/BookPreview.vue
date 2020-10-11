<template>
  <section class="book-preview">
    <router-link class="clean-link" :to="`/book/${book._id}`">
      <article className="book-preview">
        <img :src="book.thumbnail" alt="" />
        <h3>{{ book.title }}</h3>
        <h4>Price: {{ priceToShow }}</h4>
      </article>
    </router-link>
  </section>
</template>

<script>
import { utilService } from "@/services/utils.js";
export default {
    props: ['book'],
    computed: {
        priceToShow(){
            return utilService.getPrice(this.book.listPrice.amount, this.book.listPrice.currencyCode)
        }
    }
}
</script>