<template>
  <div>
    <b-container class="blogContainer">
      <b-row  v-for="post in allPosts" :key="post._id">
        <b-col class="item">
          <h1>{{ post.title.length > 30 ? `${post.title.slice(0, 30)}...` : post.title}}</h1>
          <nuxt-link :to="`/${post._id}`"><p class="postDescription">{{ post.description.length > 30 ? `${post.description.slice(0, 30)}...read more` : post.description }}</p></nuxt-link>
          <p class="readingTime">{{ post.readingTime }} mins read</p>
          <div class="tags">
             <div v-for="tag in post.tags" :key="tag">
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
  async asyncData({ $axios }) {
    const allPosts = await $axios.$get(
      "https://annie-blog.herokuapp.com/api/post/"
    );
    return { allPosts };
  },
};
</script>

<style scoped>
  .blogContainer {
   /* margin: 10px; */
   display: flex;
   flex-wrap: wrap;
   align-items: center;
  }

  .item {
    border: 1px solid #ccc;
    display: flex;
    flex-direction: column;
    box-shadow: 2px 2px 6px 0px rgba(0, 0, 0, 0.4);
    padding: 10px;
    margin-right: 50px;
    margin-left: 20px;
    margin-top: 20px;
    margin-bottom: 20px;
    width: 100%;
  }

  h1 {
    font-size: medium;
    font-weight: bold;
  }

  .postDescription {
    font-size: small;

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
</style>
