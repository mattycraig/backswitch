<template>
  <div class="pb-5">
    <h1>{{ cat[0].category | capitalize }}</h1>
    <div v-for="page in pages" :key="page.slug">
      <nuxt-link :to="page.path"> {{ page.title }} </nuxt-link>
    </div>
  </div>
</template>

<script>
import config from '~/data/siteConfig'

export default {
  filters: {
    capitalize(value) {
      if (!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    },
  },
  async asyncData({ $content, params }) {
    const pages = await $content('blog')
      .sortBy('title')
      .where({ category: params.slug })
      .fetch()

    const cat = await $content('blog')
      .only('category')
      .where({ category: params.slug })
      .limit(1)
      .fetch()

    return {
      pages,
      cat,
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
