<script>
export default {
  name: "Products",
  data(){
    return{
      product: [],
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
      const response = await fetch("http://localhost:1337/api/products", {
        method: 'GET',
        headers: this.headers,
      }).then(this.checkStatus)
          .then(this.parseJSON);
      console.log(response.data);
      this.product = response.data

    } catch (error) {
      this.error = error
    }
  }
}
</script>

<template>
  <div>
    <div v-if="error">
      {{ error }}
    </div>
    <ul v-else class="products">
      <li class="product" v-for="(item, index) in product">
        <h2 class="product__title">
          {{ item.attributes.Title }}
        </h2>
        <p class="product__price">${{ item.attributes.Price }}</p>
      </li>
    </ul>
  </div>
</template>

<style lang="scss">
  .products{
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;
    gap: 12px;
    list-style: none;

    .product{
      display: flex;
      flex-direction: column;
      border: 1px solid #ffff;
      border-radius: 16px;
      padding: 24px;
      min-height: 202px;
    }
  }

</style>