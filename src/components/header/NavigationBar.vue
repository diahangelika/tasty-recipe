<template>
  <div
    class="header__navbar row justify-content-between align-items-center"
    style="width: 450px"
  >
    <search-menu></search-menu>
    <!-- <signup-menu></signup-menu> -->
    <component :is="components[menuComponent]"></component>
  </div>
</template>

<script setup>
import SearchMenu from "../header/SearchMenu.vue";
import SignupMenu from "../header/SignupMenu.vue";

import ProfileMenu from "./ProfileMenu.vue";
import { computed, ref, watch } from "vue";
import { useStore } from "vuex";

const menuComponent = ref("signup-menu");
const store = useStore();

// console.log(store.state);

const components = {
  "signup-menu": SignupMenu,
  "profile-menu": ProfileMenu,
};

const getToken = computed(() => {
  return store.state.auth.token;
});

console.log({ token: getToken.value });

if (!getToken.value) {
  menuComponent.value = "signup-menu";
} else {
  menuComponent.value = "profile-menu";
}

console.log(getToken.value);

watch(getToken, (newValue, oldValue) => {
  if (!newValue) {
    menuComponent.value = "signup-menu";
  } else {
    menuComponent.value = "profile-menu";
  }
});
</script>
