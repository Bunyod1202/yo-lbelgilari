<template>
  <div>
    <div class="quasion-sign">
      <div class="ball">{{ 5 - incorrectSigns.length }}</div>
      <div class="sign-name">{{ randomTenName[0]?.name.uz }}</div>
      <div class="timer">{{ formattedTime }}</div>
    </div>
    <div class="card-list">
      <div class="card-item" v-for="item in randomTen" :key="item.id">
        <div
          class="card"
          @click="choseItem(item)"
          :class="{
            incorrect: itemStates[item.id + `-${item.category_id}`].incorrect,
            correct: itemStates[item.id + `-${item.category_id}`].correct,
          }"
        >
          <img
            class="status-correct"
            v-if="itemStates[item.id + `-${item.category_id}`].correct"
            src="/status/correct.png"
            alt=""
          />
          <img
            class="status-incorrect"
            v-if="itemStates[item.id + `-${item.category_id}`].incorrect"
            src="/status/incorrect.png"
            alt=""
          />
          <img class="status-default" :src="item.img" :alt="item.name.uz" />
          <!-- <p>{{ item.name.uz }}</p> -->
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, computed, ref } from 'vue'
import { useQuasar } from 'quasar'
let audio = null
const playSound = (url) => {
  audio = new Audio(url)
  audio.play()
}
const $q = useQuasar()
const props = defineProps({
  data: {
    type: Array,
    required: true,
  },
  nextPage: {
    type: Function,
    required: true,
  },
})

onMounted(() => {
  startTimer()
})
const seconds = ref(0)
let timer = null
const formattedTime = computed(() => {
  const mins = String(Math.floor(seconds.value / 60)).padStart(2, '0')
  const secs = String(seconds.value % 60).padStart(2, '0')
  return `${mins}:${secs}`
})

const startTimer = () => {
  if (!timer) {
    timer = setInterval(() => {
      seconds.value++

      // 1 daqiqa (60 soniya) o‘tganda modal ochiladi
      if (seconds.value === 60 * props.data.countTimer) {
        stopTimer()
        playSound('/sounds/second-hand-149907.mp3')
        $q.dialog({
          title: 'Vaqt tugadi',
          message: `${props.data.countTimer} daqiqa o‘tdi!`,
          ok: 'Yopish',
          persistent: true,
        }).onOk(() => {
          audio?.pause()
          audio = null

          props.nextPage({
            nextPage: 0,
            category_id: props.data.category_id,
            data: props.data,
          })
        })
      }
    }, 1000)
  }
}
const stopTimer = () => {
  clearInterval(timer)
  timer = null
}

// const resetTimer = () => {
//   stopTimer()
//   seconds.value = 0
// }
const itemStates = computed(() => {
  return randomTen.reduce((acc, item) => {
    acc[item.id + `-${item.category_id}`] = {
      correct: correctSigns.value.includes(item),
      incorrect: incorrectSigns.value.includes(item),
    }
    return acc
  }, {})
})
const randomSigns = ref([])
const correctSigns = ref([])
const incorrectSigns = ref([])
function getRandomShuffledItems(array, count = 10) {
  // Massivni nusxa olish va aralashtirish (Fisher-Yates Shuffle)
  const shuffled = [...array]
  for (let i = shuffled.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1))
    ;[shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]]
  }

  // Shundan keyin birinchi `count` ta elementni olish
  return shuffled.slice(0, count)
}
const randomTen = getRandomShuffledItems(props.data.data, props.data.countSigns)

