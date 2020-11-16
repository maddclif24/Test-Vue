<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />
        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
      </div>
      <v-spacer></v-spacer>
    </v-app-bar>

    <v-main>
      <v-card
         flat
         class="py-12"
      >
        <v-card-text>
          <v-row
             align="center"
             justify="center"
          >
            <v-btn-toggle
               v-model="toggle_exclusive"
               multiple
            >
              <v-btn>
                <v-icon>C</v-icon>
              </v-btn>
              <v-btn>
                <v-icon>1</v-icon>
              </v-btn>
              <v-btn>
                <v-icon>2</v-icon>
              </v-btn>
              <v-btn>
                <v-icon>3</v-icon>
              </v-btn>
              <v-btn>
                <v-icon>4</v-icon>
              </v-btn>
            </v-btn-toggle>
          </v-row>
        </v-card-text>
      </v-card>
      <v-data-table
            :headers="headers"
            :items="jsonData"
            :items-per-page="5"
            v-if="!toggle_exclusive.length"
            class="elevation-1"
      ></v-data-table>
      <v-data-table
              :headers="headers"
              :items="filtered"
              :items-per-page="5"
              v-else-if="filteredTable"
              v-bind:filtred="filteredTable"
              class="elevation-1"
      ></v-data-table>
    </v-main>
  </v-app>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  components: {

  },
  data() {
    return {
      jsonData: [],
      toggle_exclusive: [],
      filtered: [],
      headers: [
        {
          text: 'id',
          align: 'start',
          sortable: false,
          value: 'name',
        },
        { text: 'Korpus', value: 'korpus' },
        { text: 'Section', value: 'section' },
        { text: 'Floor', value: 'floor' },
        { text: 'Type', value: 'type' },
        { text: 'Number on floor', value: 'number_on_floor' },
        { text: 'Number', value: 'number' },
        { text: 'Quantity', value: 'quantity' },
        { text: 'Area', value: 'area' },
        { text: 'Amount', value: 'amount' },
        { text: 'Status', value: 'status' },
        { text: 'Plan type', value: 'plan_type' },
        { text: 'Balcony', value: 'balcony' },
        { text: 'Action', value: 'action' },
        { text: 'Layout type', value: 'layouttype' },
        { text: 'Amount usd', value: 'amount_usd' },
        { text: 'Amount eur', value: 'amount_eur' },
        { text: 'Price usd', value: 'price_usd' },
        { text: 'Price eur', value: 'price_eur' },
        { text: 'Floor image', value: 'floor_image' },
        { text: 'Plan image', value: 'plan_image' },
      ]
    };
  },
  mounted() {
      axios.get('https://api.gkosnova.tech/objects/api/v1/red/apartments/list').then(response => (this.jsonData = response.data));
  },
  computed: {
    filteredTable() {
      const container = [];
      if (this.toggle_exclusive.includes(0)) {
        container.push(this.jsonData.filter(item => (item['quantity'] === 'Студия')));
      }

      if (this.toggle_exclusive.includes(2)) {
        container.push(this.jsonData.filter(item => (item['quantity'] === '2' || item['quantity'] === 'Евро-2')));
      }

      if (this.toggle_exclusive.includes(3)) {
        container.push(this.jsonData.filter(item => (item['quantity'] === 'Евро-3')));
      }
      return this.filtered = container.flat(1)
    }
  }
};

</script>
