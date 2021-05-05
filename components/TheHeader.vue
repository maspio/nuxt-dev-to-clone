<template>
  <header>
    <nuxt-link to="/" class="logo-wrapper">
      <nuxt-icon />
    </nuxt-link>
    <nav>
      <ul>
        <li v-for="link in links" :key="link.slug">
          <nuxt-link :to="{ name: link.slug }">
            {{ link.name }}
          </nuxt-link>
        </li>
      </ul>
    </nav>
  </header>
</template>

<script>
import NuxtIcon from '@/assets/icons/nuxt.svg?inline'

export default {
  components: {
    NuxtIcon
  },
  data() {
    return {
      links: [
        {
          name: 'New',
          slug: 'index'
        },
        {
          name: 'Top',
          slug: 'top'
        }
      ]
    }
  }
}
</script>
