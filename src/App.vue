<template>
  <v-app>
    <v-main>
      <div>App</div>
      <router-view :key="$route.fullPath" />
    </v-main>
  </v-app>
</template>

<script>

export default {
};
</script>
