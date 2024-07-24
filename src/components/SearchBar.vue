<template>
  <div class="relative flex flex-col items-center px-4 pt-4">
    <div class="relative w-full">
      <img src="../assets/image7.png" alt="Search Icon" class="absolute left-3 top-1/2 transform -translate-y-1/2 w-5 h-5" />
      <input
        type="text"
        v-model="query"
        @input="onInput"
        placeholder="Search technologies we use at DC..."
        class="w-full pl-10 pr-4 py-2 border border-gray-300 rounded-lg focus:outline-none transition duration-200 ease-in-out"
        :class="{'focus:ring-2 focus:ring-[#6833FF]': query, 'focus:ring-2 focus:ring-red-500': error}"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { debounce } from '../utils';

export default defineComponent({
  props: {
    onSearch: {
      type: Function,
      required: true
    }
  },
  data() {
    return {
      query: '',
      error: null as string | null,
      debouncedSearch: null as unknown as (query: string) => Promise<void>
    };
  },
  methods: {
    setQuery(query: string) {
      this.query = query;
    },
    onInput() {
      this.error = null;
      if (this.query.length > 2) {
        this.debouncedSearch(this.query).catch((err: Error) => {
          this.error = err.message;
        });
      }
    }
  },
  created() {
    this.debouncedSearch = debounce(this.onSearch, 300);
  }
});
</script>

<style scoped>
/* Add custom styles here if needed */
</style>
