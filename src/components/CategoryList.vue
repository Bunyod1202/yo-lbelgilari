<template>
  <div>
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
  if (category.id === 1) {
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
.card-list {
  width: 100%;
  max-width: 1000px;
  margin: 30px auto;
  display: flex;
  gap: 20px;
}
.card {
  width: 200px;
  height: 250px;
  padding: 15px;
  border-radius: 15px;
  overflow: hidden;
  box-shadow: 0 0 10px #000;
  p {
    text-align: center;
    font-size: 18px;
    font-weight: 500;
  }
  img {
    width: 100%;
  }
}
</style>
