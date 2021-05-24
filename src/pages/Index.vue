<template>
  <Layout>
    <div class="container">
      <Hero />
      <Post :projects="$page.projects.edges" />
    </div>
    <LatestJournals :journals="$page.journals.edges" />
  </Layout>
</template>

<page-query>
query Posts {
	projects: allStrapiPost{
    edges{
      node{
        id,
        title,
        thumbnail{
          id,
          url
        },
        tags {
          id,
          title
        }
      }
    }
  },
  journals: allJournalPost (perPage: 4) {
    edges {
      node {
        id
        path
        title
      }
    }
  },
  tag: allStrapiTag {
    edges {
      node {
        id,
        title
        }
      }
    }
}
</page-query>

<script>
import Hero from "@/components/Hero"
import Post from "@/components/Post"
import LatestJournals from "@/components/LatestJournals"

export default {
  components: {
    Hero,
    Post,
    LatestJournals
  },
  provide() {
    return {
      tag: this.$page.tag.edges
    }
  }
}
</script>
