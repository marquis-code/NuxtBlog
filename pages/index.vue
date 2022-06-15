<template>
  <div>
    <div v-for="post in allPosts" :key="post._id">
      <p>{{post.title}}</p>
       <p>{{post.description}}</p>
        <p>{{post.readingTime}}</p>
       <div v-for="tag in post.tags" :key="tag">
         <p>{{tag && tag.split('#')[1]}}</p>
       </div>
    </div>
  </div>
</template>

 <script>
export default {
  name : 'IndexPage',
  async asyncData({$axios}){
    const allPosts = await $axios.$get('https://annie-blog.herokuapp.com/api/post/')
    return {allPosts}
  }
}
</script>