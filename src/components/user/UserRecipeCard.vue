<template>
  <div class="col-md-6 col-12 my-2">
    <div class="card">
      <div class="card-body">
        <div class="d-flex justify-content-between">
          <div>
            <p class="mb-0">{{ recipe.category }}</p>
            <div class="h-50">
              <h4 class="fs-5 mb-0">{{ recipe.name }}</h4>
              <!-- <p>{{ formatDate(recipe.createdAt) }}</p> -->
            </div>
            <slot></slot>
          </div>
          <div class="d-flex">
            <img
              :src="recipe.imageLink"
              :alt="recipe.name"
              width="90"
              height="80"
              class="rounded"
              style="object-fit: cover"
            />
          </div>
        </div>
        <div class="d-flex justify-content-between mt-3 pt-3 border-top">
          <button
            class="btn delete-btn px-3 py-2 rounded-pill"
            @click="$emit('btnRemove')"
          >
            {{ buttonName[0] }}
          </button>
          <button
            class="btn edit-btn px-3 py-2 rounded-pill"
            @click="$emit('btnEdit')"
          >
            {{ buttonName[1] }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
// import recipe from "@/recipe";

defineProps({
  recipe: { type: Object, required: true },
  buttonName: { type: Array, required: true },
});
</script>

<!-- <script setup>
import { defineProps, defineEmits } from 'vue';

// Props for recipe data and button names
defineProps({
  recipe: { type: Object, required: true },
  buttonName: { type: Array, required: true }
});

// Emit event for delete action
const emit = defineEmits();
const onDelete = () => emit('delete', recipe.id);

// Format the date
const formatDate = (dateString) => {
  return new Date(dateString).toDateString();
};
</script> -->
