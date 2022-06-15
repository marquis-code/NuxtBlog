<template>
    <div class="container col-md-6 offset-md-3 my-1">
      <br />
      <h1 class="">Create an account</h1>
      <p class="errorText">{{error ? error : ''}}</p>
      <b-form
        @submit.prevent="handleUser"
      >
       <b-form-group class="form-group">
          <label
            >First Name</label
          >
          <b-form-input
            type="text"
            v-model="form.firstName"
            class="form-control"
            placeholder="Enter First Name"
          ></b-form-input>
        </b-form-group>

         <b-form-group class="form-group">
          <label
            >Last Name</label
          >
          <b-form-input
            type="text"
            v-model="form.lastName"
            class="form-control"
            placeholder="Enter Last Name"
          ></b-form-input>
        </b-form-group>

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

        <b-form-group id="input-group-3" class="form_label my-4"  label-for="input-3">
        <label
            >Speciality</label
          >
        <b-form-select
          id="input-3"
          v-model="form.intrest"
          :options="intrestList"
        ></b-form-select>
      </b-form-group>

        <b-button type="submit" block variant="primary">{{loading ? 'Loading...' : 'Continue'}}</b-button>
      </b-form>
      <p>Already have an account? <nuxt-link to="/login">Login</nuxt-link></p>
    </div>
</template>

<script>
  export default {
    data() {
      return {
        loading: false,
        error : null,
        form: {
          firstName: '',
          lastName : '',
          email : '',
          password : '',
          intrest: '',
        },
        intrestList: [
          { text: 'Select One', value: null}, 
            'technology', 'fashion and lifestyle', 'politics', 'business','medicine'
        ],
      }
    },
    methods: {
        async handleUser(){
          const {firstName, lastName, email,password, intrest} = this.form;
          if(!firstName || !lastName || !email || !password || !intrest){
            this.error = 'Please enter all fields information'
          }else {
            this.loading = true
              try {
                let response = await this.$axios.post('/auth/signup', {
                    firstName,
                    lastName,
                    email,
                    password,
                    intrest
                })
              console.log(response)
                if(response.status === 201) {
                    this.$router.push('/login')
                }else {
                    console.log('something went wrong')
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
