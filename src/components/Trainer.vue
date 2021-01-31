<template>
  <div class="trainer">
    <h3 class="trainer-title">Введите перевод слова или фразы на русский</h3>
    <div class="trainer-input">
      <div class="trainer-input-word">{{ mode === 'en' ? currentWord.en : currentWord.ru }}</div>
      <input
        v-model="tryWord"
        type="text"
        class="input input-trainer"
        placeholder="Введите слово"
      >
      <button
        class="btn btn-input btn-input-main"
        :class="{ 'btn-input-correct': isCorrectChoice === true, 'btn-input-incorrect': isCorrectChoice === false }"
        :disabled="isCorrectChoice !== null"
        @click="checkWord"
      >
        <i class="material-icons">{{ isCorrectChoice === false ? 'clear' : 'done' }}</i>
      </button>
    </div>
    <div class="trainer-buttons">
      <button
        class="btn btn-fill btn-fill-main"
        @click="nextWord"
      >
        Следующее
      </button>
      <button
        class="btn btn-round btn-round-main"
        @click="endTraining"
      >
        Завершить
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    mode: {
      type: String,
      default: 'ru'
    }
  },

  data() {
    return {
      tryWord: null,
      currentWord: {},
      isCorrectChoice: null
    }
  },

  methods: {
    nextWord() {
      const wordsDB = JSON.parse(localStorage.getItem('words'))
      this.currentWord = wordsDB[Math.floor(Math.random() * wordsDB.length)]
      this.tryWord = this.isCorrectChoice = null
    },

    checkWord() {
      if (this.mode === 'ru') {
        if (this.tryWord.toLowerCase() === this.currentWord.en.toLowerCase()) this.isCorrectChoice = true
        else this.isCorrectChoice = false
      }

      if (this.mode === 'en') {
        if (this.tryWord.toLowerCase() === this.currentWord.ru.toLowerCase()) this.isCorrectChoice = true
        else this.isCorrectChoice = false
      }
    },

    endTraining() {
      this.$emit('close')
    }
  },

  mounted() {
    this.nextWord()
  }
}
</script>

<style lang="scss" scoped>
.trainer {
  width: 500px;
  margin: 60px auto;

  &-title {
    margin-bottom: 17px;
    font-size: 14px;
    color: #fff;
  }

  &-input {
    display: flex;
    align-items: center;
    height: 47px;

    &-word {
      display: flex;
      justify-content: center;
      align-items: center;
      min-width: 120px;
      height: 100%;
      padding: 10px;
      background-color: rgba(255, 255, 255, .5);
      font-weight: 600;
      font-size: 14px;
    }

    .input-trainer {
      background-color: #3b4358;
      color: #fff;
    }

    .btn:disabled {
      cursor: not-allowed;
    }
  }

  &-buttons {
    margin-top: 30px;

    .btn {
      margin-right: 20px;
    }
  }
}
</style>