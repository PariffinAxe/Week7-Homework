<template>
  <div id="app">
    <header>
      <h1>English Premier League</h1>
    </header>
    
    <main id="basic_wrapper">
      <div class="buffer_line"></div>
      <team-list v-if="teams[0]" :teams="teams"></team-list>
      <div class="buffer_line"></div>
      <team-detail v-if="selectedTeam" :team="selectedTeam"></team-detail>
      <team-chart v-if="!selectedTeam" :teams="comparedTeams"></team-chart>
      <div class="buffer_line"></div>
    </main>

    <footer>

    </footer>
      
  </div>
</template>

<script>
import TeamChart from './components/TeamChart.vue'
import TeamDetail from './components/TeamDetail.vue'
import TeamList from './components/TeamList.vue'
import {eventBus} from './main.js'

export default {
  name: 'App',
  data(){
    return {
      teams: [],
      selectedTeam: null,
      favouriteTeam: null,
      comparedTeams: []
    }
  },
  components: {
    'team-list': TeamList,
    'team-detail': TeamDetail,
    'team-chart': TeamChart
  },
  mounted(){
    var proxyUrl = 'https://cors-anywhere.herokuapp.com/'
    var targetUrl = 'https://www.api-football.com/demo/v2/leaguetable/524'
    fetch(proxyUrl + targetUrl)
    .then(res => res.json())
    .then(data => {
      this.teams = data.api.standings[0]
      eventBus.$emit('compare-teams', this.teams)
    })
    eventBus.$on('team-select', team => this.selectedTeam = team)
    eventBus.$on('compare-teams', teams => this.comparedTeams = teams)

  }
}
</script>

<style>
  #basic_wrapper {
    display: grid;
    grid-template-columns: 1fr 80fr 1fr 80fr 1fr;
    margin: 0px 28px;
  }

  .buffer_line {
    background-color: lightblue;
  }

  header {
    background-color: blue;
    color: whitesmoke;
    padding: 4px 16px;
    border-radius: 32px;
  }

  footer {

    background-color: blue;
    height: 10px;
    border-radius: 10px;
  }

  h1 {
    font-size: 72px;
  }

  button {
    font-size: 32px;
    border-radius: 8px;
  }


</style>
