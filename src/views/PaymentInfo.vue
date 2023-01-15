<template>
  <div class="wrapper">
    <h3 class="payment-title">
      Payment Information
    </h3>
    <card-menu></card-menu>
  </div>
</template>

<script>
import CardMenu from '@/components/CardMenu.vue';

export default {
  components: { CardMenu }
}
</script>

<style scoped>
.wrapper {
  padding: 2rem 15rem;
  height: 100vh;
}
.payment-title {
  text-align: center;
  font-size: 2rem;
  font-weight: 500;
  color: #64758A
}
@media (max-width: 64.125em){
  .wrapper {
    padding: 0 2rem;
    height: auto;
    width: 100%;
  }
  .payment-title {
    font-size: 2rem;
  }
}
</style>