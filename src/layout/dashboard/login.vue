<template>
  <div class="wrapper container">
     <form @submit="login()" class="form-signin text-center" style="padding:2%">
        <h1>Aeturnum Login</h1>
        <div>
          <input type="string" placeholder="Username" v-model="username"  class="form-control">
        </div>
        <div>
          <input type="password" placeholder="Password" v-model="password" class="form-control">
        </div>
        <div v-if="error">
          {{error}}
        </div>
        <br>
        <div v-if="success" id="success">
          Logged in Successfully
        </div>
        <div class="text-center">
          <button type="submit" class="btn btn-lg btn-primary btn-block" style="width:40%">Submit</button>
        </div>
      </form>
  </div>
</template>
<style lang="scss">
</style>
<script>
import TopNavbar from "./TopNavbar.vue";
import ContentFooter from "./ContentFooter.vue";
import DashboardContent from "./Content.vue";
import MobileMenu from "./MobileMenu";
export default {
  components: {
    TopNavbar,
    ContentFooter,
    DashboardContent,
    MobileMenu
  },
  data: () => ({
      username: '',
      password: '',
      error: null,
      success: false
    }),
  methods: {
      login: async function() {
        const params = new URLSearchParams()
        params.append('username', this.username);
        params.append('password', this.password);
        this.success = false;
        this.error = null;
        this.axios.post('/auth/login', params)
        .then(res => {
          console.log(res.data)
          localStorage.setItem("Authorization", "bearer "+res.data['access_token'])
          localStorage.setItem("username", this.username)
          this.success = true;
          this.$router.push('/')
        })
        .catch(err => {
          this.error = err.message
        })
      }
    },
};
</script>
