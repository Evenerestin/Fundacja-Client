<script setup>
import { onMounted, provide, ref, watchEffect } from 'vue'
import { RouterView } from 'vue-router'
import NavbarView from './Layout/NavbarView.vue'

const theme = ref('light')
provide('theme', theme)
provide('setTheme', (val) => {
  theme.value = val
})

const updateThemeClass = () => {
  document.documentElement.setAttribute('data-theme', theme.value)
}
watchEffect(updateThemeClass)
onMounted(updateThemeClass)
</script>

<template>
  <div>
    <NavbarView />
    <RouterView />
  </div>
</template>

<style src="./App.scss"></style>
