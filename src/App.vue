<template>
  <div id="app">
    <div class="wrapper">
      <div class="tabs">
        <div
          v-if="activeTab === 1"
          class="tab main-tab"
        >
          <TestPage
            :words="words"
          />
        </div>
        <div
          v-if="activeTab === 2"
          class="tab"
        >
          <Vocabulary
            :words="searchingWords"
          />
        </div>
      </div>
      <div
        class="mark"
        :class="{ 'mark-dark': activeTab === 2 }"
        @click="goTab"
      >
        {{ mark }}
      </div>
    </div>
  </div>
</template>

<script>
import TestPage from '@/views/TestPage'
import Vocabulary from '@/views/Vocabulary'

export default {
  name: 'App',
  data() {
    return {
      activeTab: 1,
      mark: 'Vocabulary',
      words: localStorage.getItem('words') ? JSON.parse(localStorage.getItem('words')).sort((a, b) => a.en.localeCompare(b.en)) : []
    }
  },

  components: {
    TestPage,
    Vocabulary
  },

  computed: {
    searchingWords() {
      return this.words.filter(x => x.en.includes(this.searchWord))
    }
  },

  methods: {
    goTab() {
      this.activeTab = this.activeTab === 1 ? 2 : 1
      this.mark = this.activeTab === 2 ? 'Test' : 'Vocabulary'
    }
  }
}
</script>

<style lang="scss">
@import '@/assets/scss/config';

.mark {
  position: absolute;
  top: 77px;
  right: -88px;
  display: flex;
  justify-content: center;
  align-items: flex-end;
  height: 60px;
  background-color: #fff;
  padding: 10px 20px;
  font-size: 24px;
  font-weight: 600;
  transform: rotate(90deg);
  cursor: pointer;
  transition: all .2s ease;
  user-select: none;

  &:hover {
    right: -78px;
  }

  &-dark {
    top: 22px;
    right: -34px;
    background-color: $main-color;
    color: #fff;

    &:hover {
      right: -24px;
    }
  }
}

.main {
  &-tab {
    background-color: $main-color;
  }
}
</style>
