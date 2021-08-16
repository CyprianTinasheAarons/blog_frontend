<template>
  <div>
    <div>
      <NuxtLink to="create"
        ><b-button class="m-2 p-2 bg-black text-white"
          >Create Post</b-button
        ></NuxtLink
      >
    </div>
    <b-table :data="getPosts">
      <b-table-column v-slot="props" field="id" label="ID">
        {{ props.row.id }}
      </b-table-column>
      <b-table-column v-slot="props" field="title" label="Title">
        {{ props.row.title }}
      </b-table-column>
      <b-table-column v-slot="props" field="author" label="Author">
        {{ props.row.author }}
      </b-table-column>
      <b-table-column v-slot="props" field="date" label="Date">
        {{ props.row.date }}
      </b-table-column>
      <b-table-column v-slot="props" field="action" label="Action">
        <NuxtLink to="edit"><b-button>Edit</b-button></NuxtLink>
        <b-button @click="removeBook(props.row)">Delete</b-button>
      </b-table-column>
    </b-table>
  </div>
</template>

<script>
import gpl from 'graphql-tag'

const POSTS = gpl`
  query {
    getPosts{
      id
      title
      author
      date
    }
    }
    `

const REMOVE_POST = gpl`
  mutation ($id: String!){
  deletePost( id: $id) {
      id
  }
}
      `

export default {
  name: 'Admin',
  apollo: {
    getPosts: { query: POSTS, prefetch: true },
  },

  data() {
    return {}
  },
  methods: {
    async removeBook(post) {
      await this.$apollo.mutate({
        mutation: REMOVE_POST,
        variables: { id: post.id },
        refetchQueries: [
          {
            query: POSTS,
          },
        ],
      })
    },
  },
}
</script>
