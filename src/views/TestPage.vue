<template>
  <div class="test-page">
    <div
      v-if="!isActiveTrainer"
      class="test-page-buttons"
    >
      <button
        class="btn btn-fill btn-fill-main"
        @click="openTrainer('en')"
      >
        Тренировка речи
      </button>
      <button
        class="btn btn-fill btn-fill-main"
        @click="openTrainer('ru')"
      >
        Тренировка перевода
      </button>
    </div>
    <Trainer
      v-if="isActiveTrainer && words.length"
      :mode="mode"
      @close="closeTrainer"
    />
    <span
      v-if="isActiveTrainer && !words.length"
      class="no-words no-words-trainer"
    >
      Невозможно запустить тренажер, т.к. в базе нет слов. Перейдите на вкладку Vocabulary, чтобы пополнить базу данных
    </span>
  </div>
</template>

<script>
import Trainer from '@/components/Trainer.vue'

export default {
  props: {
    words: {
      type: Array,
      default: () => []
    }
  },

  data() {
    return {
      isActiveTrainer: false,
      mode: 'ru'
    }
  },

  components: {
    Trainer
  },

  methods: {
    openTrainer(lang) {
      this.isActiveTrainer = true
      this.mode = lang
    },

    closeTrainer() {
      this.isActiveTrainer = false
    }
  }
}
</script>

<style lang="scss" scoped>
.test-page {
  &-buttons {
    padding: 60px 80px 0 80px;
    text-align: center;

    .btn {
      margin-right: 20px;
    }
  }
}

.no-words {
  display: block;
  text-align: center;
  font-size: 28px;
  font-weight: 600;

  &-trainer {
    font-size: 20px;
    color: #fff;
  }
}
</style>