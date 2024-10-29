<template>
  <ul class="list-group">
    <li class="list-group-item">
      <div
        class="d-flex flex-sm-row flex-column justify-content-between align-items-sm-center"
      >
        <div class="mb-3 mb-sm-0">
          <p class="my-0 fs-4 fw-semibold">My Recipe</p>
          <p class="my-0 text-secondary">Add your original recipe here</p>
        </div>
        <div>
          <button
            class="btn add-btn px-3 py-2 rounded-pill"
            @click="goToAddRecipe"
          >
            <i class="fa-solid fa-circle-plus pe-2"></i>Add Recipe
          </button>
        </div>
      </div>
    </li>
    <li class="list-group-item">
      <p class="mt-2 mb-4 fs-5 fw-semibold">Recipe</p>
      <div class="row">
        <!-- <user-recipe-card 
            v-for="recipe in recipes"
            :key="recipe.id"
            :recipe="recipe"
            :buttonName="['Delete', 'Edit']">
            <p>{{ new Date(recipe.createdAt).toDateString() }}</p>
         </user-recipe-card>  -->
        <user-recipe-card
          v-for="recipe in recipeList"
          :key="recipe.id"
          :recipe="recipe"
          :buttonName="['Delete', 'Edit']"
          @btnRemove="deleteRecipe(recipe.id)"
          @btnEdit="updateRecipe(recipe.id)"
        >
          <p>{{ moment(recipe.createdAt).format("YYYY-MM-DD") }}</p>
        </user-recipe-card>
      </div>
    </li>
  </ul>
</template>

<!-- before chatgpt -->
<script setup>
import { computed, onMounted, ref } from "vue";
import { useStore } from "vuex";

const store = useStore();
const recipeList = ref([]);

// const recipe = computed(() => {
//   const allRecipes = store.state.recipe.recipes;
//   const userId = store.state.auth.userLogin.userId;
//   return allRecipes.filter((recipe) => recipe.userId === userId);
// });
const recipes = computed(() => {
  const allRecipes = store.state.recipe?.recipes;
  const userId = store.state.auth.userLogin?.userId;
  return allRecipes.filter((recipe) => recipe.userId === userId);
});
// console.log(recipes.value)

import UserRecipeCard from "./UserRecipeCard.vue";

import { useRouter } from "vue-router";
import moment from "moment";
const router = useRouter();

const goToAddRecipe = () => {
  router.push("/new-recipe");
};

const deleteRecipe = async (id) => {
  await store.dispatch("recipe/deleteRecipe", id);
};

const updateRecipe = (id) => {
  const uid = isNaN(Number(id));

  router.push({ name: "editRecipePage", params: { id: uid ? `${id}` : id } });
};

onMounted(async () => {
  try {
    await store.dispatch("recipe/getRecipeData");
    recipeList.value = store.state.recipe.recipes;
  } catch (error) {
    console.log(error);
  }
});
</script>

<!-- <script setup>
import { ref, onMounted, watch } from 'vue';
import UserRecipeCard from './UserRecipeCard.vue';
import { useRouter } from 'vue-router';

const router = useRouter();
const recipes = ref([]);

// Load recipes from localStorage on mount
onMounted(() => {
  const savedRecipes = JSON.parse(localStorage.getItem('userRecipes')) || [];
  recipes.value = savedRecipes;
});

// Watch for changes and save to localStorage
watch(recipes, (newRecipes) => {
  localStorage.setItem('userRecipes', JSON.stringify(newRecipes));
}, { deep: true });

// Function to delete a recipe by ID
const deleteRecipe = (id) => {
  recipes.value = recipes.value.filter(recipe => recipe.id !== id);
};

// Redirect to add recipe page
const goToAddRecipe = () => {
  router.push("/new-recipe");
};
</script> -->
