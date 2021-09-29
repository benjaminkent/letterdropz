<template>
  <div class="home">
    <header>
      <h1>LetterDropz</h1>
    </header>
    <h3>Current Score: {{ currentScore }}</h3>
    <p class="question-definition">{{ currentQuestion?.definition }}</p>
    <ul class="letter-list">
      <li
        v-for="(letter, index) in puzzle.letters.split('')"
        :key="index"
        :class="[
          'letter-list__item',
          {
            drop: guessedLetterIndexes.includes(index),
            incorrect: incorrectGuess,
          },
        ]"
        @click="guessLetter(index, letter)"
      >
        <p class="letter-list__letter">{{ letter }}</p>
      </li>
    </ul>
    <button class="reset-btn" @click="reset">RESET</button>
    <p v-if="answeredCorrectly" class="correct">{{ correctWord }}!</p>
  </div>
</template>

<script lang="ts">
import {
  defineComponent,
  ref,
  Ref,
  onMounted,
  computed,
  watchEffect,
} from 'vue'

interface Question {
  id: number
  definition: string
  answer: string
}

export default defineComponent({
  name: 'Home',
  setup() {
    const puzzle = ref({
      questions: [
        {
          id: 1,
          definition:
            'To receive or take in the sense of letters, espeically by sight.',
          answer: 'read',
        },
        {
          id: 2,
          definition: 'To defy or challenge someone to do something.',
          answer: 'dare',
        },
        {
          id: 3,
          definition: 'Regarded with deep affection.',
          answer: 'dear',
        },
      ] as Question[],
      letters: 'ared',
    })
    const correctDisplayWordList: Ref<string[]> = ref([
      'Awesome',
      'Nice',
      'Good Job',
      'Well Done',
    ])
    const correctWord: Ref<string | null> = ref(null)
    let guesses: Ref<{ index: number; letter: string }[]> = ref([])
    let currentQuestionId: Ref<number | null> = ref(null)
    let answeredCorrectly: Ref<boolean> = ref(false)
    let currentScore: Ref<number> = ref(0)
    let incorrectGuess: Ref<boolean> = ref(false)

    const goToNextQuestion = () => {
      if (!currentQuestionId.value) {
        currentQuestionId.value = 1
        return
      }

      currentQuestionId.value++
    }

    const reset = () => {
      guesses.value = []
    }

    const guessLetter = (index: number, letter: string) => {
      guesses.value.push({ index, letter })
    }

    const currentQuestion = computed(() =>
      puzzle.value.questions.find(
        (question) => question.id === currentQuestionId.value
      )
    )

    const guessedLetterIndexes = computed(() => {
      return guesses.value.map((guess) => guess.index)
    })

    const guessedLetters = computed(() => {
      return guesses.value.map((guess) => guess.letter)
    })

    const getCorrectWord = () => {
      const randomNumber = Math.floor(
        Math.random() * correctDisplayWordList.value.length
      )

      correctWord.value = correctDisplayWordList.value[randomNumber]
    }

    const testCorrect = () => {
      if (guessedLetters.value.join('') === currentQuestion.value?.answer) {
        answeredCorrectly.value = true
        getCorrectWord()
        currentScore.value += 1000

        setTimeout(() => {
          reset()
          answeredCorrectly.value = false
          goToNextQuestion()
        }, 2000)
      } else {
        incorrectGuess.value = true

        setTimeout(() => {
          incorrectGuess.value = false
          reset()
        }, 1000)
      }
    }

    watchEffect(() => {
      if (guesses.value.length === puzzle.value.letters.length) {
        testCorrect()
      }
    })

    onMounted(() => goToNextQuestion())

    return {
      puzzle,
      guessedLetterIndexes,
      guessLetter,
      reset,
      currentQuestion,
      answeredCorrectly,
      currentScore,
      guessedLetters,
      currentQuestionId,
      correctWord,
      incorrectGuess,
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
.question-definition {
  margin: 0 1rem 3rem;
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
  .incorrect {
    border-color: red;
    color: red;
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
.correct {
  position: absolute;
  font-size: 3rem;
  transform: rotate(-30deg);
  color: $coral;
}
</style>
