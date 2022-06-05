<template>
  <form @submit.prevent>
    <h4>Create post</h4>
    <my-input
        v-model="post.title"
        type="text"
        placeholder="Title"
    />
    <my-input
        v-model="post.body"
        type="text"
        placeholder="Description"
    />
    <MyButton
        @click="createPost"
        style="align-self: flex-end; margin-top: 15px"
    >Create
    </MyButton>
  </form>
</template>

<script>
import MyButton from "./UI/MyButton.vue";
import MyInput from "./UI/MyInput.vue";
export default {
  components: {
    MyInput,
    MyButton
  },
  data() {
    return {
      post: {
        title: '',
        body: ''
      }
    }
  },
  methods: {
    createPost() {
        this.post.id = Date.now()
        this.$emit('create', this.post)
        this.post = {
          title: '',
          body: ''
        }
    }
  },

}
</script>

<style scoped>

form {
  display: flex;
  flex-direction: column;
}
</style>