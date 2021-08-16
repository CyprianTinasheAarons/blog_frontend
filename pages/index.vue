<template>
  <section class="section">
    <div v-for="post in getPosts" :key="post.id" class="flex justify-between">
      <NuxtLink :to="`/posts/${post.id}`">
        <card :title="post.title">
          <img :src="post.image" class="card-image" />

          {{ post.content }}
        </card>
      </NuxtLink>
    </div>
  </section>
</template>

<script>
import gpl from 'graphql-tag'

const POSTS = gpl`
  query {
    getPosts{
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
  name: 'HomePage',
  apollo: {
    getPosts: { query: POSTS, prefetch: true },
  },
}
</script>
