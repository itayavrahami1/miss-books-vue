<template>
  <section class="user-msg" :class="msg.type" v-show="msg.txt">
      {{msg.txt}}
      <button @click="closeMsg">X</button>
  </section>
</template>

<script>
import eventBus from '@/services/eventBusService'

export default {
  data() {
    return {
      msg: {
        txt: '',
        type: '',
      },
    }
  },
  created() {
    eventBus.$on('showUserMsg', (userMsg) => {
      this.msg = userMsg
      setTimeout(() => {
        this.msg = ''
      }, 2500)
    })
  },
  destroyed() {
    eventBus.$off('showUserMsg')
  },
  methods: {
    closeMsg(){
      this.msg = ''
    }
  }
}
</script>