if (randomSigns.value.length < props.data.countSigns) {
  randomSigns.value = [...randomTen]
}
const randomTenName = ref(getRandomShuffledItems(randomSigns.value, 1))
const choseItem = (item) => {
  // startTimer()
  console.log('Dsdsds', item.category_name === randomTenName.value[0].category_name)
  console.log('ssss', item.id === randomTenName.value[0].id)
  if (
    item.id === randomTenName.value[0]?.id &&
    item.category_name === randomTenName.value[0].category_name
  ) {
    randomSigns.value.splice(randomSigns.value.indexOf(item), 1)
    randomTenName.value.splice(randomTenName.value.indexOf(item), 1)
    randomTenName.value = getRandomShuffledItems(randomSigns.value, 1)
    if (!correctSigns.value.includes(item)) {
      console.log('correct', item)
      correctSigns.value.push(item)
      playSound('/sounds/new-notification-7-210334.mp3')
    }
    if (randomTenName.value.length === 0) {
      playSound('/sounds/crowd-cheer-ii-6263.mp3')
      $q.dialog({
        title: 'Muvafaqiatli yakunlandi',
        message: 'Tabrik',
        ok: 'Yopish',
        persistent: true,
      }).onOk(() => {
        audio?.pause()
        audio = null
        props.nextPage({
          nextPage: 0,
          category_id: props.data.category_id,
          data: props.data,
        })
      })
    }
  } else {
    if (!incorrectSigns.value.includes(item) && !correctSigns.value.includes(item)) {
      incorrectSigns.value.push(item)
      playSound('/sounds/error-10-206498.mp3')
      randomSigns.value.splice(randomSigns.value.indexOf(item), 1)
    }
    if (incorrectSigns.value.length === 5) {
      stopTimer()
      playSound('/sounds/game-over-31-179699.mp3')
      $q.dialog({
        title: 'Vaqt tugadi',
        message: "5 ta xatolikga yo'lqo'ydingiz!",
        ok: 'Yopish',
        persistent: true,
      }).onOk(() => {
        audio?.pause()
        audio = null
        props.nextPage({
          nextPage: 0,
          category_id: props.data.category_id,
          data: props.data,
        })
      })
    }
  }
}

// Misol uchun:
</script>

<style lang="scss" scoped>
.quasion-sign {
  padding: 30px;
  text-align: center;
  font-size: 20px;
  font-weight: 500;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.sign-name {
  width: auto;
  height: auto;
  padding: 20px;
  background-color: #ffffff72;
  color: #fff;
  border-radius: 15px;
  margin: 0 15px;
}
.ball {
  padding: 20px;
  background-color: #ffffff72;
  color: #fff;
  border-radius: 15px;
}
.timer {
  padding: 20px;
  background-color: #ffffff72;
  color: #fff;
  border-radius: 15px;
}
.card-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 10px;
  padding: 20px;
}

.card-item {
  width: 100px;
  height: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.card {
  cursor: pointer;
  width: 100%;
  height: 100%;
  display: flex;
  position: relative;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  border-radius: 15px;
  transition: all 0.3s ease;
  &:hover {
    scale: 1.05;
    box-shadow:
      rgba(255, 255, 255, 0.4) 0px 2px 4px,
      rgba(255, 255, 255, 0.3) 0px 7px 13px -3px,
      rgba(255, 255, 255, 0.2) 0px -3px 0px inset;
  }
  &.incorrect {
    box-shadow: 0 0 10px #ff4444;
    transform: scale(0.95);
    transition: all 0.3s ease;
    scale: 0.5;
  }
  &.correct {
    box-shadow: 0 0 10px #44ff44;
    transform: scale(0.95);
    transition: all 0.3s ease;
    scale: 0.5;
  }
  img {
    border-radius: 15px;
    width: 100%;
    height: 100%;
  }
  .status-correct,
  .status-incorrect {
    z-index: 2;
    position: absolute;
    width: 100%;
    height: 100%;
  }
}
@media (max-width: 768px) {
  .quasion-sign {
    flex-direction: column;
    gap: 10px;
  }
  .card-list {
    display: flex;
    flex-wrap: wrap;
    // gap: 10px;
    justify-content: space-between;
    padding: 10px;
  }
  // .card-item {
  //   width: 200px;
  //   height: 200px;
  // }
}
@media (max-width: 360px) {
  .card-list {
    display: flex;
    flex-wrap: wrap;
    gap: 5px;
  }
  .card-item {
    width: 60px;
    height: 60px;
  }
}
</style>
