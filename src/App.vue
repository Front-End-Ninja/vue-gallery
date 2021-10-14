<template>
  <div :class="[{flexStart: step === 1},'wrapper']">
    <transition name="fade">
      <HeroImage v-if="step === 0"/>
    </transition>
    <Claim v-if="step === 0"/>
    <SearchInput @handleChange="handleInput" :dark="step === 1"/>
    <div class="results">
      <div v-for="item in results" :key="data.collection.items[].nasa_id">
        <p>{{ item.links[0].href }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Claim from '@/components/Claim.vue';
import SearchInput from '@/components/SearchInput.vue';
import HeroImage from '@/components/HeroImage.vue';

const API = 'https://images-api.nasa.gov';

export default {
  name: 'App',
  components: {
    HeroImage,
    Claim,
    SearchInput,
  },
  data() {
    return {
      loading: false,
      step: 0,
      results: [],
    };
  },
  methods: {
    handleInput(event) {
      this.loading = true;
      axios.get(`${API}/search?q=${event}&media_type=image`)
        .then((resp) => {
          this.results = resp.data.collection.items;
          this.loading = false;
          this.step = 1;
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;600;800&display=swap');

* {
  box-sizing: border-box;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

body {
  font-family: 'Montserrat', sans-serif;
  margin: 0;
  padding: 0;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .3s ease;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}

.slide-enter-active, .slide-leave-active {
  transition: margin-top .3s ease;
}

.slide-enter, .slide-leave-to {
  margin-top: -50px;
}

.wrapper {
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin: 0;
  padding: 30px;
  width: 100%;
  height: 100vh;

  &.flexStart {
    justify-content: flex-start;
  }
}
</style>
