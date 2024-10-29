<template>
  <header class="shadow-sm container-fluid bg-white py-3 border-bottom fixed-top">
    <div class="container-md d-flex justify-content-between align-items-center">
      <router-link to="/" class="logo">
        <img src="@/assets/images/Logo.png" alt="Logo">
      </router-link>
      <navigation-bar></navigation-bar>
    </div>
  </header>
</template>

<script>
  import NavigationBar from "../header/NavigationBar.vue"
  export default {
    components: {
      NavigationBar
    }
  }
</script>