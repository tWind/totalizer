<template>
  <div class="totalizer-result">
    <slot></slot>  
  </div>
</template>

<script>
export default {
  name: 'TotalizerResult',
}
</script>

<style lang="scss">
.totalizer-result {
  padding: 0 1rem;
  border: 3px solid gray;
}
</style>
