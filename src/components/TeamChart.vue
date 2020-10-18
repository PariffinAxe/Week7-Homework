<template>
  <GChart
    type="ColumnChart"
    :data="chartData"
  />
</template>

<script>
import { GChart } from 'vue-google-charts'
import {eventBus} from '../main.js'

export default {
  data () {
    return {
      chartData: null
    }
  },
  props:['teams'],
  mounted(){
    eventBus.$on('compare-teams', (teams) => {
      this.updateTable(teams)
    })
    this.updateTable(this.teams)
  },
  methods: {
    updateTable(teams){
      this.chartData = [['Team', 'Goals', 'GoalsAgainst', "GoalDifference"]];
      for (let team of teams){
        this.chartData.push([team.teamName, team.all.goalsFor, team.all.goalsAgainst, team.goalsDiff])
      }
    }
  }
}
</script>

<style>

</style>