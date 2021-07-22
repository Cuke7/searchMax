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
          {{ result.nom }}
        </v-row>
      </div>
    </v-col>
  </v-row>
</template>

<script>
// Library pour le search
const { fuzzy, Searcher } = require("fast-fuzzy");

// data_magical_items contient un tableau JS avec toutes nos données
import data_magical_items from "~/static/magical_objects.json";

export default {
  data() {
    return {
      searchQuery: ""
    };
  },
  computed: {
    // Un objet pour rechercher, on le configure avec des options
    searcher() {
      let options = {
        keySelector: obj => obj.nom, // trier par nom
        threshold: 0.8 // sensibilité de la recherche (défaut = 0.6)
      };
      return new Searcher(data_magical_items, options);
    },
    // Un tableau avec les résultats de la recherche
    results() {
      // Si on ne tape rien, renvoyer tous les résultats
      if (this.searchQuery == "") {
        return data_magical_items;
      } else {
        // Sinon on renvoie les résultats de la recherche
        let arr = this.searcher.search(this.searchQuery);
        console.log(arr);
        return arr;
      }
    }
  }
};
</script>
