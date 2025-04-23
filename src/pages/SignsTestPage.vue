<template>
  <div class="page-signs-test">
    <div class="container">
      <component
        :is="component"
        :categories="categoriesList.data"
        :data="data"
        :nextPage="nextPage"
      />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import SignsTestList from '../components/SignsTestList.vue'
import CategoryList from '../components/CategoryList.vue'
import warningSigns from '../db/roadSigns/warningSigns'
import privilegeSigns from '../db/roadSigns/privilegeSigns'
import categoriesList from '../db/categories/cateegoriesList'
import prohibitionSigns from '../db/roadSigns/prohibitionSigns'
const data = ref({
  category_id: null,
  data: null,
  countSigns: null,
  countTimer: null,
})
function getRandomShuffledItems(array, count = 10) {
  const shuffled = [...array]
  for (let i = shuffled.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1))
    ;[shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]]
  }
  return shuffled.slice(0, count)
}

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
      } else if (page.category_id === 3) {
        data.value = {
          category_id: page.category_id,
          data: prohibitionSigns.data,
          countSigns: page.countSigns,
          countTimer: page.countTimer,
        }
      } else if (page.category_id === 8) {
        data.value = {
          category_id: page.category_id,
          data: getRandomShuffledItems(
            [...privilegeSigns.data, ...warningSigns.data, ...prohibitionSigns.data],
            page.countSigns,
          ),
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
<style scoped>
.page-signs-test {
  overflow: hidden;
}
</style>
<style lang="scss" scoped>
.page-signs-test {
  background-color: #000;
  width: 100%;
  padding: 20px;
  background-image: url('/bg/bg-auto.jpg');
  background-size: 100%;
  background-position: center;
  background-repeat: no-repeat;
}
.container {
  width: 100%;
  max-width: 1000px;
  margin: 30px auto;
  padding: 0 20px;
  height: 100%;
}
@media (max-width: 1000px) {
  .page-signs-test {
    background-image: url('/bg/bg-auto-rotate.jpg');
    background-size: cover;
    overflow: auto;
  }
}
</style>
