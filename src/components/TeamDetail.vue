<template>
  <div>
    <h2 :class="team===favouriteTeam?'is-favourite':null">{{team.teamName}} <button v-on:click="handleFavourite" v-if="team!=favouriteTeam">Make Favourite Team</button></h2>
    <div class="form">Form: <p v-for="(form, index) in team.forme" :key="index" :class="form==='W'?'win':form==='D'?'draw':'loss'">{{form}}</p></div>
    <div id="team_details">
      <div>
        <h3>Total:</h3>
        <p>Games Played: {{team.all.matchsPlayed}}</p>
        <p>Wins: {{team.all.win}}</p>
        <p>Draws: {{team.all.draw}}</p>
        <p>Losses: {{team.all.lose}}</p>
        <p>Goals For: {{team.all.goalsFor}}</p>
        <p>Goals Against: {{team.all.goalsAgainst}}</p>
        <p>Goal Difference: {{team.goalsDiff}}</p>
        <p>Points: {{team.points}}</p>
      </div>
      <div>
        <h5>Home:</h5>
        <p>Games Played: {{team.home.matchsPlayed}}</p>
        <p>Wins: {{team.home.win}}</p>
        <p>Draws: {{team.home.draw}}</p>
        <p>Losses: {{team.home.lose}}</p>
        <p>Goals For: {{team.home.goalsFor}}</p>
        <p>Goals Against: {{team.home.goalsAgainst}}</p>
        <p>Goal Difference: {{team.home.goalsFor - team.home.goalsAgainst}}</p>
        <p>Points: {{3 * team.home.win + team.home.draw}}</p>
      </div>
      <div>
        <h5>Away:</h5>
        <p>Games Played: {{team.away.matchsPlayed}}</p>
        <p>Wins: {{team.away.win}}</p>
        <p>Draws: {{team.away.draw}}</p>
        <p>Losses: {{team.away.lose}}</p>
        <p>Goals For: {{team.away.goalsFor}}</p>
        <p>Goals Against: {{team.away.goalsAgainst}}</p>
        <p>Goal Difference: {{team.away.goalsFor - team.away.goalsAgainst}}</p>
        <p>Points: {{3 * team.away.win + team.away.draw}}</p>
      </div>
    </div>
  </div>
</template>

<script>
import { eventBus } from '../main.js'

export default {
  name:'team-detail',
  props:['team'],
  data(){
    return {
      favouriteTeam: null
    }
  },
  methods: {
    handleFavourite(){
      this.favouriteTeam=this.team
      eventBus.$emit('select-favourite', this.team)
    }
  }
}
</script>

<style>
  #team_details {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
  }

  h2 {
    font-size: 64px;
    padding: 0 12px;
    display: flex;
    justify-content: space-around;
  }

  .form {
    display: flex;
    font-size: 32px;
  }

  .win {
    background-color: green;
    width: 40px;
    height: 40px;
    text-align: center;
  }

  .draw {
    background-color: yellow;
    width: 40px;
    height: 40px;
    text-align: center;
  }

  .loss {
    background-color: red;
    width: 40px;
    height: 40px;
    text-align: center;
  }

  .is-favourite {
    background-color: green;
    color: whitesmoke;
  }

</style>