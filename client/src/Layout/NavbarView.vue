<script setup>
import FoundationLogo from '@/components/FoundationLogo.vue'
import { IconContrastFilled, IconMoon, IconSun } from '@tabler/icons-vue'
import { inject, onBeforeUnmount, onMounted, ref, watch } from 'vue'
import { useRoute } from 'vue-router'

const theme = inject('theme', ref('light'))
const setTheme = inject('setTheme', () => {})

const accentColor = ref('')
const route = useRoute()

const menuData = ref([
  { title: 'O Anetce', url: '/o-anetce' },
  { title: 'Leczenie', url: '/leczenie' },
  { title: 'Podziękowania', url: '/podziekowania' },
  { title: 'Galeria', url: '/galeria' },
  { title: 'Fundacja Dzieciom', url: '/fundacja' },
  { title: 'Pomoc', url: '/pomoc' }
])

const setAccentColor = (path) => {
  if (path === '/o-anetce') {
    accentColor.value = 'about'
  } else if (path === '/leczenie') {
    accentColor.value = 'treatment'
  } else if (path === '/galeria') {
    accentColor.value = 'gallery'
  } else if (path === '/podziekowania') {
    accentColor.value = 'gratitude'
  } else if (path === '/fundacja') {
    accentColor.value = 'foundation'
  } else if (path === '/pomoc') {
    accentColor.value = 'help'
  } else {
    accentColor.value = ''
  }
}

watch(
  () => route.path,
  (newPath) => {
    setAccentColor(newPath)
    openMenu.value = false
  }
)
setAccentColor(route.path)
const getAccentColorClass = () => {
  return accentColor.value
}

const openMenu = ref(false)
const toggleMenu = () => {
  openMenu.value = !openMenu.value
}

const windowWidth = ref(window.innerWidth)
const isMobile = ref(window.innerWidth <= 1023)

const handleResize = () => {
  windowWidth.value = window.innerWidth
  isMobile.value = window.innerWidth <= 1023
}

onMounted(() => {
  window.addEventListener('resize', handleResize)
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', handleResize)
})
</script>

<template>
  <header id="navbarView" role="banner" class="flexColumn">
    <div class="banner flex">
      <div
        class="info flex"
        role="region"
        aria-label="Organization registration information"
      >
        <div>
          <p>KRS: 0000037904</p>
          <p>11787 Bieryt Aneta</p>
        </div>
        <div v-if="!isMobile">
          <p>Alior Bank 42 2490 0005 0000 4600 7549 3994</p>
          <p>Fundacja Dzieciom „Zdążyć z Pomocą”</p>
        </div>
      </div>
      <div class="themeSwitch flex" style="gap: 0.5rem; padding: 0.5rem 1rem">
        <button
          :class="{ active: theme === 'light' }"
          aria-label="Jasny motyw"
          @click="setTheme('light')"
        >
          <IconSun />
        </button>
        <button
          :class="{ active: theme === 'dark' }"
          aria-label="Ciemny motyw"
          @click="setTheme('dark')"
        >
          <IconMoon />
        </button>
        <button
          :class="{ active: theme === 'contrast' }"
          aria-label="Wysoki kontrast"
          @click="setTheme('contrast')"
        >
          <IconContrastFilled />
        </button>
      </div>
    </div>
    <nav class="flex" :class="{ active: openMenu, inactive: !openMenu }">
      <router-link to="/">
        <button class="siteLogo flexColumn">Anetka Bieryt<span>Ty też możesz pomóc!</span></button>
      </router-link>
      <div class="navigation flex" aria-label="Główna nawigacja">
        <ul class="flex">
          <li v-for="(menu, index) in menuData" :key="index">
            <router-link :to="menu.url">
              <button
                class="menuItem"
                :class="{
                  active: $route.path === menu.url,
                  [getAccentColorClass()]: accentColor.value !== ''
                }"
                :aria-current="$route.path === menu.url ? 'page' : null"
              >
                {{ menu.title }}
              </button>
            </router-link>
          </li>
        </ul>
      </div>
      <div class="flex">
        <a href="https://dzieciom.pl" target="_blank" rel="noopener noreferrer" :class="`foundation ${isMobile ? 'mobile' : ''}`">
          <FoundationLogo />
        </a>
        <div
          v-if="isMobile"
          @click="toggleMenu"
          :class="{ active: openMenu, inactive: !openMenu }"
          class="toggleMobile gridCenter"
        >
          <svg id="hamburger" viewbox="0 0 60 40">
            <g stroke-width="4" stroke-linecap="round" stroke-linejoin="round">
              <path id="top-line" d="M10,15 L40,15 Z"></path>
              <path id="middle-line" d="M10,25 L40,25 Z"></path>
              <path id="bottom-line" d="M10,35 L40,35 Z"></path>
            </g>
          </svg>
        </div>
      </div>
    </nav>
  </header>
</template>

<style src="./NavbarView.scss"></style>
