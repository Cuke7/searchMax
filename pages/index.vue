<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-text-field
        label="Faire une recherche"
        v-model="searchQuery"
        autocomplete="off"
        hide-details
      ></v-text-field>
      <v-chip-group
        filter
        active-class="primary--text"
        class="my-3"
        column
        v-model="selectedType"
      >
        <v-chip v-for="(type, index) in types" :key="index">
          {{ type }}
        </v-chip>
      </v-chip-group>
      <div class="mt-6">
        <v-row v-for="(result, index) in results" :key="index" class="mx-3">
          <v-col> {{ result.nom }} - {{ result.type }} </v-col>
        </v-row>
      </div>
    </v-col>
  </v-row>
</template>

<script>
// Library pour le search
const { search } = require("fast-fuzzy");

export default {
  data() {
    return {
      searchQuery: "",
      dataSearch: [],
      types: [
        "Grass",
        "Bug",
        "Fire",
        "Water",
        "Electric",
        "Ground",
        "Poison",
        "Fairy",
        "Fighting",
        "Psychic",
        "Rock",
        "Ghost",
        "Ice",
        "Dragon",
        "Normal",
        "Dark",
        "Steel",
        "Flying"
      ],
      selectedType: 3
    };
  },
  methods: {
    parseSheet(sheetId, sheetName = null) {
      return this.$gsparser.parse(sheetId, sheetName);
    }
  },
  computed: {
    results() {
      // Si on ne tape rien, renvoyer tous les résultats
      if (this.searchQuery == "") {
        return this.dataSearch.filter(
          pokemon => pokemon.type == this.types[this.selectedType]
        );
      } else {
        // Sinon on renvoie les résultats de la recherche
        let arr = search(
          this.searchQuery,
          this.dataSearch.filter(
            pokemon => pokemon.type == this.types[this.selectedType]
          ),
          {
            threshold: 0.6,
            keySelector: obj => obj.nom
          }
        );
        return arr;
      }
    }
  },
  async mounted() {
    const sheetId = "1v_Mc_FRtV7_yrBcEH6_2S1EVyqtLI5DhLAbudJ419f4";
    const data1 = await this.parseSheet(sheetId);
    this.dataSearch = data1;
  }
};
</script>
