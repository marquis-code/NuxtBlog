<template>
    <div class="col-md-6 offset-md-3 my-1">
      <br />
      <h1 class="">Login to your account</h1>
       <p class="errorText">{{error ? error : ''}}</p>
      <b-form
        @submit.prevent="login"
      >
        <b-form-group class="form-group">
          <label
            >Email address</label
          >
          <b-form-input
            type="email"
            v-model="form.email"
            class="form-control"
            placeholder="Enter email"
          ></b-form-input>
        </b-form-group>

         <b-form-group class="form-group">
          <label
            >Password</label
          >
          <b-form-input
            type="password"
            v-model="form.password"
            class="form-control"
            placeholder="Enter password"
          ></b-form-input>
        </b-form-group>
         <b-button type="submit" block variant="primary">{{loading ? 'Loading...' : 'Login'}}</b-button>
      </b-form>
       <p>Dont have an account? <nuxt-link to="/register">Create account</nuxt-link></p>
    </div>
</template>

<script>
  export default {
    data() {
      return {
        loading: false,
        error : null,
        form: {
          email : '',
          password : ''
        }
      }
    },
    methods: {
       async login(){
         const {email,password} = this.form;
        if(!email || !password){
            this.error = 'Please enter all fields information'
        } else {
              this.loading = true
              try {
                let response = await this.$auth.loginWith('local', {
                   data : {
                     email : this.form.email,
                     password : this.form.password
                   }
                })
                  console.log(response)

                if(process.browser) {
                    localStorage.setItem('firstName', response.data.firstName)
                    this.$auth.setUser(response.data)
                    this.$router.push('/')
                }
            } catch (error) {
                this.error = error.response.data.errorMessage
            }
        }
    }
  }

  }
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

h1 {
    text-align: center;
}

p {
    text-align: center;
    padding: 10px;
}

h1 {
    text-align: center;
    font-family: cursive;
    font-weight: bold;
    font-size: x-large;
}

label {
    font-weight: 500;
}

.errorText {
  color: red;
  font-size: medium;
}
</style>
