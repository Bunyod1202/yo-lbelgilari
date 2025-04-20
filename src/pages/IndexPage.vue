<template>
  <q-page class="flex flex-center">
    <div class="card" v-for="sign in warningSigns.data" :key="sign.id">
      <!-- {{ getImageUrl(sign) }} -->
      <!-- <img width="40" :src="sign.image" alt="" /> -->
      <p>{{ sign.name.uz }}</p>
    </div>
    <!-- {{ warningSigns.data[0] }}
    <img :src="warningSigns.data[0].image" alt="" /> -->
  </q-page>
</template>

<script setup>
//
import warningSigns from '../db/roadSigns/warningSigns'
console.log(warningSigns)
</script>
<style lang="scss" scoped>
.card {
  width: 100px;
  height: 100px;
  border: 1px solid #000;
}
</style>
