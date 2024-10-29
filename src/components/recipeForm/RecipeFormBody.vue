<template>
  <!-- {{ isLoadingUpdate && "Loading ..." }} -->

  <li class="list-group-item">
    <form @submit.prevent="addNewRecipe">
      <!-- General Information Start -->
      <div>
        <p class="my-3 fs-5 fw-semibold">General Information</p>
        <div>
          <div class="mb-3">
            <base-input
              type="file"
              identity="recipeImage"
              label="Photo Recipe"
              @input="checkImage"
            ></base-input>
            <div>
              <img
                :src="recipeData.imageLink"
                :alt="recipeData.name"
                class="image"
              />
            </div>
          </div>
          <div class="mb-3">
            <base-input
              type="text"
              identity="recipeTitle"
              placeholder="Give your recipe a title"
              label="Recipe Title"
              v-model="recipeData.name"
            ></base-input>
          </div>
          <div class="mb-3">
            <base-text-area
              identity="recipeDescription"
              label="Description"
              placeholder="Share story behind recipe and what make it spesial"
              v-model="recipeData.description"
            ></base-text-area>
          </div>
          <div class="mb-3 mt-3">
            <base-select
              class="mt-3"
              :data="['Breakfast', 'Lunch', 'Dinner', 'Meals', 'Snacks']"
              v-model="recipeData.category"
            ></base-select>
          </div>
        </div>
      </div>
      <!-- General Information End -->

      <!-- Time Setting Start -->
      <div class="border-top py-1">
        <p class="my-3 fs-5 fw-semibold">Time Setting</p>
        <div>
          <div class="mb-3">
            <base-input
              type="number"
              identity="prepTime"
              placeholder="0"
              label="Prep Time"
              v-model="recipeData.prepTime"
            ></base-input>
          </div>
          <div class="mb-3">
            <base-input
              type="number"
              identity="cookTime"
              placeholder="0"
              label="Cook Time"
              v-model="recipeData.cookTime"
            ></base-input>
          </div>
          <div class="mb-3">
            <base-input
              type="number"
              identity="totalTime"
              placeholder="0"
              label="Total Time"
              v-model="recipeData.totalTime"
              @totalTimeFocus="totalTime"
              readonly="1"
            ></base-input>
          </div>
        </div>
      </div>
      <!-- Time Setting End -->

      <!-- Ingredients Start -->
      <div class="border-top py-1">
        <p class="my-3 fs-5 fw-semibold">Ingredients</p>
        <p>
          Enter one ingredient per line. Include the quantity (i.e. cups,
          tablespoons) and any special preparation (i.e. sifted, softened,
          chopped). Use optional headers to organize the different parts of the
          recipe (i.e. Cake, Frosting, Dressing).
        </p>
        <div>
          <div class="mb-3 row" v-for="count in ingredientCount" :key="count">
            <div class="col-lg-11 col-11">
              <base-input
                type="text"
                identity="Ingredient"
                placeholder="Ex: 1 cup sugar"
                v-model="recipeData.ingredients[count - 1]"
              ></base-input>
            </div>
            <div
              class="col-lg-1 col-1 col-form-label align-self-end delete-ingredient"
              style="color: #cb3a31"
              v-if="ingredientCount > 1"
              @click="deleteIngredient(count - 1)"
            >
              <i class="fa-regular fa-trash-can px-1"></i
              ><span class="d-none d-md-inline">Delete</span>
            </div>
          </div>
        </div>
        <base-button
          class="new-ingredient-btn px-3 py-2"
          type="button"
          @clickButton="addIngredient"
        >
          Add More<!-- 9 -->
        </base-button>
      </div>
      <!-- Ingredients End -->

      <!-- Direction Start -->
      <div class="border-top my-3">
        <p class="my-3 fs-5 fw-semibold">Directions</p>
        <p>
          Explain how to make your recipe, including oven temperatures, baking
          or cooking times, and pan sizes, etc. Use optional headers to organize
          the different parts of the recipe
        </p>
        <div>
          <div class="mb-3 row" v-for="count in directionCount" :key="count">
            <div class="col-lg-11 col-11">
              <base-input
                type="text"
                identity="Direction"
                placeholder="Ex: 1 cup sugar"
                v-model="recipeData.directions[count - 1]"
              ></base-input>
            </div>

            <div
              class="col-lg-1 col-1 col-form-label align-self-end delete-ingredient"
              style="color: #cb3a31"
              v-if="directionCount > 1"
              @click="deleteDirection(count - 1)"
            >
              <i class="fa-regular fa-trash-can px-1"></i
              ><span class="d-none d-md-inline">Delete</span>
            </div>
          </div>
        </div>
        <base-button
          class="new-ingredient-btn px-3 py-2"
          type="button"
          @clickButton="addDirection"
        >
          Add More<!-- 11 -->
        </base-button>
      </div>
      <!-- Direction End -->

      <!-- Form Button Start -->
      <div class="border-top py-3 d-flex my-4 justify-content-end">
        <base-button class="cancel-btn px-3 py-2 ms-1">
          Cancel<!-- 12 -->
        </base-button>

        <base-button class="submit-recipe-btn px-3 py-2 ms-1">
          {{ isEdit ? "Update Data" : "Tambah Data" }}
        </base-button>
      </div>
      <!-- Form Button End -->
    </form>
  </li>
