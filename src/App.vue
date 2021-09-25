<template>
  <div class="root" :class="{ active: isShow }">
    <Home @show="onShow" @close="onClose" />
  </div>
</template>

<script>
import Home from './components/Home.vue';

export default {
  name: 'App',
  components: {
    Home
  },
  data() {
    return {
      isShow: false
    };
  },
  methods: {
    onShow(data) {
      this.isShow = data;
    },
    onClose(data) {
      this.isShow = !data;
    }
  }
};
</script>

<style lang="scss" src="./assets/styles/style.scss"></style>
