<template>
  <div class="card-wrapper">
    <div class="card-list">
      <div
        class="card"
        v-for="category in props.categories"
        :key="category.id"
        @click="choseCategory(category)"
      >
        <img :src="category.img" alt="" />
        <p>{{ category.name.uz }}</p>
      </div>
    </div>
    <!-- <iframe
      width="560"
      height="315"
      src="https://fabulous-croquembouche-890ee8.netlify.app/"
      frameborder="0"
      allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
      referrerpolicy="strict-origin-when-cross-origin"
      allowfullscreen
    ></iframe> -->
    <q-dialog v-model="confirm" persistent>
      <q-card>
        <q-card-section class="row items-center q-pb-none">
          <q-space />
          <q-btn icon="close" flat round dense v-close-popup />
        </q-card-section>
        <q-card-section class="items-center">
          <h4>🎮 Yo‘l Belgilarini Topish O‘yini — Qoidalar</h4>
          <p>
            🚗 O‘yin boshlanishidan oldin:<br />
            Belgilar sonini tanlang – nechta belgini topmoqchisiz? <br />Vaqtni belgilang –
            belgilarning barchasini necha daqiqa ichida topasiz? <br /><br />O‘yin qanday
            ishlaydi?<br />
            1) O‘yin boshlangach, yuqori qismda bir belgi nomi ko‘rsatiladi.<br />
            2) Siz shu nomga mos keladigan belgini tanlashingiz kerak.<br />
            3) Belgilar ekranning pastki qismida tartibsiz joylashtirilgan. <br />
            4) O‘yin boshlanganda sizga +5 ball beriladi.<br />
            5) Har bir xato javob uchun -1 ball ayiriladi.<br />
            6) Agar ball 0 ga tushsa, o‘yin avtomatik tugaydi.<br />
            7) Agar xatolar soni 5 tadan kam bo‘lsa va barcha belgilar topilsa, o‘yin avtomatik
            tugaydi va: 🏆 "Siz g‘olib bo‘ldingiz!" deb chiqadi!
          </p>
          <q-select
            class="sign-select"
            standout
            v-model="countSigns"
            :options="options"
            label="belgilar soni"
          />
          <q-select
            class="timer-select"
            standout
            v-model="countTimer"
            :options="optionsTimer"
            label="daqiqa"
          />
        </q-card-section>

        <q-card-actions align="right">
          <q-btn flat label="o'yinni boshlash" color="primary" @click="sendData" />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useQuasar } from 'quasar'
const $q = useQuasar()
const props = defineProps({
  categories: {
    type: Array,
    required: true,
  },
  data: {
    type: Array,
    required: true,
  },
  category_id: {
    type: Number,
    required: true,
  },
  nextPage: {
    type: Function,
    required: true,
  },
})
const categoryId = ref(null)
const countSigns = ref(null)
const countTimer = ref(null)
const optionsTimer = ref([
  {
    label: '1 daqiqa',
    value: '1',
    description: 'belgini miqdorini tanlang',
  },
  {
    label: '3 daqiqa',
    value: '3',
    description: 'belgini miqdorini tanlang',
  },
  {
    label: '5 daqiqa',
    value: '5',
    description: 'belgini miqdorini tanlang',
  },
])
const options = ref([
  {
    label: 'belgilar soni 15ta',
    value: '15',
    description: 'belgini miqdorini tanlang',
  },
  {
    label: 'belgilar soni 25ta',
    value: '25',
    description: 'belgini miqdorini tanlang',
  },
  {
    label: 'belgilar soni 35ta',
    value: '35',
    description: 'belgini miqdorini tanlang',
  },
])
// onMounted(() => {
//   options.value = props.categories.map((category) => ({
//     label: category.name.uz,
//     value: category.id,
//   }))
// })
const confirm = ref(false)
const choseCategory = (category) => {
  console.log(category)
  confirm.value = true
  categoryId.value = category.id
  if (category.id === 1 || category.id === 3 || category.id === 8) {
    options.value = [
      {
        label: 'belgilar soni 15ta',
        value: '15',
        description: 'belgini miqdorini tanlang',
      },
      {
        label: 'belgilar soni 25ta',
        value: '25',
        description: 'belgini miqdorini tanlang',
      },
      {
        label: 'belgilar soni 35ta',
        value: '35',
        description: 'belgini miqdorini tanlang',
      },
    ]
    optionsTimer.value = [
      {
        label: '1 daqiqa',
        value: '1',
        description: 'belgini miqdorini tanlang',
      },
      {
        label: '3 daqiqa',
        value: '3',
        description: 'belgini miqdorini tanlang',
      },
      {
        label: '5 daqiqa',
        value: '5',
        description: 'belgini miqdorini tanlang',
      },
    ]
  } else if (category.id === 2) {
    options.value = [
      {
        label: 'belgilar soni 5ta',
        value: '5',
        description: 'belgini miqdorini tanlang',
      },
      {
        label: 'belgilar soni 9ta',
        value: '9',
        description: 'belgini miqdorini tanlang',
      },
    ]
    optionsTimer.value = [
      {
        label: '1 daqiqa',
        value: '1',
        description: 'belgini miqdorini tanlang',
      },
      {
        label: '3 daqiqa',
        value: '3',
        description: 'belgini miqdorini tanlang',
      },
    ]
  }
  // props.nextPage({
  //   nextPage: 1,
  //   category_id: category.id,
  //   data: props.data,
  // })
}
const sendData = () => {
  if (!countSigns.value || !countTimer.value) {
    $q.notify({
      type: 'negative',
      message: 'Belgilar soni va vaqtni tanlang',
    })
    return
  }
  confirm.value = false
  props.nextPage({
    nextPage: 1,
    category_id: categoryId.value,
    data: props.data,
    countSigns: Number(countSigns.value.value),
    countTimer: Number(countTimer.value.value),
  })
}
</script>

