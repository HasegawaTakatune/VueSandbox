<script setup lang="ts">
import { reactive, ref, watch, computed, onMounted } from 'vue'

// ユーザ名
const name = ref('')

// 時刻と時刻ごとのアイコン
const time = reactive({
  value: '00:00',
  icon: 'sunny',
})

// 一定間隔処理ID
const intervalId = ref(-1)

// 時間を監視してアイコンを更新する
watch(
  () => time.value,
  () => {
    // 時間ごとにアイコンを切り替える
    const hours = parseInt(time.value.slice(0, 2))
    if (2 <= hours && hours < 12) time.icon = 'sunny'
    else if (12 <= hours && hours < 18) time.icon = 'sun-oversea'
    else time.icon = 'moon'
  },
)

// ユーザ名と時間の変更を検知して文字を更新する
const showGreeting = computed(() => {
  // ユーザ名未入力なら入力を促す
  if (!name.value) {
    return "Hi, What's your name?"
  }

  // 時間ごとに挨拶を切り替える
  const hours = parseInt(time.value.slice(0, 2))
  if (2 <= hours && hours < 12) return `Good morning, ${name.value}`
  else if (12 <= hours && hours < 18) return `Good afternoon, ${name.value}`
  else return `Good evening, ${name.value}`
})

// 現在時刻を更新する
const updateTime = () => {
  const today = new Date()
  const hours = `00${today.getHours()}`.slice(-2)
  const minutes = `00${today.getMinutes()}`.slice(-2)
  time.value = `${hours}:${minutes}`
}

// 時刻入力イベント
const onInputTime = () => {
  // 間隔処理をキャンセルする
  clearInterval(intervalId.value)
  intervalId.value = -1
}

// 時刻リセットクリックイベント
const onClickResetTime = () => {
  // 間隔処理を起動する
  if (intervalId.value == -1) {
    intervalId.value = setInterval(updateTime, 1 * 1000)
  }
}

// 初期処理
onMounted(() => {
  updateTime()

  intervalId.value = setInterval(updateTime, 1 * 1000)
})
</script>

<template>
  <div class="wrapper">
    <h2 class="content">
      {{ showGreeting }}
    </h2>

    <!-- 名前入力 -->
    <input
      class="input-name"
      type="text"
      name="name"
      id="name"
      placeholder="Your name"
      v-model="name"
    />

    <div>
      <!-- 時刻ごとのアイコン -->
      <img
        v-if="time.icon == 'sunny'"
        class="icon"
        src="@/assets/sunny.svg"
        width="30"
        height="30"
      />
      <img
        v-if="time.icon == 'sun-oversea'"
        class="icon"
        src="@/assets/sun-oversea.svg"
        width="30"
        height="30"
      />
      <img
        v-if="time.icon == 'moon'"
        class="icon"
        src="@/assets/moon.svg"
        width="30"
        height="30"
      />

      <!-- 時刻入力 -->
      <input
        type="time"
        name="time"
        id="time"
        v-model="time.value"
        @input="onInputTime"
      />
      <!-- 時刻リセットボタン -->
      <button @click="onClickResetTime">Reset time</button>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  text-align: -webkit-center;
}

.content {
  font-size: 4rem;
  margin: 8rem 0 14rem 0;
}

.icon {
  vertical-align: middle;
}

input,
button {
  background: var(--color-background);
  color: var(--color-text);
  margin: 0 0 10px 5px;
  border: none;
  &.input-name {
    display: block;
    border-bottom: solid var(--color-text);
  }
}
</style>
