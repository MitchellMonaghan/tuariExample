<template>
  <router-view />
</template>
<script lang="ts">
import { defineComponent, onMounted } from 'vue'
import { invoke } from '@tauri-apps/api'

export default defineComponent({
  name: 'App',

  setup () {
    onMounted(() => {
      void invoke('init_process')
    })
  }
})
</script>
