<template>
  <div>{{ getPost }}</div>
</template>

<script>
import gql from 'graphql-tag'

const POST_BY_ID = gql`
  query {
    getPost(id: $id) {
      id
      image
      title
      content
      author
      date
    }
  }
`

export default {
  name: 'Post',
  data() {
    return {
      post: null,
    }
  },
  apollo: {
    getPost: {
      query: POST_BY_ID,
      variables: {
        id: '5',
      },
    },
  },
  created() {},
  methods: {},
}
</script>

<style></style>
