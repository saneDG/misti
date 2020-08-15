<template>
    <div id="app">
        <img
            class="bak"
            src="./assets/bak1.png">
        <Paa />
    </div>
</template>

<script>
import Paa from './components/Paa.vue'

export default {
    name: 'App',
    components: {
        Paa
    }
}
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Lato:ital,wght@0,100;0,300;0,400;0,700;0,900;1,100;1,300;1,400;1,700;1,900&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Raleway:ital,wght@0,100;0,200;0,300;1,100;1,200&display=swap");

#app {
  // font-family: "Lato", sans-serif;
  font-family: "Raleway", sans-serif;
  font-weight: 100;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: whitesmoke;
  text-transform: uppercase;
  @for $i from 1 through 6 {
    h#{$i} {
      font-weight: 100;
      font-size: 96px;
      &::selection {
        background: none;
      }
      &:hover {
      }
    }
  }
  h2 {
    font-weight: 100;
    font-size: 48px;
  }
}
.bak {
  position: absolute;
  left: 0;
  top: 0;
  z-index: -1;
  object-fit: cover;
  width: 100%;
  height: 100%;
}
</style>
