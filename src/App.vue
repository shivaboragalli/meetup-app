<template>
  <v-app>
  
    <v-toolbar dark class="primary">
  
      <v-toolbar-side-icon @click="sideNav = !sideNav" class="hidden-sm-and-up"></v-toolbar-side-icon>
  
      <v-toolbar-title>
  
        <router-link to="/" tag="span" style="cursor:pointer">
  
          DevMeetup
  
        </router-link>
  
      </v-toolbar-title>
  
      <v-spacer></v-spacer>
  
      <v-toolbar-items class="hidden-xs-only">
  
        <v-btn flat v-for="item in menuItems" :key="item.title"  :to="item.link">
  
          <v-icon left>{{item.icon}}</v-icon>
  
          {{item.title}}
  
        </v-btn>
        <v-btn flat v-if="userIsAuthenticated" @click="onLogout">
  
          <v-icon left>exit_to_app</v-icon>
  
         logout
  
        </v-btn>
  
      </v-toolbar-items>
  
    </v-toolbar>
  
    <main>
  
      <router-view></router-view>
  
    </main>
  
    <v-navigation-drawer temporary="" v-model="sideNav">
  
      <v-list>
  
        <v-list-tile v-for="item in menuItems" :key="item.title"  :to="item.link">
  
          <v-list-tile-action>
  
            <v-icon>{{item.icon}}</v-icon>
  
          </v-list-tile-action>
  
          <v-list-tile-content>{{item.title}}</v-list-tile-content>
  
        </v-list-tile>
        <v-list-tile v-if="userIsAuthenticated" @click="onLogout">
  
          <v-list-tile-action>
  
            <v-icon>exit_to_app</v-icon>
  
          </v-list-tile-action>
  
          <v-list-tile-content>logout</v-list-tile-content>
  
        </v-list-tile>
        
  
      </v-list>
  
    </v-navigation-drawer>
  
  </v-app>
</template>

<script>
import Vue from "vue";

import Vuetify from "vuetify";

import colors from "vuetify/es5/util/colors";

Vue.use(Vuetify, {
  theme: {
    primary: colors.red.darken1,

    accent: colors.red.accent1,

    secondary: colors.grey.lighten2,

    info: colors.blue.lighten1,

    warning: colors.amber.darken2,

    error: colors.red.accent4,

    success: colors.green.lighten2
  }
});

export default {
  data() {
    return {
      sideNav: false,
    };
  },
  computed: {
    menuItems() {
      let menuItems =  [
        {
          icon: "face",
          title: "Sign up",
          link: "/signup"
        },
        {
          icon: "lock_open",
          title: "Sign in",
          link: "/signin"
        }
      ]
      if(this.userIsAuthenticated){
        menuItems = [
         {
          icon: "supervisor_account",
          title: "View Meetups",
          link: "/meetups"
        },
        {
          icon: "room",
          title: "Organize Meetup",
          link: "/meetup/new"
        },
        {
          icon: "person",
          title: "Profile",
          link: "/profile"
        }
        ]
      }
      return menuItems;
    },
    userIsAuthenticated(){
      return this.$store.getters.user !== null && this.$store.getters.user !== undefined;
    }
  },
  methods:{
    onLogout(){
      this.$store.dispatch('logout')
    }
  }
};
</script>

<style lang="stylus">
@import './stylus/main';
</style>
