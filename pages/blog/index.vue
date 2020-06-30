<template>
  <div class="pb-5">
    <div v-for="page in pages" :key="page.slug">
      <nuxt-link :to="page.path">
        {{ page.title }}
      </nuxt-link>
    </div>
  </div>
</template>

<script>
import config from '~/data/siteConfig'

export default {
  async asyncData({ $content, params }) {
    const pages = await $content('blog').sortBy('title').fetch()

    return {
      pages,
    }
  },
  data() {
    return {
      metaTitle: 'Blog',
      metaDescription: 'This is the blog page.',
      // metaImage: 'blog_icon.png'
    }
  },
  head() {
    return {
      title: this.metaTitle,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.metaDescription,
        },
        {
          hid: 'twitter:title',
          name: 'twitter:title',
          content: `${this.metaTitle} - ${config.siteTitle}`,
        },
        {
          hid: 'twitter:description',
          name: 'twitter:description',
          content: this.metaDescription,
        },
        {
          hid: 'og:title',
          property: 'og:title',
          content: `${this.metaTitle} - ${config.siteTitle}`,
        },
        {
          hid: 'og:description',
          property: 'og:description',
          content: this.metaDescription,
        },
        // {
        //   hid: 'twitter:image',
        //   name: 'twitter:image',
        //   content: `${config.siteUrl}/${this.metaImage}`
        // },
        // {
        //   hid: 'og:image',
        //   property: 'og:image',
        //   content: `${config.siteUrl}/${this.metaImage}`
        // }
      ],
    }
  },
}
</script>
