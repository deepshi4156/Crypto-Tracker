<template>
  <v-app>
    <v-navigation-drawer
      persistent
      :mini-variant="miniVariant"
      :clipped="clipped"
      v-model="drawer"
      enable-resize-watcher
      fixed
      app
    >
      <v-list>
        <v-list-tile value="true" v-for="(item, i) in items" :key="i">
          <v-list-tile-action>
            <v-icon v-html="item.icon"></v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title v-text="item.title"></v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar app :clipped-left="clipped">
      <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>
      <v-btn icon @click.stop="miniVariant = !miniVariant">
        <v-icon v-html="miniVariant ? 'chevron_right' : 'chevron_left'"></v-icon>
      </v-btn>
      <v-btn icon @click.stop="clipped = !clipped">
        <v-icon>web</v-icon>
      </v-btn>
      <v-btn icon @click.stop="fixed = !fixed">
        <v-icon>remove</v-icon>
      </v-btn>
      <v-toolbar-title v-text="title"></v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn icon @click.stop="rightDrawer = !rightDrawer">
        <v-icon>menu</v-icon>
      </v-btn>
    </v-toolbar>
    <v-content>
      <Tracker v-bind:cryptocurrencies="cryptocurrencies" />
    </v-content>
    <v-navigation-drawer temporary :right="right" v-model="rightDrawer" fixed app>
    </v-navigation-drawer>
  </v-app>
</template>

<script>
import axios from "axios";
import Tracker from "./components/Tracker";

export default {
  data() {
    return {
      clipped: false,
      drawer: true,
      fixed: false,
      items: [
        {
          icon: "bubble_chart",
          title: "Cryptocurrency Tracker",
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: "Cryptocurrency Tracker",
      cryptocurrencies: [],
      errors: [],
    };
  },
  name: "App",
  components: {
    Tracker,
  },
  created() {
    axios
      .get(
        "https://min-api.cryptocompare.com/data/pricemulti?fsyms=BTC,SHIB,SOL,FIL,ATOM,USDT,ETH,IOT,LTC,XMR&tsyms=USD,EUR"
      )
      .then((response) => {
        this.cryptocurrencies = response.data;
      })
      .catch((error) => this.errors.push(error));
  },
};
</script>
<style>
#hearts {
  color: #ff3b30;
}
</style>
