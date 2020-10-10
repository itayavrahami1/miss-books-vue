<template>
  <section v-if="user" class="edit-user">
    <form @submit.prevent="saveUser">
      <input
        ref="nameEditInput"
        type="text"
        v-model="user.name"
        placeholder="Enter username"
      />
      <button>Save</button>
    </form>
    <router-link class="clean-link" :to="backURL">
      <button>Back</button></router-link
    >
  </section>
</template>

<script>
import { userService } from "@/services/userService";

export default {
  data() {
    return {
      user: undefined,
      backURL: undefined,
    };
  },
  async created() {
    const userId = this.$route.params.userId;
    const user = userId
      ? await userService.getById(userId)
      : await userService.getEmptyUser();
    const URL = userId ? `/whoswatch/${userId}` : "/whoswatch";
    this.user = user;
    this.backURL = URL;
  },
  updated() {
    this.$refs.nameEditInput.focus();
  },
  methods: {
    async saveUser() {
      const savedUser = await userService.save(this.user);
      this.user = {
        name: "",
      };
      this.$router.push("/whoswatch");
    },
  },
};
</script>