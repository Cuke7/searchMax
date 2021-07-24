<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-text-field
        label="Faire une recherche"
        v-model="searchQuery"
        autocomplete="off"
        hide-details
      ></v-text-field>
      <div class="mt-12">
        <v-row v-for="(result, index) in results" :key="index" class="my-3">
          {{ result.prenom }}
        </v-row>
      </div>
    </v-col>
  </v-row>
</template>

<script>
// Library pour le search
const { search } = require("fast-fuzzy");

// data_magical_items contient un tableau JS avec toutes nos données
import data_magical_items from "~/static/magical_objects.json";

export default {
  data() {
    return {
      searchQuery: "",
      dataSearch: [],
      searcher: null
    };
  },
  methods: {
    parseSheet(sheetId, sheetName = null) {
      return this.$gsparser.parse(sheetId, sheetName);
    }
  },
  computed: {
    // Un objet pour rechercher, on le configure avec des options
    // searcher() {
    // },
    // Un tableau avec les résultats de la recherche
    results() {
      // Si on ne tape rien, renvoyer tous les résultats
      if (this.searchQuery == "") {
        return this.dataSearch;
      } else {
        // Sinon on renvoie les résultats de la recherche
        let arr = search(this.searchQuery, this.dataSearch, {
          keySelector: obj => obj.prenom
        });

        // console.log(arr);
        return arr;
      }
    }
  },
  async mounted() {
    // see: https://docs.google.com/spreadsheets/d/10WDbAPAY7Xl5DT36VuMheTPTTpqx9x0C5sDCnh4BGps
    const sheetId = "1v_Mc_FRtV7_yrBcEH6_2S1EVyqtLI5DhLAbudJ419f4";

    const data1 = await this.parseSheet(sheetId);
    console.log(data1); // [{"a":1,"b":2,"c":3},{"a":4,"b":5,"c":6},{"a":7,"b":8,"c":9}]

    // const data2 = await this.parseSheet(sheetId, "Sheet2");
    // console.log(data2); // [{"a":10,"b":20,"c":30},{"a":40,"b":50,"c":60},{"a":70,"b":80,"c":90}]

    this.dataSearch = data1;
  }
};
</script>
