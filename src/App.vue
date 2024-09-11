<template>
  <div id="app">
    <nav class="navbar navbar-expand-lg bg-light custom-navbar">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">Sportz Interactive</a>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <select
              v-model="selectedTeam"
              @change="filterPlayers"
              class="form-select team-select"
            >
              <option value="ALL">ALL</option>
              <option value="IND">IND</option>
              <option value="PAK">PAK</option>
              <option value="AUS">AUS</option>
              <option value="ENG">ENG</option>
            </select>
          </ul>
          <form class="d-flex search-form">
            <input
              class="form-control me-2"
              type="search"
              placeholder="Search Players"
              v-model="searchQuery"
              @input="filterPlayers"
            />
          </form>
        </div>
      </div>
    </nav>

    <div class="container text-center mt-4">
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
    this.filterPlayers();
  },
  methods: {
    filterPlayers() {
      let filtered = this.originalPlayers;

      if (this.selectedTeam !== "ALL") {
        filtered = filtered.filter(
          (player) => player.team_name === this.selectedTeam
        );
      }

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
  },
};
</script>

<style scoped>
.custom-navbar {
  background-color: #f8f9fa;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.navbar-brand {
  font-weight: bold;
  color: #0275d8;
  font-size: 1.5rem;
}

.team-select {
  margin-right: 20px;
  font-weight: 500;
}

.search-form input {
  border: 2px solid #0275d8;
  border-radius: 20px;
  padding: 8px 15px;
}

.search-form input::placeholder {
  font-style: italic;
  color: #666;
}

.search-form input:focus {
  box-shadow: 0 0 5px rgba(0, 123, 255, 0.5);
}
</style>
