<template>
  <div>
    <b-alert style="overflow:hidden" :variant="game.get('team1Score')==0 || game.get('team2Score')==0 ? 'rudi' : 'primary' " show fade class="p-1">
      <span v-if="game.get('team1Score')==0 || game.get('team2Score')==0 " class="rudi-game marker">
        rudi
      </span>
      <span class="float-left date gray">{{getDateAsString(game.get('createdAt'))}}</span>
      <span class="float-right delete gray" @click="deleteGame()">
        <b-icon-trash-fill/>
      </span>
      <b-container class="mt-4 pb-0 px-0">
        <b-row class="mt-n2">
          <b-col>
            <span v-if="game.get('description') != undefined" class="tournament">{{game.get('description')}}</span><br>
          </b-col>
        </b-row>
        <b-row class="m-1">
          <b-col class="px-1">
            <team-list :team="game.get('team1')" class="float-right"/>
          </b-col>
          <b-col cols="1" class="px-1 text-body">
            <h5 class="float-right">{{game.get("team1Score")}}</h5>
          </b-col>
          <b-col cols="1" class="p-0 text-body"><h5>:</h5></b-col>
          <b-col cols="1" class="px-1 text-body">
            <h5 class="float-left">{{game.get("team2Score")}}</h5>
          </b-col>
          <b-col class="px-1">
            <team-list :team="game.get('team2')" class="float-left"/>
          </b-col>
        </b-row>
      </b-container>
      
    </b-alert>
  </div>
</template>

<script>
import TeamList from './TeamList.vue';

export default {
  name: "GameCard",
  components: {
    TeamList
  },
  props: {
    game: Object, //objectId, team1, team2, team1Score, team2Score
  }, 
  data() {
    return {
    };
  },
  mounted: function() {
  },
  methods: {
      deleteGame() {
        if (confirm(`Are you sure you want to delete the game ${this.game.get("team1")} vs. ${this.game.get("team2")} ${this.game.get("team1Score")}:${this.game.get("team2Score")}`)) {
          this.game.destroy().then((game) => {
            console.log(`Game succesfully destroyed.`)
          }, (error) => {
            alert('Failed to destroy game, with error code: ' + error.message);
          });
        }
      }
  },
};
</script>

<style scoped>
.rudi-game {
  position: absolute;
  top: -50px;
  right: 0px;
  left: 0px;
  font-size: 120px;
  opacity: 0.1;
}
.delete {
  position: absolute;
  top: 4px;
  right: 8px;
}
.delete:hover {
  cursor: pointer;
  color: rgb(39, 39, 39);
}
.date {
  font-size: 10pt;
  position: absolute;
  top: 4px;
  left: 8px;
}
.gray {
  color: gray;
}
.tournament{
  font-size: 10pt;
}
</style>