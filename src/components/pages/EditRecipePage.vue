<template>
  <main>
    <div class="container-md my-5 py-5">
      <recipe-form
        :detailData="detailData"
        v-if="detailData && !isLoading"
        :isEdit="true"
      >
      </recipe-form>
    </div>
  </main>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useStore } from "vuex";
import { useRoute } from "vue-router";
import RecipeForm from "../recipeForm/RecipeForm.vue";

const store = useStore();
const route = useRoute();
const detailData = ref({});
let isLoading = false;

onMounted(async () => {
  try {
    isLoading = true;
    const recipeId = route.params.id;
    const response = await store.dispatch("recipe/getRecipeDetail", recipeId);

    detailData.value = response;
    isLoading = false;
  } catch (error) {
    console.error("Failed to load recipe details:", error);
  }
});
</script>
