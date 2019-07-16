<template>
  <div class="container">
    <div class="columns">
      <div class="column is-offset-2 is-8">
        <h1 class="title is-2">
          Latest Posts
        </h1>
        <hr />
        <h2 v-for="(post, index) in posts" :key="index" class="title is-4">
          <nuxt-link
            :to="{ name: 'blogpost', params: { post: post.fields.slug } }"
          >
            {{ post.fields.title }}
          </nuxt-link>
        </h2>
      </div>
    </div>
  </div>
</template>

<script>
import { createClient } from '../plugins/contentful.js';

const client = createClient();
export default {
  // `env` is available in the context object
  asyncData({ env }) {
    return Promise.all([
      // fetch the owner of the blog
      client.getEntries({
        'sys.id': env.CTF_PERSON_ID,
      }),
      // fetch all blog posts sorted by creation date
      client.getEntries({
        content_type: env.CTF_BLOG_POST_TYPE_ID,
        order: '-sys.createdAt',
      }),
    ]).then(([entries, posts]) => {
      // return data that should be available
      // in the template
      return {
        posts: posts.items,
      };
    });
  },
};
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
