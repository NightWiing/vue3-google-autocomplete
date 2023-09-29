<template>
  <GoogleAutocomplete
    v-model="value"
    api-key="process.env.VITE_APP_GAPI_KEY"
  />
</template>

<script setup lang="ts">
import { ref } from 'vue'
import GoogleAutocomplete from './components/GoogleAutocomplete.vue'

const value = ref()
</script>
