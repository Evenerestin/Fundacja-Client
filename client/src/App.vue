<script setup>
import { RouterView } from 'vue-router'
import NavbarView from './Layout/NavbarView.vue'
</script>

<template>
  <div>
    <NavbarView />
    <RouterView />
  </div>
</template>

<style src="./App.scss"></style>
