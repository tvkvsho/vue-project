<template>
  <div class="order">
    <h2 class="order__number"></h2>
    <div class="order__items">
      <OrderItem />
    </div>
  </div>
</template>

<script>
import OrderItem from "../OrderItem/OrderItem";

export default {
  name: "Order",
  components: {
    OrderItem
  },
}
</script>

<style scoped>

</style>