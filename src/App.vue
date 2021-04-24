<template>
  <the-header></the-header>
  <div class="container">
    <router-view />
  </div>
</template>

<script>
import TheHeader from "./components/TheHeader";
export default {
  components: {
    TheHeader,
  },
};
</script>
