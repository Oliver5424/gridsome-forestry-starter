<template>
  <Layout>
    <div class="project">

      <div class="container">

        <div class="project-header">
          <h1 class="project-title" v-html="$page.post.title" />
          <div class="project-info">

            <div class="categories-container">
              <div class="categories">
                <span class="label">Categories</span>
                <span 
                  class="category"
                  v-for="item in $page.post.tags" 
                  :key="item.id"
                  v-text="item.title"
                />
              </div>
            </div>

            <div class="year-container">
              <span class="label">Date</span>
              <div v-html="$page.post.updated_at"/>
            </div>
          </div>
        </div>

        <div v-html="mdToHtml($page.post.content)" class="content" />

      </div>

    </div>
  </Layout>
</template>

<page-query>
query ($id: ID!) {
  post: strapiPost (id: $id) {
    id,
    title,
    content,
    updated_at(format: "YYYY-MM-DD"),
    tags {
      id,
      title
    }
  }
}
</page-query>

<script>
import MarkdownIt from 'markdown-it'
const md = new MarkdownIt()
export default {
  metaInfo () {
    return {
      title: this.$page.post.title,
      bodyAttrs: {
        style: `background-color: ${this.$page.post.project_bg_color ? this.$page.post.project_bg_color : 'var(--color-base)'}; color: ${this.$page.post.project_fg_color ? this.$page.post.project_fg_color : 'var(--color-contrast)'}`
      }
    }
  },
  methods: {
    mdToHtml (markdown) {
      console.log(md.render(markdown))
      const content = md.render(markdown)
      var newContent= content.replace(/<img [^>]*src=['"]([^'"]+)[^>]*>/gi,
        (match, capture) => {
          const index = match.indexOf('src="') + 5
          const newStr = match.slice(0, index) + this.GRIDSOME_API_URL + match.slice(index)
          return newStr;
        })
      console.log(newContent);
      return newContent
    }
  }
}
</script>

<style scoped>
.project-header {
  padding: 20vh 0 4rem 0;
}
.project-title {
  font-size: 4rem;
  margin: 0 0 4rem 0;
  padding: 0;
}
.project-info {
  display: flex;
  flex-wrap: wrap;
  font-size: 0.8rem;
}
.project-info > div {
  margin-right: 4rem;
}
.project-info > div:last-of-type {
  margin: 0;
}
.category:after {
  content: ', '
}
.category:last-of-type:after {
  content: '';
}
</style>
