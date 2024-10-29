<template>
  <ul class="list-group">
    <recipe-form-header></recipe-form-header>
    <recipe-form-body :isEdit="isEdit" :detailData="detailData"></recipe-form-body>

    <!-- <recipe-form-body :isEdit="isEdit"></recipe-form-body> -->
  </ul>
  <p v-if="isEdit">Editing Mode</p>
</template>

<script setup>
import RecipeFormHeader from './RecipeFormHeader.vue';
import RecipeFormBody from './RecipeFormBody.vue';

import { defineProps } from 'vue';

const props = defineProps({
  isEdit: {
    type: Boolean,
    default: false,
  },
});
</script>
