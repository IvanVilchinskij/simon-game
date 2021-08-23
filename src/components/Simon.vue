<template>
  <div class="simon">
    <div class="simon__game">
      <div class="simon__wrapper">
        <div
          class="simon__quadrant simon__quadrant--green"
          @click="clickBtn(1)"
          :class="{ active: isActive[1] }"
        ></div>
        <div
          class="simon__quadrant simon__quadrant--red"
          @click="clickBtn(2)"
          :class="{ active: isActive[2] }"
        ></div>
        <div
          class="simon__quadrant simon__quadrant--yellow"
          @click="clickBtn(3)"
          :class="{ active: isActive[3] }"
        ></div>
        <div
          class="simon__quadrant simon__quadrant--blue"
          @click="clickBtn(4)"
          :class="{ active: isActive[4] }"
        ></div>
      </div>
    </div>
    <div class="simon__control-bar">
      <p class="simon__game-text">{{ gameText }}</p>
      <p class="simon__score">Раунд: {{ round }}</p>
      <button class="simon__btn" @click="startGame(false)">
        {{ controlBtn }}
      </button>
    </div>
  </div>
</template>

<script>
import soundBlue from "../assets/audio/blue.mp3";
import soundGreen from "../assets/audio/green.mp3";
import soundYellow from "../assets/audio/yellow.mp3";
import soundRed from "../assets/audio/red.mp3";
import soundWin from "../assets/audio/winGame.mp3";
import soundError from "../assets/audio/error.mp3";

export default {
  props: {
    difficulty: String,
  },
  data() {
    return {
      controlBtn: "Начать",
      gameText: "Simon the game",
      isClickabel: false,
      difficultyDurations: {
        easy: 1500,
        normal: 1000,
        hard: 400,
      },
      round: 1,
      sequence: [],
      playerSequence: [],
      sequenceInterval: null,
      isActive: {
        1: false,
        2: false,
        3: false,
        4: false,
      },
      sounds: {
        1: new Audio(soundGreen),
        2: new Audio(soundRed),
        3: new Audio(soundYellow),
        4: new Audio(soundBlue),
      },
    };
  },
  watch: {
    difficulty() {
      this.startGame(true);
    },
  },
  methods: {
    startGame(isChangeDifficulty) {
      this.controlBtn = "Начать";
      this.sequence = [];
      this.playerSequence = [];
      this.round = 1;
      this.gameText = "Simon the game";
      clearInterval(this.sequenceInterval);

      if (!isChangeDifficulty) {
        this.controlBtn = "Начать заново";
        this.showSequence();
      }
    },
    clickBtn(i) {
      if (this.isClickabel) {
        this.playSound(i);
        this.setActiveClass(i);
        this.playerSequence.push(i);
        this.checkWinOrLose();
      }
    },
    playSound(i) {
      this.sounds[i].play();
    },
    setActiveClass(i) {
      this.isActive[i] = true;

      setTimeout(() => {
        this.isActive[i] = false;
      }, 300);
    },
    checkWinOrLose() {
      const speed = this.difficultyDurations[this.difficulty];

      for (let i = 0; i < this.playerSequence.length; i++) {
        if (this.playerSequence[i] !== this.sequence[i]) {
          const errorSound = new Audio(soundError);

          errorSound.volume = 0.4;
          errorSound.play();

          this.gameText = "Вы проиграли";
        } else if (this.playerSequence.length === this.sequence.length) {
          if (this.round === 15) {
            const winSound = new Audio(soundWin);

            winSound.play();

            this.gameText = "Победа!";
            this.isClickabel = false;

            setTimeout(() => {
              this.controlBtn = "Новая игра";
            }, 3000);
          } else {
            this.playerSequence = [];
            this.round++;
            let timeBetweenShow = speed === 400 ? 1000 : 0;

            setTimeout(() => this.showSequence(), timeBetweenShow);
          }
        }
      }
    },
    showSequence() {
      let currentIndex = 0;
      let randomBtn = Math.floor(Math.random() * 4 + 1);
      const speed =
        this.sequence.length === 0
          ? 1000
          : this.difficultyDurations[this.difficulty];

      this.isClickabel = false;
      this.sequence.push(randomBtn);

      this.sequenceInterval = setInterval(() => {
        if (currentIndex >= this.sequence.length) {
          clearInterval(this.sequenceInterval);
          this.isClickabel = true;
        }

        this.playSound(this.sequence[currentIndex]);
        this.setActiveClass(this.sequence[currentIndex]);

        currentIndex++;
      }, speed);
    },
  },
};
</script>