<template>
  <div class="page-wrapper">
    <username-block />
    <username-articles-block />
  </div>
</template>

<script>
import UsernameBlock from '@/components/blocks/UsernameBlock'
import UsernameArticlesBlock from '@/components/blocks/UsernameArticlesBlock'

export default {
  components: {
    UsernameBlock,
    UsernameArticlesBlock
  }
}
</script>
