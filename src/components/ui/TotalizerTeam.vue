<template>
  <div class="totalizer-team">
    <div class="totalizer-team__name">
      <b>{{ team.name }}</b>
    </div>
    <div class="totalizer-team__score">
      {{ team.score }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'TotalizerTeam',
  props: {
    team: {
      type: Object,
      default: () => {
        return {
          name: 'Команда не назначена',
          score: 0,
        }
      }
    }
  }
}
</script>

<style scoped>
.totalizer-team {
  padding: .5rem;
  margin: .5rem;
  border: 1px solid gray;
  border-radius: 10px;
}

.totalizer-team__score {
  font-size: 20px;
}
</style>
