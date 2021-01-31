<template>
  <div class="word">
    <span class="word-en">{{ word.en }}</span>
    <span class="word-ru">{{ word.ru }}</span>
    <button
      class="btn btn-input btn-input-delete"
      @click="deleteWord(word.id)"
    >
      <i class="material-icons">clear</i>
    </button>
  </div>
</template>

<script>
export default {
  props: {
    word: {
      type: Object,
      default: () => {}
    }
  },

  methods: {
    deleteWord(id) {
      this.$emit('delete', id)
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/scss/config';

.word {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  border-bottom: 2px solid rgba(10, 20, 46, .05);

  &-ru, &-en {
    width: 25%;
    font-size: 14px;
  }

  &-en {
    font-weight: 600;
    color: $main-color;
  }
}
</style>