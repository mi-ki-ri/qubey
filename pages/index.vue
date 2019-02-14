<template>
  <section class="container">
    <div>
      <lane />
    </div>
  </section>
</template>

<script>
import Lane from '~/components/Lane.vue'

export default {
  components: {
    Lane
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  background: linear-gradient(rgb(200,225,255), rgb(225,255,255))
}

</style>