<style lang="scss" scoped>
.timer-select {
  margin-top: 20px;
}
.card-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  height: 100vh;
  width: 100%;
}
.card-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
}
.card {
  // transform: perspective(800px) rotateY(25deg) scale(0.9) rotateX(10deg);
  transform: scale(0.9);
  filter: blur(2px);
  opacity: 0.5;
  transition: 0.6s ease all;
  &:nth-child(1),
  &:nth-child(2) {
    transform: perspective(800px) rotateY(25deg) scale(0.9) rotateX(10deg);
    &:hover {
      transform: perspective(800px) rotateY(-15deg) translateY(-50px) rotateX(10deg) scale(1);
    }
  }
  &:nth-child(3),
  &:nth-child(4) {
    transform: perspective(800px) rotateY(-25deg) scale(0.9) rotateX(10deg);
    &:hover {
      transform: perspective(800px) rotateY(15deg) translateY(-50px) rotateX(10deg) scale(1);
    }
  }
  &:hover {
    transform: scale(1);

    // transform: perspective(800px) rotateY(-15deg) translateY(-50px) rotateX(10deg) scale(1);
    filter: blur(0);
    opacity: 1;
  }
}
.card {
  cursor: pointer;
  width: 200px;
  height: 250px;
  padding: 15px;
  background-color: #fff;
  border-radius: 15px;

  // box-shadow:
  //   rgba(50, 50, 93, 0.25) 0px 50px 100px -20px,
  //   rgba(0, 0, 0, 0.3) 0px 30px 60px -30px,
  //   rgba(10, 37, 64, 0.35) 0px -2px 6px 0px inset;
  // transition: all 0.7s linear;
  // &:hover {
  //   transition: all 0.7s linear;
  //   background-color: #c52330;
  //   color: #fff;
  //   box-shadow:
  //     rgb(204, 219, 232) 3px 3px 6px 0px inset,
  //     rgba(255, 255, 255, 0.5) -3px -3px 6px 1px inset;

  // }
  p {
    text-align: center;
    font-size: 18px;
    font-weight: 500;
  }
  img {
    border-radius: 15px;

    width: 100%;
  }
}
@media (max-width: 768px) {
  .card-list {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 5px;
  }

  .card {
    width: 110px;
    height: 160px;
    padding: 5px;
    filter: none;
    opacity: 1;
    transform: perspective(0px) rotateY(0deg) scale(0.9) rotateX(0deg) !important;
    &:hover {
      transform: perspective(0px) rotateY(0deg) translateY(0px) rotateX(0deg) scale(1) !important;
    }
    p {
      text-align: center;
      font-size: 14px;
      font-weight: 500;
    }
  }
}
</style>
