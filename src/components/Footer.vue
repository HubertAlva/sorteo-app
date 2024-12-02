<script setup lang="ts">
import { GithubLogoIcon } from '@radix-icons/vue'
</script>

<template>
  <footer>
    <div class="flex justify-between items-center gap-4 flex-wrap">
      <small
        >Desarrollado por
        <a href="https://hubertalva.com" target="_blank" class="hover:underline"> Hubert Alva </a>
      </small>
      <a href="https://github.com/HubertAlva" target="_blank">
        <GithubLogoIcon
          class="size-4 text-muted-foreground hover:text-gray-950 dark:hover:text-white"
        />
      </a>
    </div>
  </footer>
</template>
