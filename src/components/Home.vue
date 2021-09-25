<template>
  <div class="home">
    <Popup :isShow="isShow" @close="closeModal" />
    <button class="home__callpopup" @click="openModal">{{ title }}</button>
  </div>
</template>

<script>
import Popup from './Popup.vue';

export default {
  name: 'Home',
  components: {
    Popup
  },
  data() {
    return {
      title: 'Налоговый вычет',
      isShow: false,
      isHide: true
    };
  },
  methods: {
    openModal() {
      this.isShow = true;
      this.$emit('show', this.isShow);
    },
    closeModal(data) {
      this.isShow = !data;
      this.$emit('close', data);
    }
  }
};
</script>

<style lang="scss">
.home {
  &__callpopup {
    position: relative;
    width: 198px;
    height: 56px;
    color: #ffffff;
    border: 1px solid #ffffff;
    box-sizing: border-box;
    filter: drop-shadow(0px 0px 44px #cfdae7);
    border-radius: 6px;
    background: transparent;
    cursor: pointer;
  }
}
</style>
