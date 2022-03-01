<template>
  <section class="user-details">
    <div class="user-detail-bar">
      <div class="user-details-title">Dashboard</div>
      <div class="user-profile">
        <div class="user-name">
          <p>{{ showUser }}</p>
        </div>
        <div @click="goHome" class="user-actions">Home</div>
        <div @click="goToTemplates" class="user-actions">Templates</div>
        <div @click="logout" v-if="user" class="user-actions logout">
          Log Out
        </div>
        <div v-else @click="toggleModal" class="user-actions">Log In</div>
      </div>
    </div>
    <div class="user-main-container">
      <h3 v-if="user" class="title">Your Sites</h3>
      <h3 v-else class="title">No Sites Yet</h3>

      <template v-if="user">
        <user-list :waps="waps"></user-list>
        <!-- <div v-for="wap in waps" :key="wap._id">
          <p>{{ wap.name }}</p>
        </div> -->
      </template>
      <userLogin></userLogin>
    </div>
  </section>
</template>

<script>
import userLogin from '../cmps/user-login-modal.cmp.vue';
import userList from '../cmps/user-wap-list.cmp.vue';

export default {
  components: {
    userLogin,
    userList,
  },
  data() {
    return {
      userName: null,
      user: null,
      waps: null,
    };
  },
  async created() {
    this.user = this.$store.getters.getUser;
    this.waps = this.user
      ? await this.$store.dispatch({ type: 'getUserWaps', user: this.user })
      : null;
  },
  methods: {
    async logout() {
      await this.$store.dispatch({ type: 'logout' });
      this.$router.push('/');
    },
    toggleModal() {
      this.$store.commit('toggleModal');
    },
    goHome() {
      this.$router.push('/');
    },
    goToTemplates() {
      this.$router.push('/template');
    },
  },
  computed: {
    showUser() {
      this.userName = this.$store.getters.getUser;
      return this.userName ? this.userName.username : 'Guest';
    },
  },
};
</script>

<style></style>
