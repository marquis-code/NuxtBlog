<template>
  <div>
    <b-container class="blogContainer">
      <b-row>
        <b-col class="item">
          <h1>{{ eachPost.title }}</h1>
           <p class="readingTime">{{ eachPost.readingTime }} mins read</p>
          <p class="readingTime">{{ eachPost.subTitle}}</p>
           <p class="postDescription">{{ eachPost.description }}</p>
          <div class="tags">
             <div v-for="tag in eachPost.tags" :key="tag">
                <p class="tag">{{ tag && tag.split("#")[1] }}</p>
              </div>
          </div>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
export default {
  name: "IndexPage",
  async asyncData({ $axios, params}) {
     const eachPost = await $axios.$get(
      `https://annie-blog.herokuapp.com/api/post/${params.postId}`
    );
    return {eachPost}
  },
};
</script>

<style scoped>
    h1{
        font-size: x-large;
        font-weight: bolder;
    }

     .tags {
    display: flex;
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

  .readingTime {
    font-size: small;
  }

  .blogContainer {
    margin-top: 10px;
  }
</style>