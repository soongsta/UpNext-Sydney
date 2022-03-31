<template>
  <article>
    <h1>{{ $route.params.slug }}</h1>
    <!-- <nuxt-content :document="page" /> -->
    <div v-html="content.default"></div>
  </article>
</template>

<script>
export default {
  name: 'BlogPostPage',
  async asyncData({ $content, params }) {
    // console.log('$content:', $content)
    // console.log('params:', params)
    //const page = await $content('MD' + params.slug).fetch()
    const page = await import(`/content/MD/${params.slug}.md`)

    return {
      content: page,
    }
  },
}
</script>