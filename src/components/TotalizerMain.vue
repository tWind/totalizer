<template>
  <div class="totalizer-main">
    <div class="totalizer-main__betting">
      <totalizer-user
        v-for="user in users" :key="user.id"
        :id="user.id"
        :teamsCount="teams.length"
        :betLimits="scoreLimits"
        @user:confirm-bet="updateUser"
      />

      <div class="totalizer-main__betting-add">
        <button @click="addUser">Добавить пользователя</button>
      </div>
    </div>

    <div class="totalizer-main__proceed">
      <div class="totalizer-main__teams">
        <totalizer-team
          v-for="team in teams" :key="team.name"
          :team="team"
        />
      </div>
      
      <div class="totalizer-main__game">
        <totalizer-game
          :teams="teams.length"
          :scoreLimits="scoreLimits"
          @game:finish="showGameResult"
          @game:new="clearResults"
        />
      </div>
    </div>

    <div class="totalizer-main__result">
      <totalizer-result>
        <p v-if="alertMsg">{{ alertMsg }}</p>
        <ul v-if="totalResult">
          <li v-for="result in totalResult" :key="result.id">
            <span>User#{{ result.id }}</span>
            <span>Баллы: {{ result.points }}</span>
          </li>
        </ul>
      </totalizer-result>
    </div>
  </div>
</template>

<script>
import TotalizerGame from './TotalizerGame';
import TotalizerUser from './TotalizerUser';
import TotalizerTeam from './ui/TotalizerTeam';
import TotalizerResult from './ui/TotalizerResult'

const TEAMS = [
  'Красная Команда',
  'Синяя Команда'
];
const SCORE_LIMITS = {
  min: 1,
  max: 10,
};
const ALERT_MESSAGES = {
  emptyResults: 'Для получения результата начните игру',
  noBets: 'Пользователи не сделали ставок!'
};

export default {
  name: 'TotalizerMain',
  data() {
    return {
      teams: this.formTeams(),
      scoreLimits: SCORE_LIMITS,
      users: [],
      alertMsg: ALERT_MESSAGES.emptyResults,
      totalResult: null,
    };
  },
  methods: {
    showGameResult(score) {
      this.teams.forEach((team, index) => team.score = [...score][index]);
      
      if(!this.users.filter(user => 'bet' in user).length) {
        this.alertMsg = ALERT_MESSAGES.noBets;
        return;
      }
      this.alertMsg = null;
      this.totalResult = this.getResult(score);
    },
    formTeams() {
      return TEAMS.map(team => {
        return {
          name: team,
          score: 0,
        };
      });
    },
    addUser() {
      this.users.push({ id: this.users.length + 1 });
    },
    updateUser(bet, id) {
      this.users.find(user => user.id === id).bet = bet;
    },
    getResult(score) {
      return this.users.map(user => {
        return {
          id: user.id,
          points: calculateUserPoints(user.bet, score),
        }
      });

      function getWinner(scores) {
        const maxScore =  scores.reduce((a, b) => a > b ? a : b);
        // check if 'winner' score is the only one, 
        // e.g. result is not a draw with max scores in row
        return (scores.filter(score => score === maxScore).length < 2)
          ? scores.indexOf(maxScore)
          : false;
      }

      function calculateUserPoints(userBet, matchResult) {
        if(JSON.stringify(userBet) === JSON.stringify(matchResult)) return 2;
        
        if(getWinner(userBet) && getWinner(matchResult)) {
          return getWinner(userBet) === getWinner(matchResult) ? 1 : 0;
        }

        return 0;
      }
    },
    clearResults() {
      this.alertMsg = ALERT_MESSAGES.emptyResults;
      this.users.splice(0);
      this.teams = this.formTeams();
      this.totalResult = null;
      this.$nextTick(() => this.addUser());
    }
  },
  components: { TotalizerGame, TotalizerUser, TotalizerTeam, TotalizerResult, },
  mounted() {
    this.addUser();
  },
}
</script>

<style lang="scss" scoped>
.totalizer-main {
  display: flex;
  flex-wrap: wrap;
  border: 1px solid gray;
  border-radius: 15px;
  margin: 1rem;
  padding: 1rem;
}

.totalizer-main__betting {
  flex-basis: 60%;
  display: flex;
  flex-direction: column;
}

.totalizer-main__betting-add {
  display: flex;
}

.totalizer-main__proceed {
  flex-basis: 40%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.totalizer-main__game {
  margin-bottom: 1rem;
}

.totalizer-main__result {
  margin: 0 auto;
  flex-basis: 60%;

  ul {
    list-style: none;
    padding: 0;
    margin: .5rem 0;
  }

  li {
    text-align: left;

    &:not(:last-child) {
      margin-bottom: .5rem;
    }
    span {
      &:first-child {
        font-weight: bold;
        margin-right: .5rem;
      }
    }
  }
}

.totalizer-main__teams {
  display: flex;
}
</style>
