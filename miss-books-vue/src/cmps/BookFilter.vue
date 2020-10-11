<template>
  <section class="book-filter">
    <input type="text" v-model="filterBy.title" @input="setFilter" placeholder="Search a book.."/>
  </section>
</template>

<script>
export default {
  data() {
    return {
      filterBy: {
        title: '',
      },
    }
  },
  methods: {
      setFilter(){
          this.$emit('filtered', this.filterBy)
      }
  }
}
</script>