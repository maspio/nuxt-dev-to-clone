<template>
  <div v-if="error" class="page-wrapper">
    <h1>{{ error.statusCode }}</h1>
    <h2>{{ error.message }}</h2>

    <div class="error-navigation">
      <a href="#" @click="$router.push(-1)"> Back </a>
      <nuxt-link to="/"> Main Page </nuxt-link>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    error: {
      type: Object,
      default: () => {}
    }
  },
  head() {
    return {
      title: this.error.message || 'Error Page'
    }
  }
}
</script>

<style scoped>
.page-wrapper {
	 max-width: 1280px;
	 margin: auto;
	 padding: 1rem;
	 min-height: calc(100vh - 11rem);
	 display: flex;
	 flex-direction: column;
	 align-items: center;
	 justify-content: center;
}
 .page-wrapper h1 {
	 font-size: 2.25rem;
	 letter-spacing: -0.05rem;
	 margin-bottom: 0.5rem;
}
 .page-wrapper h2 {
	 font-size: 1.5rem;
	 letter-spacing: -0.05rem;
	 margin-bottom: 1rem;
}
 .page-wrapper .error-navigation {
	 letter-spacing: -0.05rem;
}
 .page-wrapper .error-navigation a:first-child {
	 margin-right: 1rem;
}
 .page-wrapper .error-navigation a:hover {
	 color: #6e87d2;
}

</style>
