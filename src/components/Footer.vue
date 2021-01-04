<template>
  <v-footer
    dark
    fixed:false
  >
    <v-card
      flat
      tile
      class="card-color card-width"
    >
      <v-card-text>
        <v-btn
          v-for="icon in icons"
          :key="icon"
          icon
          class="icon-color"
        >
          <v-icon size="24px">
            {{ icon }}
          </v-icon>
        </v-btn>
      </v-card-text>

      <v-card-text >
        <h2>Sign Up For Newsletters</h2>
        <input type="email" placeholder="Your email address" required />
        <button>SUBSCRIBE</button>
      </v-card-text>

      <v-divider></v-divider>

      <v-card-text >
        <strong>Swapna Sahu </strong>© {{ new Date().getFullYear() }}. All rights reserved.</strong>
      </v-card-text>
    </v-card>
  </v-footer>
</template>
<script>
export default {
  name: "Footer",
  data: () => ({
    icons: ["mdi-facebook", "mdi-twitter", "mdi-linkedin", "mdi-instagram"],
  }),
};
</script>
<style>
.card-color {
  background-color: rgb(10, 10, 82) !important;
}
.icon-color {
  background-color: transparent !important;
}
.card-width {
  width: 100%;
}
</style>