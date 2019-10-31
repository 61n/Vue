<template>
  <div>
    <b-nav>
      <b-nav-item>Counter: {{ correctNum }}/{{ total }}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  name: 'Header',
  props: ['correctNum', 'total']
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
