<template>
  <div class="container_main">
    <p id="mainTitle">Team {{this.team}}</p>
    <p id="mainSub">@ {{comp.name}}</p>
    
    <div class="container_match">
      <Matches v-bind:matches="matches" v-bind:exclude="'allianceColor teamNumber'" v-bind:sortable="false"></Matches>
    </div>
  </div>
</template>

<script>
import CompService from '../CompService.js'
import MatchService from '../MatchService.js'
import Matches from './Matches_Component.vue'

export default {
  name: 'Team',
  components: {
    Matches
  },
  data() {
    return {
      comp: {},
      matches: [],
      team: ""
    }
  },
  async created() {
    //Figure out the name of the current competition
    try {
      this.comp = await CompService.getOneComp(this.$route.params.competition);
      if(this.comp.name == undefined) throw "No competition with this id exists.";
    } catch(err) {
      console.log(err);
      alert("Please click a valid team link from the teams page!");
      this.$router.push({name: 'home'});
    }

    this.team = this.$route.params.team;

    try {
      this.matches = await MatchService.getMatchesByTeam(this.comp._id, this.team);
    } catch(err) {
      alert("Error: " + err.message);
    }

    console.log(this.matches);
  }
}
</script>