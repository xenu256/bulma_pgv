<template>
<div class="container">
  <div class="columns">
    <div class="column is-two-third">
      <ad-component></ad-component>
      <h1 class="title is-1">{{ post.Title }}</h1>
      <div v-if="post.Image">
        <a :href="baseUrl + post.Slug+'/'">
          <img class="img-fluid" :src="imgBaseUrl + post.Image" :alt="post.Title">
        </a>
      </div>
      <div class="content">
        <p><small><a :href="baseUrl + keyword + '/' + post.CategoryID.Slug + '/'">
          {{ post.CategoryID.Title }}
        </a>
         | {{ post.Date | formatDate }}</small></p>
        <p v-if="post.Content">
          {{ post.Content }}
        </p>
        <ad-component></ad-component>
        <social-sharing :url="baseUrl + post.Slug + '/'" :title="post.Title">
        </social-sharing>
        <a class="button is-danger" :href="post.URL">
          Read more...
        </a>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import axios from 'axios'
import SocialSharing from '../components/SocialSharing.vue'
import Ads from '../components/Ads.vue'

export default {
  name: 'post',
  data () {
    return {
      post: null,
      baseUrl: process.env.BASE_URL,
      imgBaseUrl: process.env.IMG_URL,
      title: process.env.SITE_NAME,
      keyword: process.env.KEYWORD
    }
  },
  asyncData ({ req, params, error }) {
    return axios.get('/post/' + params.postSlug + '/')
      .then((response) => {
        return { post: response.data }
      })
      .catch((e) => {
        error({ statusCode: 500, message: e })
      })
  },
  components: {
    'social-sharing': SocialSharing,
    'ad-component': Ads
  },
  head () {
    return {
      title: this.post.Title + ' | ' + this.title
    }
  }
}
</script>

<style>
.img-fluid {
  width: 100%;
}

p {
    font-size: 2em;
    font-face: Roboto, Arial;
}

h1 {
  font-size: 4em;
  font-face: Roboto, Arial;
}
</style>