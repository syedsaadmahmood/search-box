<template>
  <div
    :tabindex="0"
    :class="[
      'px-3 py-1 rounded-full cursor-pointer flex items-center hover:bg-[#6833FF] hover:text-white transition duration-200 ease-in-out',
      { 'bg-[#6833FF] text-white': isActive, 'bg-[#F2F4F8] text-purple-700': !isActive }
    ]"
    @keydown.enter="$emit('click')"
    @keydown.tab="handleTab"
    @mouseover="handleMouseOver"
    @mouseout="handleMouseOut"
  >
    <img :src="currentImage" alt="" class="w-5 mr-2 mb-1" />
    <slot></slot>
  </div>
</template>

<script>
import image1 from '../assets/image5.png';
import image2 from '../assets/image6.png';

export default {
  props: {
    isActive: Boolean
  },
  data() {
    return {
      defaultImage: image1,
      activeImage: image2,
      hoverImage: image2,
      currentImage: image1
    };
  },
  watch: {
    isActive(newVal) {
      this.currentImage = newVal ? this.activeImage : this.defaultImage;
    }
  },
  methods: {
    handleTab(event) {
      event.preventDefault();
      this.$emit('tab');
    },
    handleMouseOver() {
      this.currentImage = this.hoverImage;
    },
    handleMouseOut() {
      this.currentImage = this.isActive ? this.activeImage : this.defaultImage;
    }
  },
  mounted() {
    this.currentImage = this.isActive ? this.activeImage : this.defaultImage;
  }
};
</script>

<style scoped>
/* Add additional styles if needed */
</style>
