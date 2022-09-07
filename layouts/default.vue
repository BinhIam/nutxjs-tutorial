<template>
  <v-card>
    <v-app-bar
      dense
      dark
    >
      <v-app-bar-nav-icon></v-app-bar-nav-icon>
      <v-toolbar-title>STUDENT MANAGEMENT</v-toolbar-title>
      <v-spacer></v-spacer>
      
      <NuxtLink to="/login">
        <v-btn v-if="!$auth.loggedIn" class="ma-2">
          Login
          <v-icon>mdi-login</v-icon>
        </v-btn>
      </NuxtLink>
      <NuxtLink to="/register">
        <v-btn v-if="!$auth.loggedIn">
          Register
          <v-icon>mdi-account-plus</v-icon>
        </v-btn>
      </NuxtLink>
      <v-btn v-if="$auth.loggedIn" @click="logout">
        Logout
        <v-icon>mdi-logout</v-icon>
      </v-btn>
    </v-app-bar>
    <Nuxt />
  </v-card>
</template>
<script>
export default {
  methods: {
    async logout() {
      this.$nuxt.$loading.start();
      this.$auth.logout();
      this.$nuxt.$loading.finish();
      this.$router.push("/login");
    },
  },
};
</script>