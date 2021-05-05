<template>
  <footer>
    <span>Build with</span>
    <a href="https://nuxtjs.org" target="_blank">
      <nuxt-icon class="nuxt-icon" />
    </a>
    <span>&</span>
    <a href="https://docs.dev.to/api" rel="nofollow noopener" target="_blank">
      <dev-to-icon />
    </a>
  </footer>
</template>

<script>
import DevToIcon from '@/assets/icons/dev-to.svg?inline'
import NuxtIcon from '@/assets/icons/nuxt.svg?inline'

export default {
  components: {
    DevToIcon,
    NuxtIcon
  }
}
</script>
