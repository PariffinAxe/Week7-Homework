<template>
  <div>
    <h2>League Table <button v-if="selectedTeam" v-on:click="deselectTeam">See Chart</button></h2>
    <table>
      <tr>
        <th>Team</th>
        <th>Games Played</th>
        <th>Wins</th>
        <th>Draws</th>
        <th>Losses</th>
        <th>Goals For</th>
        <th>Goals Against</th>
        <th>Goal Difference</th>
        <th>Points</th>
        <th>Compare</th>
      </tr>
      <tr v-for="(team, index) in teams" :key="index" :class="team===favouriteTeam?'is-favourite':team===selectedTeam?'is-selected':null">
          <td class="team_name" v-on:click="handleClick(team)">{{team.teamName}}</td>
          <td>{{team.all.matchsPlayed}}</td>
          <td>{{team.all.win}}</td>
          <td>{{team.all.draw}}</td>
          <td>{{team.all.lose}}</td>
          <td>{{team.all.goalsFor}}</td>
          <td>{{team.all.goalsAgainst}}</td>
          <td>{{team.goalsDiff}}</td>
          <td>{{team.points}}</td>
          <th><input type="checkbox" v-on:change="handleChange(team)"></th>
      </tr>
    </table>
    <div class="bottom_buffer"></div>
  </div>
</template>

<script>
import {eventBus} from '../main.js'

export default {
  name: 'team-list',
  props: ['teams', 'favourite'],
  data(){
    return {
      favouriteTeam: null,
      selectedTeam: null,
      comparisonTeams: []
    }
  },
  methods: {
    handleClick(team){
      this.selectedTeam=team
      eventBus.$emit('team-select', team)
    },
    deselectTeam(){
      this.selectedTeam=null
      eventBus.$emit('team-select', null)
      this.displayChart()
    },
    handleChange(team){
      if(this.comparisonTeams.indexOf(team) == -1){
        this.comparisonTeams.push(team)
      } else {
        this.comparisonTeams = this.comparisonTeams.filter((current) => current != team)
      }
      this.displayChart()
    },
    displayChart(){
      if (this.comparisonTeams[0]){
        eventBus.$emit('compare-teams', this.comparisonTeams)
      } else {
        eventBus.$emit('compare-teams', this.teams)
      }
    }
  },
  mounted(){
    eventBus.$on('select-favourite', team => this.favouriteTeam = team)
  }
}
</script>

<style>
  .is-favourite {
    background-color: green;
    color: whitesmoke;
  }

  .is-selected {
    background-color: yellow;
  }

  .bottom_buffer {
    height: 60px;
  }
</style>