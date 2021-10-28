<template>
  <md-app
    v-cloak
    md-waterfall
    md-mode="fixed"
  >
    <md-app-toolbar class="md-primary md-dense">
      <div class="md-toolbar-section-start">
        <router-link to="/" class="md-title">Azure Cloud Integration - Game</router-link>
      </div>
    </md-app-toolbar>

    <md-app-content>
      <game/>
    </md-app-content>
  </md-app>
</template>

<script>
import Game from "@/components/Game";
export default {

    name: 'App',

    components: {
      Game
    },

    methods: {
        answer() {
            return 42
        },
    },
}
</script>

<style scoped>
</style>
