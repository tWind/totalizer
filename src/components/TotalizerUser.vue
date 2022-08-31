<template>
  <div class="totalizer-user">
    <div class="totalizer-user__id">
      User #{{ id }}
    </div>
    <div class="totalizer-user__set">
      <div class="totalizer-user__input"
        v-for="(team, index) in teamsCount" :key="index"
      >
        <input
          type="number"
          :min="betLimits.min" :max="betLimits.max"
          :disabled="betConfirmed"
          v-model.number="scores[index]"
          @blur="setBet($event.target.value, index)"
        />
      </div>
    </div>
    <div class="totalizer-user__confirm">
      <button
        v-if="!betConfirmed"
        @click="confirmBet()"
        :disabled="!betPossible">Сделать ставку</button>
      <p v-else>Ваша ставка принята! Спасибо.</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TotalizerUser',
  data() {
    return {
      scores: [],
      betConfirmed: false,
    };
  },
  props: {
    teamsCount: {
      type: Number,
      default: 0,
    },
    betLimits: {
      type: Object,
      default: () => {
        return { min: 0, max: 0 }
      },
    },
    id: {
      type: Number,
      default: 0,
    }
  },
  computed: {
    betPossible() {
      return [...this.scores].length === this.teamsCount;
    }
  },
  methods: {
    setBet(val, index) {
      const score = Math.abs(Math.round(val));

      this.scores[index] = this.validateBet(score);        
    },
    confirmBet() {
      this.betConfirmed = true;
      this.$emit('user:confirm-bet', this.scores, this.id);
    },
    validateBet(bet) {
      if(bet > this.betLimits.max)
        return this.betLimits.max;
      else if(bet < this.betLimits.min)
        return this.betLimits.min;

      return bet;
    }
  },
}
</script>

<style lang="scss" scoped>
.totalizer-user {
  display: flex;
  align-items: center;
  padding: .5rem;
}

.totalizer-user__id {
  white-space: nowrap;
  margin-right: 1.5rem;
}

.totalizer-user__set {
  display: flex;
}

.totalizer-user__input {
  input {
    width: 80px;
    height: 20px;
    margin-right: .5rem;
    text-align: center;
  }

  &:not(:last-child) {
    &:after {
      content: ":";
      display: inline-block;
      margin-right: .5rem;
    }
  }
}

.totalizer-user__confirm {
  p {
    margin: .25rem;
    font-size: 14px;
  }
}
</style>
