<template>
  <div class="home">
    <header>
      <h1>LetterDropz</h1>
    </header>
    <p>{{ puzzle.questions[1].definition }}</p>
    <ul class="letter-list">
      <li
        v-for="(letter, index) in puzzle.letters.split('')"
        :key="index"
        :class="[
          'letter-list__item',
          { drop: guessedLetterIndexes.includes(index) },
        ]"
        @click="makeGuess(index)"
      >
        <p class="letter-list__letter">{{ letter }}</p>
      </li>
    </ul>
    <button class="reset-btn" @click="reset">RESET</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, Ref } from 'vue'

export default defineComponent({
  name: 'Home',
  setup() {
    const puzzle = ref({
      questions: [
        {
          id: 1,
          definition: 'An action',
          answer: 'read',
        },
        {
          id: 2,
          definition: 'To Test someone',
          answer: 'dare',
        },
        {
          id: 3,
          definition: 'A nice term for someone',
          answer: 'dear',
        },
      ],
      letters: 'ared',
    })

    let guessedLetterIndexes: Ref<number[]> = ref([])

    const reset = () => {
      guessedLetterIndexes.value = []
    }

    const makeGuess = (index: number) => {
      guessedLetterIndexes.value.push(index)
    }

    return {
      puzzle,
      guessedLetterIndexes,
      makeGuess,
      reset,
    }
  },
})
</script>

<style lang="scss" scoped>
$coral: #f97068;
$blue: #57c4e5;

.home {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  padding: 1rem 2rem;
}
header {
  background-color: #57c4e5;
  width: 100%;
  padding: 0.5rem 0;
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 0;
  h1 {
    margin: 0;
  }
}
.letter-list {
  margin: 0;
  padding: 0;
  list-style-type: none;
  display: flex;
  height: 220px;
  align-items: flex-start;
  &__item {
    border: 1px solid #222;
    padding: 1rem;
    border-radius: 100%;
    margin: 0 0.5rem;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
  }
  &__letter {
    margin: 0;
    width: 1rem;
    height: 1rem;
    display: flex;
    justify-content: center;
    align-items: center;
    font-weight: bold;
  }
  .drop {
    transform: translateY(100px);
    transition: all 0.5s ease-in-out;
    border-color: green;
    color: green;
  }
}
.reset-btn {
  padding: 1rem 0;
  font-size: 14px;
  border-radius: 10px;
  width: 80%;
  border: 1px solid #d1d646;
  background-color: #d1d646;
}
</style>
