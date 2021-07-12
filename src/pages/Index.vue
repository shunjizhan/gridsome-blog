<template>
  <Layout>
    <!-- Page Header -->
    <header class="masthead" style="background-image: url('img/home-bg.jpg')">
      <div class="overlay"></div>
      <div class="container">
        <div class="row">
          <div class="col-lg-8 col-md-10 mx-auto">
            <div class="site-heading">
              <h1>Clean Blog</h1>
              <span class="subheading">A Blog Theme by Start Bootstrap</span>
            </div>
          </div>
        </div>
      </div>
    </header>

    <!-- Main Content -->
    <div class="container">
      <div class="row">
        <div class="col-lg-8 col-md-10 mx-auto">
          <div class="post-preview" v-for="edge in $page.posts.edges" :key="edge.node.id">
            <a href="post.html">
              <h2 class="post-title">
                {{ edge.node.title}}
              </h2>
              <!-- <h3 class="post-subtitle">
                Problems look mighty small from 150 miles up
              </h3> -->
            </a>
            <p class="post-meta">
              Posted by
              <a href="#">author name</a>
               {{ edge.node.created_at }}
            </p>
            <p>
              <span v-for="tag in edge.node.tags" :key="tag.id">
                <a href="" >{{ tag.title }}</a>
                &nbsp;
              </span>
            </p>
          </div>
          <hr />
          <!-- Pager -->
          <div class="clearfix">
            <a class="btn btn-primary float-right" href="#"
              >Older Posts &rarr;</a
            >
          </div>
        </div>
      </div>
    </div>
  </Layout>
</template>

<page-query>
query {
	posts: allStrapiPost {
    edges {
      node {
        id
        title
				created_at
        tags {
          id
          title
        }
      }
    }
  }
}
</page-query>

<script>
export default {
  metaInfo: {
    title: "Hello, world!",
  },
  name: 'Homepage',
};
</script>

<style>
</style>
