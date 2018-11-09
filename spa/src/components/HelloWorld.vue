<template>
  <v-layout row>
    <v-flex >
      <v-card>
        <v-toolbar color="blue" dark>
          <v-toolbar-side-icon></v-toolbar-side-icon>

          <v-toolbar-title>Carros</v-toolbar-title>

          <v-spacer></v-spacer>

          <v-btn icon>
            <v-icon>search</v-icon>
          </v-btn>

        </v-toolbar>

        <v-list two-line>
          <template v-for="(item, index) in items">
            <v-list-tile
              :key="item.title"
              avatar
              ripple
              @click="toggle(index)"
            >
              <v-list-tile-content>
                <v-list-tile-title>{{ item.nome }}</v-list-tile-title>
                <v-list-tile-sub-title class="text--primary">Preço: R$ {{ item.preco }}</v-list-tile-sub-title>
                <v-list-tile-sub-title>Ano: {{ item.ano }}</v-list-tile-sub-title>
              </v-list-tile-content>

              <v-list-tile-action>
                <v-icon
                  v-if="selected.indexOf(index) < 0"
                  color="grey lighten-1"
                >
                  star_border
                </v-icon>

                <v-icon
                  v-else
                  color="yellow darken-2"
                >
                  star
                </v-icon>
              </v-list-tile-action>

            </v-list-tile>
            <v-divider
              v-if="index + 1 < items.length"
              :key="index"
            ></v-divider>
          </template>
        </v-list>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
  export default {
    data: () => ({
      selected: [2],
        items: []
    }),
    created() {
      this.$http.get('http://localhost:3000/api/Carros').then(response => {

        // get body data
        this.items = response.body;
      }, response => {
        // error callback
      });
    }
  }
</script>

<style>

</style>
