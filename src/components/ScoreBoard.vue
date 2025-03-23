<template>
  <div>
    <section class="score">
      Player <span>{{this.winCount}}</span> x
      <span>{{this.lossCount}}</span> Computer
    </section>

  </div>
</template>

<script>
  export default {
    props: ['winCount', 'lossCount']
  }
</script>

<style lang="scss">

</style>