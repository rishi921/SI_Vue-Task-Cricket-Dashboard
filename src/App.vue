<template>
  <div id="app">
    <nav class="navbar navbar-expand-lg bg-light">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">Sportz Interactive</a>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <select v-model="selectedTeam" @change="filterPlayers">
              <option value="ALL">ALL</option>
              <option value="IND">IND</option>
              <option value="PAK">PAK</option>
              <option value="AUS">AUS</option>
              <option value="ENG">ENG</option>
            </select>
          </ul>
          <form class="d-flex" role="search">
            <input
              class="form-control me-2"
              type="search"
              placeholder="Search"
              v-model="searchQuery"
              @input="filterPlayers"
            />
            <button
              class="btn btn-outline-success"
              type="button"
              @click="searchPlayers"
            >
              Search
            </button>
          </form>
        </div>
      </div>
    </nav>

    <div class="container text-center">
      <PlayerList :players="filteredPlayers" role="2" title="Batsmen" />
      <PlayerList :players="filteredPlayers" role="4" title="Bowlers" />
      <PlayerList :players="filteredPlayers" role="3" title="All-rounders" />
    </div>
  </div>
</template>

<script>
import PlayerList from "./components/PlayerList.vue";
import playersData from "./data/players.json";

export default {
  name: "App",
  components: {
    PlayerList,
  },
  data() {
    return {
      originalPlayers: playersData.originalPlayers,
      filteredPlayers: [],
      selectedTeam: "ALL",
      searchQuery: "",
    };
  },
  mounted() {
    this.filterPlayers(); // Ensure that it applies default filters on mount
  },
  methods: {
    filterPlayers() {
      let filtered = this.originalPlayers;

      // Filter by selected team
      if (this.selectedTeam !== "ALL") {
        filtered = filtered.filter(
          (player) => player.team_name === this.selectedTeam
        );
      }

      // Filter by search query
      const query = this.searchQuery.toLowerCase();
      if (query) {
        filtered = filtered.filter(
          (player) =>
            player.name.toLowerCase().includes(query) ||
            player.team_name.toLowerCase().includes(query)
        );
      }

      this.filteredPlayers = filtered;
    },
    searchPlayers() {
      // This method is no longer needed if filterPlayers handles the search
      this.filterPlayers();
    },
  },
};
</script>
