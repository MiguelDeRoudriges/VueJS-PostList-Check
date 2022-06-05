<template>
  <div class="app">
    <h1>Posts Page</h1>
    <my-input
    v-model="searchQuery"
    />
    <div class="app__btns">
      <my-button
          @click="showDialog"
      >
        Create Post
      </my-button>
      <my-select
          v-model="selectedSort"
          :options="sortOptions"
      />
    </div>
    <my-dialog v-model:show="dialogVisible">
      <post-form
          @create="createPost"
      />
    </my-dialog>
    <post-list
        :posts="sortedPosts"
        @remove="removePost"
        v-if="!isPostsLodaing"
    />
    <div v-else>Loading...</div>
  </div>
</template>

<script>

import PostForm from "@/components/PostForm.vue";
import PostList from "@/components/PostList.vue";

import axios from 'axios'

export default {
  components : {
    PostList, PostForm
  },
  data() {
    return {
      posts: [],
      dialogVisible: false,
      isPostsLodaing: false,
      selectedSort: '',
      sortOptions: [
        {value: 'title', name: 'By the title'},
        {value: 'body', name: 'By the description'},
      ]
    }
  },
  methods: {
    createPost(post) {
      this.posts.push(post);
      this.dialogVisible = false;
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },
    showDialog(){
      this.dialogVisible = true;
    },
    async fetchPosts() {
      try{
        this.isPostsLodaing = true;
          const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10')
          this.posts = response.data
      } catch (e) {
        alert('Error')
      } finally {
        this.isPostsLodaing = false;
      }
    }
  },
  mounted() {
    this.fetchPosts();
  },
    computed: {
    sortedPosts() {
      return [...this.posts].sort((post1,post2) =>
        post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
      )
    }
    },
  watch: {

  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app {
  padding: 20px;
}

.app__btns {
  margin: 15px 0;
  display: flex;
  justify-content: space-between;

}
</style>