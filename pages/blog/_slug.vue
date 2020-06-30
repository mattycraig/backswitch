<template>
  <div class="pb-5">
    <nuxt-content :document="page" />
    <h5 v-if="prev">
      <nuxt-link :to="prev.slug">Previous</nuxt-link>
    </h5>
    <h5 v-if="next">
      <nuxt-link :to="next.slug">Next</nuxt-link>
    </h5>
  </div>
</template>

<script>
import config from '~/data/siteConfig'

export default {
  async asyncData({ $content, params }) {
    const slug = `blog/${params.slug}` || 'index'
    const page = await $content(slug).fetch()
    const [prev, next] = await $content('blog')
      .only(['title', 'slug'])
      .sortBy('title', 'asc')
      .surround(params.slug, { before: 1, after: 1 })
      .fetch()

    return {
      page,
      prev,
      next,
    }
  },
  head() {
    return {
      title: this.page.title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.page.description,
        },
        {
          hid: 'twitter:title',
          name: 'twitter:title',
          content: `${this.page.title} - ${config.siteTitle}`,
        },
        {
          hid: 'twitter:description',
          name: 'twitter:description',
          content: this.page.description,
        },
        {
          hid: 'og:title',
          property: 'og:title',
          content: `${this.page.title} - ${config.siteTitle}`,
        },
        {
          hid: 'og:description',
          property: 'og:description',
          content: this.page.description,
        },
        // {
        //   hid: 'twitter:image',
        //   name: 'twitter:image',
        //   content: `${config.siteUrl}/${this.page.image}`,
        // },
        // {
        //   hid: 'og:image',
        //   property: 'og:image',
        //   content: `${config.siteUrl}/${this.page.image}`,
        // },
      ],
    }
  },
}
</script>
