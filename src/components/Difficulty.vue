<template>
  <div class="difficulty">
    <div class="difficulty__text">Уровень сложности:</div>
    <div class="difficulty__btns-wrapper">
      <button
        @click="defDifficulty('easy')"
        class="difficulty__btn difficulty__btn--green"
        :class="{ active: difficulty === 'easy' }"
      >
        Лёкгий
      </button>
      <button
        @click="defDifficulty('normal')"
        class="difficulty__btn difficulty__btn--blue"
        :class="{ active: difficulty === 'normal' }"
      >
        Средний
      </button>
      <button
        @click="defDifficulty('hard')"
        class="difficulty__btn difficulty__btn--red"
        :class="{ active: difficulty === 'hard' }"
      >
        Тяжёлый
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    defDifficulty: Function,
    difficulty: String,
  },
};
</script>