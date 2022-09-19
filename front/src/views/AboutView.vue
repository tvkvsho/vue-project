<script>

export default {
  name: 'App',
  data () {
    return {
      categories: [],
      img: [],
      error: null,
      headers: {'Content-Type': 'application/json'}
    }
  },
  methods: {
    parseJSON: function (resp) {
      return (resp.json ? resp.json() : resp);
    },
    checkStatus: function (resp) {
      if (resp.status >= 200 && resp.status < 300) {
        return resp;
      }
      return this.parseJSON(resp).then((resp) => {
        throw resp;
      });
    }
  },
  async mounted () {
    try {
      const response = await fetch("http://localhost:1337/api/categories", {
        method: 'GET',
        headers: this.headers,
      }).then(this.checkStatus)
          .then(this.parseJSON);
      this.categories = response.data

      const img = await fetch("http://localhost:1337/api/upload/files", {
        method: 'GET',
        headers: this.headers,
      }).then(this.checkStatus)
          .then(this.parseJSON);
      console.log(img);
      this.img = img

    } catch (error) {
      this.error = error
    }
  }
}
</script>

<template>
  <div id="app">
    <div v-if="error">
      {{ error }}
    </div>
    <ul v-else>
      <li class="category" v-for="(category, index) in categories">
        <img :src="'http://localhost:1337' + img[index].url" alt="" class="category__icon" width="200">
        <h2 class="category__title">{{ category.attributes.Title }}</h2>
      </li>
    </ul>
  </div>
</template>