<template>
  <div class="col-12 col-sm-8">
    <recipe-description :recipes="recipeDetail"></recipe-description>
    <recipe-ingredients></recipe-ingredients>
    <recipe-directions></recipe-directions>
  </div>
</template>

<script setup>
import RecipeDirections from "./RecipeDirections.vue";
import RecipeIngredients from "./RecipeIngredients.vue";
import { onMounted, computed } from "vue";
import { useRoute } from "vue-router";
import { useStore } from "vuex";
import RecipeDescription from "./RecipeDescription.vue";

const store = useStore();
const route = useRoute();

// const regex = /^\d+$/;

// const recipeDetail = computed(() => store.state.recipe.recipeDetail);
// const isParamsANumber = regex.test(route.params.id);

onMounted(async () => {
    await store.dispatch("recipe/getRecipeDetail", route.params.id
      
    );
});
</script>

<!-- if (isParamsANumber) {
  await store.dispatch("recipe/getRecipeDetail", Number(route.params.id));
} else {
  alert("Parameter harus angka");
  window.location.replace("/");
} -->
