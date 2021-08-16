<template>
  <section class="m-2 p-2">
    <div><h1 class="text-center">Create a new post</h1></div>
    <div class="container flex justify-center">
      <section class="w-1/2">
        <b-field label="Title">
          <b-input v-model="title"></b-input>
        </b-field>

        <b-field label="Subtitle">
          <b-input v-model="subtitle"> </b-input>
        </b-field>
        <b-field label="Image">
          <b-input v-model="image"></b-input>
        </b-field>
        <b-field label="Content">
          <b-input v-model="content" type="textarea"></b-input>
        </b-field>

        <b-field label="Author">
          <b-input v-model="author"></b-input>
        </b-field>
        <div>
          <b-button @click="createPost"> Save Post </b-button>
        </div>
      </section>
    </div>
  </section>
</template>

<script>
import gql from 'graphql-tag'

const ADD_POST_MUTATION = gql`
  mutation (
    $title: String!
    $subtitle: String!
    $image: String!
    $content: String!
    $author: String!
    $date: String!
  ) {
    createPost(
      data: {
        title: $title
        subtitle: $subtitle
        image: $image
        content: $content
        author: $author
        date: $date
      }
    ) {
      id
      title
    }
  }
`

export default {
  name: 'Create',
  data() {
    return {
      image: '',
      title: '',
      subtitle: '',
      content: '',
      author: '',
      date: '',
    }
  },
  computed: {
    postDate() {
      return new Date().toLocaleDateString('en-US', {
        month: 'long',
        day: 'numeric',
        year: 'numeric',
      })
    },
  },

  methods: {
    async createPost() {
      this.date = this.postDate

      const title = this.title
      const subtitle = this.subtitle
      const image = this.image
      const content = this.content
      const author = this.author
      const date = this.date

      await this.$apollo
        .mutate({
          mutation: ADD_POST_MUTATION,
          variables: {
            title,
            subtitle,
            image,
            content,
            author,
            date,
          },
        })
        .then(() => {
          this.$router.push('/')
        })
        .catch((error) => {
          console.log(error)
        })
    },
  },
}
</script>

<style></style>
