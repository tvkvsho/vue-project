<template>
  <div>
    <div v-if="error">
      {{ error }}
    </div>
    <ul v-else class="categories">
      <li class="category" v-for="item in category">
        <div class="container" :style="">
          <h2 class="category__title">
            {{ item.attributes.Title }}
          </h2>
          <h3 class="category__color">
            {{ item.attributes.color }}
          </h3>
          <h3 class="category__counter">{{ item.length }}</h3>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Categories",
  data(){
    return{
      category: [],
      error: null,
      headers: { 'Content-Type': 'application/json'}
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
    },
  },
  async mounted() {
    try {
      const response = await fetch("http://localhost:1337/api/categories", {
        method: 'GET',
        headers: this.headers,
      }).then(this.checkStatus)
          .then(this.parseJSON);
      console.log(response.data);
      this.category = response.data

    } catch (error) {
      this.error = error
    }
  }
}
</script>

<style lang="scss">
  .categories{
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;
    gap: 12px;
    list-style: none;
    margin: 0 auto 32px;

    .category{
      display: flex;
      flex-direction: column;
      padding: 24px;
      border: 1px solid lightslategrey;
      border-radius: 16px;
      min-height: 202px;
    }
  }
</style>