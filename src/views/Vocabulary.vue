<template>
  <div class="voc">
    <VocForm @update="updateState" />
    <div class="voc-search">
      <input
        v-model="searchWord"
        class="input input-voc input-search"
        placeholder="Поиск..."
      />
      <span class="words-count">{{ words.length }}</span>
    </div>
    <div class="voc-words">
      <WordItem
        v-for="(word, i) in words"
        :key="i"
        :word="word"
        @delete="deleteWord($event)"
      />
      <span
        v-if="!words.length"
        class="no-words"
      >
        База пуста
      </span>
    </div>
  </div>
</template>

<script>
import VocForm from '@/components/VocForm.vue'
import WordItem from '@/components/WordItem.vue'

export default {
  props: {
    words: {
      type: Array,
      default: () => []
    }
  },

  data() {
    return {
      searchWord: ''
    }
  },

  components: {
    VocForm,
    WordItem
  },

  methods: {
    deleteWord(id) {
      this.words = this.words.filter(word => word.id !== id)
      localStorage.setItem('words', JSON.stringify(this.words))
      this.updateState()
    },

    updateState() {
      this.words = localStorage.getItem('words') ? JSON.parse(localStorage.getItem('words')).sort((a, b) => a.en.localeCompare(b.en)) : []
    }
  }
}
</script>

<style lang="scss" scoped>
.voc {
  padding: 40px 84px;

  &-search {
    display: flex;
    justify-content: space-between;
  }
}

.words-count {
  font-size: 28px;
  font-weight: 800;
}
</style>