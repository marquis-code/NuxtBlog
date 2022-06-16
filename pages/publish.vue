<template>
    <div class="container col-md-6 offset-md-3 my-1">
      <br />
      <h1 class="">Publish new post</h1>
       <p class="errorText">{{error ? error : ''}}</p>
       <p class="errorText">{{message ? message : ''}}</p>
      <b-form
        @submit.prevent="handlePost"
      >
       <b-form-group class="form-group">
          <label 
            >Blog Title</label
          >
          <b-form-input
            type="text"
            v-model="form.title"
            class="form-control"
            placeholder="Enter blog title"
          ></b-form-input>
        </b-form-group>

        <b-form-group class="form-group">
          <label
            >Blog Subtitle</label
          >
          <b-form-input
            type="text"
            v-model="form.subTitle"
            class="form-control"
            placeholder="Enter blog subtitle"
          ></b-form-input>
        </b-form-group>

         <b-form-group class="form-group">
          <label
            >Blog reading time</label
          >
          <b-form-input
            type="number"
            v-model="form.readingTime"
            class="form-control"
            placeholder="Enter blog reading time"
          ></b-form-input>
        </b-form-group>

       <b-form-group class="form-group">
          <label 
            >Blog description</label
          >
          <b-form-textarea
          v-model="form.description"
          placeholder="Enter your story here"
          style="resize : none"
          rows="10"
        ></b-form-textarea>
        </b-form-group>
        <b-form-group>
        <label
            >Blog Hash Tags</label
          >
        <b-form-input
          v-model="form.hashTag"
          placeholder="Enter blog hash tags"
          type="text"
          @keyup.alt="addHashTag"
        ></b-form-input>
         <div class="tagsContainer">
            <div v-for="tag in form.tags" :key="tag">
             <div class="tag">{{tag && tag.split('#')[1]}}</div>
        </div>
         </div>
            <b-form-text>
             Post hash tags must begin with # and ensure you press ALT + COMMA after every hash tag
         </b-form-text>
      </b-form-group>

       <b-button type="submit" block variant="primary">{{loading ? 'Publishing blog...' : 'Publish'}}</b-button>
      </b-form>
    </div>
</template>

<script>
  export default {
    data() {
      return {
        loading: false,
        error : null,
        successMessage : '',
        form: {
          title : '',
          subTitle : '',
          email: '',
          readingTime : '',
          description : '',
          hashTag : '',
          tags : []
        },
      }
    },
    methods: {
        addHashTag(e){
           if(e.key === "," && this.form.hashTag.startsWith('#') && !this.form.hashTag.endsWith(',') && this.form.hashTag){
             if(!this.form.tags.includes(this.form.hashTag))
            this.form.tags.push(this.form.hashTag);
            this.form.hashTag = ''
           }
        },
        async handlePost(){
          const {title, subTitle, readingTime, description, tags } = this.form;
          if(!title || !subTitle || !readingTime || !description || !tags){
            this.error = 'Please enter all fields information'
          } else {
            let blogData = { title, subTitle, description, readingTime, tags }

            try {
                   const response = await this.$axios.post("https://annie-blog.herokuapp.com/api/post", { title, subTitle, description, readingTime, tags })
                   this.successMessage = response.data.successMessage;
                   this.$router.push('/')
                   this.form.title = '';
                   this.form.subTitle = '';
                   this.form.email= '';
                   this.form.readingTime = '';
                   this.form.description = '';
                   this.form.hashTag = '';
                   this.form.tags = [];
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
    font-family: cursive;
    font-weight: bold;
    font-size: x-large;
}

p {
    text-align: center;
    padding: 10px;
}

label {
    font-weight: 500;
}

.errorText {
  color: red;
  font-size: medium;
}

.tagsContainer{
  margin-top: 3px;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
}

  .tag {
     border: 1px solid #ccc;
     font-size: small;
     padding-top: 2px;
      padding-bottom: 2px;
     padding-right: 5px;
     padding-left: 5px;
     border-radius: 5px;
     margin: 2px;
  }
</style>
