<template>
  <div class="container-fluid py-5" style="background-color: #f5f5f5">
    <div style="background-color: #ffffff" class="p-5 m-auto signup-form">
      <div class="text-center">
        <img src="../../assets/images/Logo.png" alt="Logo" />
        <h2 class="mt-4">Create your account</h2>
        <p>Enter your details to use all the app features.</p>
      </div>
      <form class="mt-3" @submit.prevent="register">
        <div class="row">
          <div class="col-md-6">
            <!-- Firstname -->
            <base-input
              type="text"
              identity="firstname"
              placeholder="Ex: Jack"
              label="Firstname"
              v-model="signupData.firstname"
            ></base-input>
          </div>
          <div class="col-md-6">
            <!-- Lastname -->
            <base-input
              type="text"
              identity="lastname"
              placeholder="Ex: Daniel"
              label="Lastname"
              v-model="signupData.lastname"
            >
            </base-input>
          </div>
        </div>
        <div class="my-4">
          <!-- Username -->
          <base-input
            type="text"
            identity="username"
            placeholder="Your Username"
            label="Username"
            v-model="signupData.username"
          ></base-input>
        </div>
        <div class="my-4">
          <!-- Email -->
          <base-input
            type="email"
            identity="email"
            placeholder="Your email address"
            label="Email"
            v-model="signupData.email"
          ></base-input>
        </div>
        <div class="my-4">
          <!-- Password -->
          <base-input
            type="password"
            identity="password"
            placeholder="Your Password"
            label="Password"
            v-model="signupData.password"
            @keyInput="passwordCheck"
          ></base-input>
          <p
            class="text-danger mt-1 fw-medium"
            style="font-size: 11px"
            :style="{ display: passwordStatusDisplay }"
          >
            The password field must be at least 8 characters
          </p>
        </div>
        <div class="my-4">
          <!-- Confirm Password -->
          <base-input
            type="password"
            identity="confirmedPassword"
            placeholder="Same with password"
            label="Confirmation Password"
            v-model="signupData.confirmationPassword"
            @keyInput="confirmationPasswordCheck"
          ></base-input>
          <p
            class="text-danger mt-1 fw-medium"
            style="font-size: 11px"
            :style="{ display: confirmationPasswordDoesNotMatch }"
          >
            The password confimation does not match
          </p>
          <p
            class="text-success mt-1 fw-medium"
            style="font-size: 11px"
            :style="{ display: confirmationPasswordMatch }"
          >
            The password confirmation does match
          </p>
        </div>
        <div class="my-4">
          <base-input
            type="file"
            identity="recipeImage"
            label="Profile Photo"
            isImage="true"
            @input="checkImage"
          >
            <div>
              <div class="border p-1 mt-2 rounded-circle">
                <img
                  :src="signupData.imageLink"
                  class="rounded-circle"
                  width="140"
                  height="150"
                  style="object-fit: cover"
                />
              </div>
              <div class="text-center" style="transform: translateY(-24px)">
                <i
                  class="fa-solid fa-camera fs-5 p-2 rounded-circle bg-white"
                ></i>
              </div>
            </div>
          </base-input>
        </div>
        <base-button class="login w-100 my-3"> Register </base-button>
      </form>
      <div class="text-center mt-4">
        <p class="fw-semibold">
          Already have account?<span style="color: #4c4ddc"
            ><router-link to="/login" class="text-decoration-none">
              Login</router-link
            ></span
          >
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import BaseInput from "../ui/BaseInput.vue";
import BaseButton from "../ui/BaseButton.vue";

import { reactive, ref } from "vue";

const passwordStatusDisplay = ref("none");

const signupData = reactive({
  firstname: "",
  lastname: "",
  username: "",
  email: "",
  password: "",
  confirmationPassword: "",
  isLogin: false,
  imageLink: "",
});

console.log({ signupData });

const passwordCheck = () => {
  if (signupData.password.length < 8) {
    passwordStatusDisplay.value = "block";
  } else {
    passwordStatusDisplay.value = "none";
  }
};

let confirmationPasswordDoesNotMatch = ref("none");
const confirmationPasswordMatch = ref("none");

const confirmationPasswordCheck = () => {
  if (signupData.confirmationPassword === "") {
    confirmationPasswordDoesNotMatch.value = "block";
    confirmationPasswordMatch.value = "none";
    return;
  }

  if (signupData.password !== signupData.confirmationPassword) {
    confirmationPasswordDoesNotMatch.value = "block";
    confirmationPasswordMatch.value = "none";
    return;
  }
  // else { // MEKANISME SAMA DENGAN CARA BEDA :
  //   confirmationPasswordDoesNotMatch.value = "none";
  //   confirmationPasswordMatch.value = "block";
  // }
  confirmationPasswordDoesNotMatch.value = "none";
  confirmationPasswordMatch.value = "block";
};

const checkImage = (e) => {
  const file = e.target.files[0];
  const reader = new FileReader();
  reader.readAsDataURL(file);

  reader.addEventListener("load", () => {
    signupData.imageLink = reader.result;
  });
};

import { useStore } from "vuex";
import { useRouter } from "vue-router";

const store = useStore();
const router = useRouter();

const register = async () => {
  if (
    signupData.password !== signupData.confirmationPassword ||
    signupData.password.length < 8
  ) {
    signupData.confirmationPassword = "";
    signupData.password = "";
    confirmationPasswordDoesNotMatch = "none";
    confirmationPasswordMatch = "none";
  } else {
    const response = await store.dispatch("auth/getRegisterData", signupData);

    console.log({ response });

    if (!response) {
      return alert("Oops, gagal melakukan registrasi");
    }

    alert("horeee berhasil")
    router.push("/");
  }
};
</script>
