<template>
    <div id="app">
        <div class="overlay" />
        <div
            class="input-div"
            :class="{ reveal: inputIsActive }">
            <input
                v-model="input"
                class="image-input"
                :placeholder="imageInputPlaceholder"
                @keyup.space="getImage(input)"
                @keyup.enter="inputIsActive = !inputIsActive">
            <div
                class="switch"
                @click="inputIsActive = !inputIsActive">
                IMG
            </div>
        </div>
        <transition name="fade">
            <img
                :key="image"
                class="bak"
                :src="image">
        </transition>
        <Paa />
    </div>
</template>

<script>
import Paa from './components/Paa.vue';

export default {
    name: 'App',
    components: {
        Paa
    },
    data: function () {
        return {
            image: null,
            params: 'calm forest',
            inputIsActive: false,
            imageInputPlaceholder: 'calm, forest'
        };
    },
    mounted: function () {
        this.$nextTick(() => {});
        try {
            this.image = `https://source.unsplash.com/1600x900/?${this.params}`;
        } catch (error) {
            console.log(error);
            this.image = './assets/bak1.png';
        }
    },
    methods: {
        imageParams (string) {
            this.params = string.split(' ');
        },
        getImage (string) {
            console.log(string);
            this.params = this.params + string;
            this.imageParams(string);
            console.log(this.params);
            this.image = `https://source.unsplash.com/1600x900/?${this.params}`;
        }
    }
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Lato:ital,wght@0,100;0,300;0,400;0,700;0,900;1,100;1,300;1,400;1,700;1,900&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Raleway:ital,wght@0,100;0,200;0,300;1,100;1,200&display=swap");
.overlay{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    margin: 0;
    padding:0;
  background-color: rgba(54, 54, 54, 0.603);
}
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
      font-size: 80px;
      filter: drop-shadow(3px 3px 2px rgba(0, 0, 0, 0.829));
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
.fade-enter-active, .fade-leave-active {
  transition: all 2s ease-in-out;
}
.fade-enter /* .fade-leave-active below version 2.1.8 */ {
  margin-top: 100vh;
  opacity: 0;
}
.fade-leave-to{
    margin-top: -100vh;
    opacity: 0;
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
.input-div {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  width: 20rem;
  top: 0rem;
  left: -19rem;
  transition: all 0.5s ease-in-out;
  height: 50px;
  .image-input {
    width: 100%;
    height: 100%;
    background-color: transparent;
    border: none;
    font-family: "Lato", sans-serif;
    font-weight: 100;
    color: whitesmoke;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    font-size: 30px;
    text-transform: uppercase;
    transition: background-color 0.5s;
    background-color: rgba(82, 82, 82, 0.171);
    &:hover {
      background-color: rgba(27, 27, 27, 0.171);
    }
    &:focus {
      outline: none;
      background-color: rgba(27, 27, 27, 0.171);
    }
    &::selection {
      background: #ffb7b73a;
    }
    &::placeholder {
      color: rgb(153, 153, 153);
      filter: drop-shadow(3px 3px 2px rgba(0, 0, 0, 0.829));
    }
  }
  .switch {
    min-width: 3rem;
    padding-right: 1rem;
    height: 100%;
    border-radius: 0 0 5px 0;
    background-color: rgba(170, 170, 170, 0.74);
  }
}
.reveal {
  left: 0rem;
}

@media screen and (max-width: 992px) {
  #app {
    h1 {
      font-size: 46px;
    }
    h2 {
      font-size: 46px;
    }
    h4 {
      font-size: 46px;
    }
  }
}
</style>
