<template>
  <div id="app">
    <CoolBox></CoolBox>
  </div>
</template>

<script>
import CoolBox from './CoolBox';

export default {
  components: {
    CoolBox,
  },
  name: 'app',
};
</script>

<style lang="scss">
#app {
  font-family: 'Libre Franklin', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  font-weight: 400;
  line-height: 1.45;
  color: #333;
  font-size: 14px;
}

p {margin-bottom: 1.3em;}

h1, h2, h3, h4 {
  margin: 1.414em 0 0.5em;
  font-weight: inherit;
  line-height: 1.2;
  font-family: 'Space Mono', monospace;
}

h1 {
  margin-top: 0;
  font-size: 2.074em;
}

h2 {font-size: 1.728em;}

h3 {font-size: 1.44em;}

h4 {font-size: 1.2em;}

small, .font_small {font-size: 0.833em;}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}
</style>
