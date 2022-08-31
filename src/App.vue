<template>
  <totalizer-main />
</template>

<script>
import TotalizerMain from './components/TotalizerMain';

export default {
  name: 'App',
  components: {
    TotalizerMain,
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#app > * {
  box-sizing: border-box;
}

button {
  background-color: #265727;
  border: none;
  color: white;
  padding: .5rem;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  border-radius: 5px;

  &:hover {
    background-color: #479f48;
  }

  &:disabled {
    background-color: #98b099;
  }

  &:not(:disabled) {
    cursor: pointer;
  }

  &.button-wide {
    width: 100%;
  }
}
</style>
