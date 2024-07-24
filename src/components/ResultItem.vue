<template>
  <div
    class="flex items-center m-4 p-4 rounded-2xl border-gray-200 hover:bg-[#F2F4F8] relative group"
  >
    <div class="flex items-center flex-1">
      <img :src="item.image" alt="" class="w-8 h-8 mr-4" loading="lazy" />
      <div>
        <h3 class="font-semibold">{{ item.title }}</h3>
        <p class="text-gray-500">{{ item.description }}</p>
      </div>
    </div>
    <a :href="item.url" target="_blank">
      <img
        src="../assets/image4.png"
        alt=""
        class="w-6 ml-auto opacity-0 group-hover:opacity-100"
      />
    </a>
  </div>
</template>

<script>
export default {
  props: ['item']
};
</script>

<style scoped>
/* Darken image4 on hover */
.group:hover img:last-child {
  filter: brightness(0.7);
}
</style>
