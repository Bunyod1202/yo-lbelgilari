<template>
  <div>
    <component
      :is="component"
      :categories="categoriesList.data"
      :data="data"
      :nextPage="nextPage"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import SignsTestList from '../components/SignsTestList.vue'
import CategoryList from '../components/CategoryList.vue'
import warningSigns from '../db/roadSigns/warningSigns'
import privilegeSigns from '../db/roadSigns/privilegeSigns'
import categoriesList from '../db/categories/cateegoriesList'
const data = ref({
  category_id: null,
  data: null,
  countSigns: null,
  countTimer: null,
})

const component = ref(CategoryList)
const nextPage = (page) => {
  switch (page.nextPage) {
    case 0:
      component.value = CategoryList
      console.log('nextPage', page.nextPage)
      break
    case 1:
      if (page.category_id === 1) {
        data.value = {
          category_id: page.category_id,
          data: warningSigns.data,
          countSigns: page.countSigns,
          countTimer: page.countTimer,
        }
      } else if (page.category_id === 2) {
        data.value = {
          category_id: page.category_id,
          data: privilegeSigns.data,
          countSigns: page.countSigns,
          countTimer: page.countTimer,
        }
      }
      component.value = SignsTestList
      console.log('nextPage', page.nextPage)
      break
  }
}
</script>

<style lang="scss" scoped></style>
