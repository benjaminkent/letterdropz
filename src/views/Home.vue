<template>
  <div class="home">
    <h1>LetterDropz</h1>
    <h2>Hi Mom!</h2>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'Home',
})
</script>

<style lang="scss" scoped>
.home {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}
</style>
