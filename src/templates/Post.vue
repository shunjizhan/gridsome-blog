<template>
  <layout>
    <!-- Page Header -->
    <header class="masthead" :style="{ backgroundImage: `url(http://localhost:1337${$page.curPost.cover.url})  ` }">
      <div class="overlay"></div>
      <div class="container">
        <div class="row">
          <div class="col-lg-8 col-md-10 mx-auto">
            <div class="post-heading">
              <h1>{{ $page.curPost.title }}</h1>
              <span class="meta"
                >Posted by
                <a href="#">xxx</a>
                {{ $page.curPost.created_at }}</span
              >
            </div>
          </div>
        </div>
      </div>
    </header>

    <!-- Post Content -->
    <article>
      <div class="container">
        <div class="row">
          <div class="col-lg-8 col-md-10 mx-auto" v-html="$page.curPost.content">
          </div>
        </div>
      </div>
    </article>
  </layout>
</template>

<page-query>
query ($id: ID!) {
	curPost: strapiPost (id: $id) {
    id
    title
    created_at
    content
    cover {
      url
    }
    tags {
      id
      title
    }
  }
}
</page-query>

<script>
export default {
  name: 'PostPage',
}
</script>

<style>
</style>