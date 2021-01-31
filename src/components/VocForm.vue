<template>
  <div class="voc-form">
    <h3 class="voc-form-title">Пополните словарь</h3>
    <div class="voc-form-input">
      <input
        v-model="enWord"
        type="text"
        class="input input-voc input-voc-en"
        placeholder="En"
        @keyup.enter="addWord"
      >
      <input
        v-model="ruWord"
        type="text"
        class="input input-voc input-voc-ru"
        placeholder="Ru"
        @keyup.enter="addWord"
      >
      <button
        class="btn btn-input btn-input-add"
        :disabled="(!enWord && ruWord) || (enWord && !ruWord) || (!enWord && !ruWord)"
        @click="addWord"
      >
        <i class="material-icons">add</i>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      enWord: null,
      ruWord: null
    }
  },

  methods: {
    generateId() {
      let id = ''

      for (let i = 0; i < 12; i++) {
        const num = Math.round(Math.random() * 122)
        const char = String.fromCharCode(num < 48 ? 48 : num)
        id += char
      }

      return id
    },

    addWord() {
      if ((!this.enWord && this.ruWord) || (this.enWord && !this.ruWord) || (!this.enWord && !this.ruWord)) {
        return false
      } else {
        const oldState = JSON.parse(localStorage.getItem('words')) || []
        const newWord = {
          id: this.generateId(),
          created: new Date().getTime(),
          en: this.enWord,
          ru: this.ruWord
        }

        this.enWord = this.ruWord = ''

        localStorage.setItem('words', JSON.stringify([ ...oldState, newWord ]))
        this.$emit('update')
      }
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/scss/config';

.voc {
  padding: 40px 84px;

  &-form {
    margin-bottom: 45px;

    &-title {
      margin-bottom: 17px;
      font-size: 14px;
    }

    &-input {
      display: flex;

      .input-voc {
        &-en, &-ru {
          width: 50%;
        }
      }

      .btn:disabled {
        opacity: .5;
        cursor: not-allowed;
      }
    }
  }
}
</style>