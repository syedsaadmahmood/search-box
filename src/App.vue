<template>
  <div class="flex justify-center min-h-screen bg-blue-50 relative pt-20">
    <transition name="box-size">
      <div :class="boxSizeClass" class="rounded-2xl shadow-lg bg-white relative z-10 flex flex-col">
        <SearchBar ref="searchBar" :onSearch="search" />
        <div class="mt-4 px-4">
          <div class="tag-list grid grid-cols-2 gap-2 sm:flex sm:flex-wrap sm:space-x-2" ref="tagsContainer">
            <TagList
              v-for="(tag, index) in tags"
              :key="tag"
              :isActive="activeTag === tag"
              @click="filterTag(tag)"
              @tab="handleTagTab(index)"
            >
              {{ tag }}
            </TagList>
          </div>
        </div>
        <div class="flex justify-center">
          <img
            v-if="!loading && !timeoutError && results?.length === 0"
            src="./assets/image1.png"
            class="w-80 h-80 mt-16"
            alt="Loading Image"
          />
          <img
            v-if="timeoutError && results.length === 0"
            src="./assets/image3.png"
            class="w-80 h-80 mt-16"
            alt="Error Image"
          />
        </div>
        <div class="overflow-auto flex-grow mt-4 custom-scrollbar max-h-[445px]">
          <Loader v-if="loading" />
          <div v-else>
            <transition-group name="fade" tag="div">
              <div v-for="item in results" :key="item.id" class="mt-4">
                <ResultItem :item="item" />
              </div>
            </transition-group>
          </div>
        </div>
        <div class="absolute bottom-0 left-0 right-0 p-4 bg-white border-t rounded-b-2xl">
          <div v-if="loading" class="text-gray-500">Searching...</div>
          <div v-if="timeoutError && results.length === 0" class="text-red-500">
            Request timed out. Please try again.
          </div>
          <div v-if="!loading && !timeoutError">
            <div v-if="results?.length === 0" class="text-gray-500">
              No result
            </div>
            <div v-else class="text-gray-500">
              {{ results.length }}
              {{ results.length === 1 ? 'result' : 'results' }}
            </div>
          </div>
        </div>
      </div>
    </transition>
    <img
      src="./assets/image2.png"
      class="absolute bottom-5 right-5 h-10 w-16"
      alt="Bottom Right Image"
    />
  </div>
</template>

<script>
import SearchBar from './components/SearchBar.vue';
import TagList from './components/TagList.vue';
import ResultItem from './components/ResultItem.vue';
import Loader from './components/Loader.vue';

export default {
  components: {
    SearchBar,
    TagList,
    ResultItem,
    Loader
  },
  data() {
    return {
      results: [],
      loading: false,
      timeoutError: false,
      activeTag: '',
      tags: ['Languages', 'Build', 'Design', 'Cloud'],
      focusedTagIndex: null
    };
  },
  computed: {
    boxSizeClass() {
      switch (this.results.length) {
        case 1:
          return 'size-1';
        case 2:
          return 'size-2';
        case 3:
          return 'size-3';
        default:
          return 'size-full';
      }
    }
  },
  methods: {
    async search(query) {
      this.loading = true;
      this.timeoutError = false;
      this.results = [];
      if (query !== this.activeTag) {
        this.activeTag = '';
        this.results = [];
      }
      try {
        const response = await this.fetchWithTimeout(`https://frontend-test-api.digitalcreative.cn/?no-throttling=false&search=${query}`, 8000);
        if (!response.ok) throw new Error('Network response was not ok.');
        const data = await response.json();
        this.results = data;
        if (data.length === 0) {
          this.timeoutError = true;
        }
      } catch (error) {
        if (error.message === 'Request timed out' || this.results.length === 0) {
          this.timeoutError = true;
        } else {
          console.error('Search error:', error);
          this.timeoutError = true;
        }
      } finally {
        this.loading = false;
      }
    },
    fetchWithTimeout(url, timeout) {
      return new Promise((resolve, reject) => {
        const timer = setTimeout(() => {
          reject(new Error('Request timed out'));
        }, timeout);

        fetch(url)
          .then(response => {
            clearTimeout(timer);
            resolve(response);
          })
          .catch(err => {
            clearTimeout(timer);
            reject(err);
          });
      });
    },
    filterTag(tag) {
      this.activeTag = tag;
      this.$refs?.searchBar?.setQuery(tag);
      this.search(tag);
    },
    handleTagTab(index) {
      this.focusedTagIndex = index;
    },
    handleKeydown(event) {
      if (event.key === 'Tab') {
        const tagsContainer = this.$refs.tagsContainer;
        const tags = tagsContainer.children;
        if (this.focusedTagIndex !== null) {
          const nextIndex = (this.focusedTagIndex + 1) % tags.length;
          tags[nextIndex].focus();
          this.focusedTagIndex = nextIndex;
        }
        event.preventDefault();
      }
    }
  },
  mounted() {
    document.addEventListener('keydown', this.handleKeydown);
  },
  beforeDestroy() {
    document.removeEventListener('keydown', this.handleKeydown);
  }
}
</script>

<style scoped>
.custom-scrollbar::-webkit-scrollbar {
  width: 12px;
}

.custom-scrollbar::-webkit-scrollbar-track {
  background: white;
}

.custom-scrollbar::-webkit-scrollbar-thumb {
  background-color: #888;
  border-radius: 10px;
  border: 3px solid #f1f1f1;
}

.custom-scrollbar::-webkit-scrollbar-thumb:hover {
  background: #555;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}

.box-size-enter-active, .box-size-leave-active {
  transition: all 0.5s ease;
}

.box-size-enter, .box-size-leave-to {
  transform: scale(1);
}

.size-1 {
  width: 690px;
  height: 300px;
}
@media (max-width: 640px) {
  .size-1 {
    width: 690px;
    height: 340px;
  }
}
.size-2 {
  width: 690px;
  height: 400px;
}
.size-3 {
  width: 690px;
  height: 500px;
}
.size-full {
  width: 690px;
  height: 600px;
}
</style>
