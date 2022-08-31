<template>
  <div class="totalizer-game">
    <div v-if="teamsCount" class="totalizer-game__start">
      <button @click="runGame(0, 10)">Начать матч!</button>
    </div>
    <div v-else class="totalizer-game__finish">
      <p>Игра завершена! Начать новую игру?</p>
      <button class="button-wide" @click="newGame()">Да</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TotalizerGame',
  data() {
    return {
      score: [],
      teamsCount: this.teams,
    }
  },
  props: {
    teams: {
      type: Number,
      default: 0,
    },
    scoreLimits: {
      type: Object,
      default: () => {
        return { min: 0, max: 0, };
      }
    }
  },
  computed: {
    
  },
  methods: {
    getScore(min, max) {
      return Math.floor(Math.random() * (max - min + 1)) + min;
    },
    runGame() {
      if(this.teamsCount === 0) {
        this.finishGame();
        return;
      }
      
      this.teamsCount--;
      this.score.push(this.getScore(this.scoreLimits.min, this.scoreLimits.max));
      this.runGame();
    },
    finishGame() {
      this.$emit('game:finish', this.score);
    },
    newGame() {
      this.score = [];
      this.teamsCount = this.teams;
      this.$emit('game:new', this.score);
    },
  },
}
</script>

<style scoped>

</style>
