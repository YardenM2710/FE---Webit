<template>
  <section class="user-modal slide-in-top" v-if="hide">
    <div v-if="isLogin">
      <h3>Login</h3>
      <form>
        <el-input type="text" placeholder="user Name" v-model="cred.username" />
        <el-input
          type="password"
          placeholder="password"
          v-model="cred.password"
        />
        <div class="btn-container">
          <el-button @click.prevent="login">Login</el-button>
          <el-button @click="isGuest = true">Continue as guest</el-button>
          <p>
            Need an account? <span @click="changeIsLogin">{{ showLink }}</span>
          </p>
        </div>
      </form>
    </div>
    <div v-else>
      <h3>Signup</h3>
      <form>
        <el-input type="text" placeholder="Full name" v-model="cred.fullname" />
        <el-input type="text" placeholder="Username" v-model="cred.username" />
        <el-input
          type="password"
          placeholder="Password"
          v-model="cred.password"
        />
        <div class="btn-container">
          <el-button @click.prevent="signup">Sign Up</el-button>
          <el-button @click.prevent="signup">Continue as guest</el-button>
          <p>
            Allready got an account?
            <span @click="changeIsLogin">{{ showLink }}</span>
          </p>
        </div>
      </form>
    </div>
    <!-- <button @click="logout">Logout</button> -->
  </section>
</template>

<script>
export default {
  data() {
    return {
      isLogin: false,
      isGuest: false,
      cred: {
        fullname: '',
        username: '',
        password: '',
      },
    };
  },
  created() {},
  methods: {
    async login() {
      try {
        if (this.isGuest) {
          this.cred = {
            fullname: 'guest', //change to Guest
            username: 'guest',
            password: 'guest',
          };
        }
        const user = await this.$store.dispatch({
          type: 'login',
          cred: this.cred,
        });
        console.log('LOGGEDINUSER', user);
        if (!user) return;
        this.$store.commit({ type: 'toggleModal' });
        this.$router.push('/');
      } catch (err) {
        console.log('invalid username or password');
      }
    },
    async signup() {
      await this.$store.dispatch({ type: 'signup', cred: this.cred });
      this.$store.commit({ type: 'toggleModal' });
      this.$router.push('/');
    },
    changeIsLogin() {
      this.isLogin = !this.isLogin;
    },
  },
  computed: {
    showLink() {
      return this.isLogin ? 'Sign Up' : 'Login';
    },
    hide() {
      return this.$store.getters.getIsHide;
    },
  },
};
</script>

<style></style>