</template>

<script setup>
const props = defineProps({
  isEdit: { type: Boolean, default: false },
});

import BaseInput from "../ui/BaseInput.vue";
import BaseButton from "../ui/BaseButton.vue";
import BaseSelect from "../ui/BaseSelect.vue";
import BaseTextArea from "../ui/BaseTextArea.vue";
import { onMounted, reactive, ref, watchEffect } from "vue";
// import recipe from '@/recipe';
import { useStore } from "vuex";
import { useRouter } from "vue-router";
import { useRoute } from "vue-router";

const store = useStore();
const router = useRouter();
const route = useRoute();

// const isLoadingUpdate = ref(false);

// onMounted(() => {
//   if (props.isEdit) {
//     recipeData.value = store.state.recipe.recipeDetail;
//     ingredientCount.value = recipeData.value.ingredients.length;
//     directionCount.value = recipeData.value.directions.length;
//   }
// });

// onMounted(async () => {
//   if (props.isEdit) {
//     await store.dispatch(
//       "recipe/getRecipeDetail", route.params.id
//     )
//   }
// })

const recipeData = reactive({
  imageLink: "",
  name: "",
  description: "",
  category: "Breakfast",
  prepTime: 0,
  cookTime: 0,
  totalTime: 0,
  ingredients: [],
  directions: [],
});

watchEffect(() => {
  if (props.isEdit) {
    // true jika di halaman edit
    const valueEditRecipe = store.state.recipe.recipeDetail; // null

    // Periksa apakah valueEditRecipe sudah ada dan bukan null atau undefined
    if (valueEditRecipe) {
      recipeData.imageLink = valueEditRecipe.imageLink || "";
      recipeData.name = valueEditRecipe.name || "";
      recipeData.description = valueEditRecipe.description || "";
      recipeData.category = valueEditRecipe.category || "";
      recipeData.prepTime = valueEditRecipe.prepTime || 0;
      recipeData.cookTime = valueEditRecipe.cookTime || 0;
      recipeData.totalTime = valueEditRecipe.totalTime || 0;
      recipeData.ingredients = valueEditRecipe.ingredients || [];
      recipeData.directions = valueEditRecipe.directions || [];
    }
  }
});

const ingredientCount = ref(1);
const directionCount = ref(1);

console.log({ recipeData });

const addIngredient = () => {
  ingredientCount.value++;
};

const addDirection = () => {
  directionCount.value++;
};

const deleteIngredient = (index) => {
  recipeData.ingredients.splice(index, 1);
  ingredientCount.value--;
};

const deleteDirection = (index) => {
  recipeData.directions.splice(index, 1);
  directionCount.value--;
};

const totalTime = () => {
  recipeData.totalTime =
    parseInt(recipeData.prepTime) + parseInt(recipeData.cookTime);
};

const checkImage = (e) => {
  const file = e.target.files[0];
  const reader = new FileReader();
  reader.readAsDataURL(file);

  reader.addEventListener("load", () => {
    recipeData.imageLink = reader.result;
  });
};

console.log({ isProps: props.isEdit });

const addNewRecipe = async () => {
  try {
    // isLoadingUpdate.value = true;
    if (props.isEdit) {
      // Panggil action updateRecipe jika isEdit bernilai true
      console.log("HELO");
      await store.dispatch("recipe/updateRecipe", {
        id: route.params.id,
        newRecipe: recipeData,
      });

      // isLoadingUpdate.value = false;

      alert("Data Berhasil diupdate");
      router.push("/user/user-recipe");

      return;
      // console.log({ recipeData })
    }

    // Panggil action addNewRecipe jika isEdit bernilai false
    await store.dispatch("recipe/addNewRecipe", {
      ...recipeData,
    });

    // isLoadingUpdate.value;

    alert("Data Berhasil diTAMBAH");
    router.push("/user/user-recipe");
  } catch (error) {
    console.error({ error });
    // isLoadingUpdate.value = false;
  }
};
</script>
