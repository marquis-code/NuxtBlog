<template>
    <div>
  <b-navbar toggleable="lg" type="dark" variant="info">
    <b-navbar-brand href="#"><nuxt-link to="/">Marquis Blog</nuxt-link></b-navbar-brand>
    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <!-- Right aligned nav items -->
      <b-navbar-nav class="ml-auto">
         <b-dropdown-item><nuxt-link to="/publish">Write</nuxt-link></b-dropdown-item>
          <b-dropdown-item><nuxt-link to="/login">Login</nuxt-link></b-dropdown-item>
          <b-dropdown-item><nuxt-link to="/register">Register</nuxt-link></b-dropdown-item>
        <b-nav-item-dropdown right>
          <!-- Using 'button-content' slot -->
          <template #button-content>
            <span>Welcome {{userName}} :)</span>
          </template>
          <b-dropdown-item><button @click="handleLogout">Logout</button></b-dropdown-item>
        </b-nav-item-dropdown>
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
</div>
</template>

 <script>
export default {
  name : 'NavBar',
  data(){
    return {
      userName : ''
    }
  },

  methods : {
    fetchUser(){
        if(process.browser){
          this.userName = localStorage.getItem('firstName')
        }
    },
    async handleLogout(){
       localStorage.removeItem('firstName');
       await this.$auth.logout();
    }
  },

  mounted() {
     this.fetchUser();
  }
}
</script>