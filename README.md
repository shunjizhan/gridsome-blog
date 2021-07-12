# Gridsome Blog
This is a **static** blog app that is **SSR** rendered, powered by `Vue`, `grindsome` and `Strapi`, for learning purpose.

## Implementation
see more details in [commits](https://github.com/shunjizhan/gridsome-blog/commits/main)

### prefetch
In order to read pre-fetch data from different data source, we will need to install relevant plugins.Everytime we changed data in strapi backend, we will need to restart the gridsome dev server, since it needs to pre-fetch all the data in the starting process, so that it can do 预渲染 in SSR。

```ts
// gridsome.config.js
{
  use: '@gridsome/source-strapi',
  options: {
    apiURL: 'http://localhost:1337',
    queryLimit: 1000, // Defaults to 100
    contentTypes: ['post'],
    // singleTypes: ['impressum'],
    // Possibility to login with a Strapi user,
    // when content types are not publicly available (optional).
    // loginData: {
    //   identifier: '',
    //   password: ''
    // }
  }
}
```

### grab data
Prev step will save all the pre-fetched data to graphql, in order to grab data to the page, we can use `<page-query />`.
```html
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
```
this will save the graphql result to `this.$page.posts`. Vue treats its instance as a big context, pass everything around...

### pagination
gridsome has some built-in pagination helpers. We can use them directly.

### routing
we can put vue component in `templates/`, and specify the routing in config.
```ts
// gridsome.config.js
templates: {
  StrapiPost: [   // typename + contentType
    {
      path: '/post/:id',
      component: './src/templates/Post.vue',
    }
  ],
}
```
then in the page-query we can use this `:id` as param:
```html
<page-query>
query ($id: ID!) {
	curPost: strapiPost (id: $id) {
    id
    title
    ...
  }
}
</page-query>
```


## Resources
### production
[frontend page](https://gridsome-blog-eta.vercel.app/) (deployed by [vercel](vercel.com/shunjizhan/gridsome-blog))  
[admin entry](http://106.54.70.175:1337/admin) (deployed at Tecent cloud)  

### development
[admin entry](http://localhost:1337/admin)  
[frontend page](https://localhost:8080)

### repo
[Strapi backend repo](https://github.com/shunjizhan/strapi-CMS)  
[Gridsome frontend repo](https://github.com/shunjizhan/gridsome-blog)

## Integration
**goal**: When we update the Strapi data, trigger Vercel deploy.

This is achieved by **webhook**:
- first Vercel generates a webhook url (that triggers a new deploy) 
- then Strapi creates an action, so that when data were updated, trigger this webhook url

