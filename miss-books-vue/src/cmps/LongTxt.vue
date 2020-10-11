<template>
  <section class="book-desc flex column">
    {{ descToShow }}
    <button v-if="desc.length > 100" @click="toggleBtnTxt">
      {{ getBtnTxt }}
    </button>
  </section>
</template>

<script>
export default {
  props: ['desc'],
  data() {
    return {
      isReadMore: true,
    }
  },
  computed: {
    getBtnTxt() {
      const btnTxt = this.isReadMore ? 'Read More' : 'Read Less'
      return btnTxt
    },
    descToShow(){
        let textToShow = this.desc;
        if (this.isReadMore) textToShow = textToShow.slice(0,99) + '...'
        return textToShow

    }
  },
  methods: {
    toggleBtnTxt() {
      this.isReadMore = !this.isReadMore
    },
  },
}
</script>