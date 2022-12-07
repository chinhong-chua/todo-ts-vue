<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import { defineComponent } from "vue";

defineComponent({
  name: "App",
});
</script>

<template>
  <div class="me-auto">
    <nav class="navbar navbar-expand navbar-dark bg-dark  p-3">
      <div class="ms-3">
        <router-link to="/" class="navbar-brand ms-3">Home</router-link>

      </div>
      <div class="navbar-nav me-auto">
        <li class="nav-item">
          <router-link to="/tutorials" class="nav-link">Tutorials</router-link>
        </li>
        <li class="nav-item">
          <router-link to="/add" class="nav-link">Add</router-link>
        </li>
      </div>
    </nav>
    <div class="container mt-3">
      <router-view />
    </div>
  </div>
</template>

<style scoped></style>
