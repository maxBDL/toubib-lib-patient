<script setup>
import {RouterView, RouterLink} from 'vue-router'
</script>


<template>
  <div>
    <nav>
      <RouterLink to="/"> Home </RouterLink> |
      <RouterLink to="/register"> Register </RouterLink> |
      <RouterLink to="/login"> Login </RouterLink> |
    </nav>
    <RouterView />
  </div>
</template>


<style scoped>

</style>
