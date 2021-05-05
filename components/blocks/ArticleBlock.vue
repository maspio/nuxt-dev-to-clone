<template>
  <article>
    <template v-if="$fetchState.pending">
      <content-placeholders rounded>
        <content-placeholders-heading />
        <content-placeholders-img />
        <content-placeholders-text :lines="50" />
      </content-placeholders>
    </template>
    <template v-else-if="$fetchState.error">
      <inline-error-block :error="$fetchState.error" />
    </template>
    <template v-else>
      <header>
        <h1>{{ article.title }}</h1>
        <div class="tags">
          <nuxt-link
            v-for="tag in article.tags"
            :key="tag"
            :to="{ name: 't-tag', params: { tag } }"
            class="tag"
          >
            #{{ tag }}
          </nuxt-link>
        </div>
        <div v-if="article.cover_image" class="image-wrapper">
          <img :src="article.cover_image" :alt="article.title" />
        </div>
        <div class="meta">
          <div class="scl">
            <span>
              <heart-icon />
              {{ article.positive_reactions_count }}
            </span>
            <span class="comments" @click="scrollToComments">
              <comments-icon />
              {{ article.comments_count }}
            </span>
          </div>
          <time>{{ article.readable_publish_date }}</time>
        </div>
      </header>
      <!-- eslint-disable-next-line -->
      <div class="content" v-html="article.body_html" />
    </template>
  </article>
</template>

<script>
import HeartIcon from '@/assets/icons/heart.svg?inline'
import InlineErrorBlock from '@/components/blocks/InlineErrorBlock'
import CommentsIcon from '@/assets/icons/comments.svg?inline'

export default {
  components: {
    HeartIcon,
    InlineErrorBlock,
    CommentsIcon
  },
  props: [],
  async fetch() {
    const article = await fetch(
      `https://dev.to/api/articles/${this.$route.params.article}`
    ).then((res) => res.json())

    if (article.id && article.user.username === this.$route.params.username) {
      this.article = article
      this.$store.commit('SET_CURRENT_ARTICLE', this.article)
    } else {
      // set status code on server
      if (process.server) {
        this.$nuxt.context.res.statusCode = 404
      }
      throw new Error('Article not found')
    }
  },
  data() {
    return {
      article: {}
    }
  },
  activated() {
    // Call fetch again if last fetch more than 60 sec ago
    if (this.$fetchState.timestamp <= Date.now() - 60000) {
      this.$fetch()
    }
  },
  methods: {
    scrollToComments() {
      const el = document.querySelector('#comments')
      if (el) {
        const scrollTo = el.getBoundingClientRect().top
        window.scrollBy({ top: scrollTo - 20, left: 0, behavior: 'smooth' })
      }
    }
  },
  head() {
    return {
      title: this.article.title
    }
  }
}
</script>

<style scoped>
article {
	 padding: 0.5rem;
	 border-radius: 1rem;
}
 header {
	 margin-bottom: 1rem;
}
 header h1 {
	 font-size: 2.25rem;
	 letter-spacing: -0.025rem;
	 margin-bottom: 1rem;
}
 header .tags {
	 display: flex;
	 flex-wrap: wrap;
	 margin-bottom: 1.5rem;
}
 header .tags .tag {
	 font-weight: 500;
	 line-height: 1;
	 padding: 0.5rem 0.5rem;
	 margin: 0 0.5rem 0.5rem 0;
	 border-radius: 0.25rem;
	 box-shadow: -4px -4px 8px #f8fafe, 4px 4px 8px #ced2db;
}
 header .tags .tag:hover {
	 background: linear-gradient(135deg, rgba(0, 0, 0, 0.09), rgba(255, 255, 255, 0));
}
 header .tags .tag:active {
	 background: transparent;
	 box-shadow: inset -4px -4px 8px #f0f3f9, inset 4px 4px 8px #ced2db, inset -1px -1px 4px #8e8e8e;
}
 header .image-wrapper {
	 position: relative;
	 padding-bottom: 56.25%;
	 background-color: #d4dfe8;
	 margin-bottom: 1.5rem;
	 border-radius: 0.5rem;
	 overflow: hidden;
}
 @media (min-width: 834px) {
	 header .image-wrapper {
		 margin-bottom: 1.5rem;
	}
}
 header .image-wrapper img {
	 position: absolute;
	 top: 0;
	 left: 0;
	 width: 100%;
	 height: 100%;
	 object-fit: cover;
}
 header .meta {
	 line-height: 1;
	 font-size: 0.875rem;
	 text-transform: uppercase;
	 font-weight: 500;
	 letter-spacing: -0.025rem;
	 display: flex;
	 align-items: center;
	 justify-content: space-between;
}
 header .meta .scl {
	 display: flex;
}
 header .meta .scl span {
	 display: flex;
	 align-items: center;
	 margin-right: 1rem;
}
 header .meta .scl span svg {
	 margin-right: 0.25rem;
}
 header .meta .scl .comments {
	 cursor: pointer;
}
 ::v-deep .content .ltag__user {
	 display: none;
}
 ::v-deep .content iframe {
	 max-width: 100%;
}
 ::v-deep .content h1 {
	 font-size: 1.875rem;
	 margin-top: 2rem;
	 margin-bottom: 1rem;
	 letter-spacing: -0.025rem;
}
 ::v-deep .content h2 {
	 font-size: 1.5rem;
	 margin-top: 2rem;
	 margin-bottom: 1rem;
	 letter-spacing: -0.025rem;
}
 ::v-deep .content h3 {
	 font-size: 1.25rem;
	 margin-top: 2rem;
	 margin-bottom: 1rem;
	 letter-spacing: -0.025rem;
}
 ::v-deep .content h4 {
	 font-size: 1rem;
	 margin-top: 2rem;
	 margin-bottom: 1rem;
	 letter-spacing: -0.025rem;
}
 ::v-deep .content a {
	 color: #6e87d2;
}
 ::v-deep .content p {
	 margin-bottom: 1rem;
	 line-height: 1.4;
}
 ::v-deep .content p code {
	 background-color: #d2f3e1;
	 border-radius: 0.25rem;
	 padding: 0.25rem;
}
 ::v-deep .content img {
	 width: 100%;
	 border-radius: 0.5rem;
}
 ::v-deep .content .highlight {
	 margin-bottom: 1rem;
	 border-radius: 0.5rem;
}
 ::v-deep .content ul {
	 list-style: numeral;
	 margin-bottom: 1rem;
}
 ::v-deep .content ul li p {
	 margin-bottom: 0;
}
 ::v-deep .content ol {
	 margin-bottom: 1rem;
}

</style>
