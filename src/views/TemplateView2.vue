<script setup>
import {
  ref,
  reactive,
  watch,
  computed,
  onActivated,
  onMounted,
  onUnmounted,
} from 'vue'

// リアクティブな変数には
// ref・reactiveの２パターン書き方がある
const message = ref('Hello world')
const user = reactive({
  name: 'User name',
  age: 999,
})

watch(
  () => message.value,
  () => {
    // ここに処理を書く
  },
)

const actionName = computed(() => {
  // ここに処理を書く
  return 'message'
})

function onClickAction() {
  // ここに処理を書く
}

const myAction = () => {
  // 関数はこのように書くこともできる
}

onActivated(() => {
  // ここに初期処理を書く
})

onMounted(() => {
  // ここに初期処理を書く
})

onUnmounted(() => {
  // ここに画面を去る際の処理を書く
})
</script>

<template>
  <span>{{ message }}</span>

  <div>
    {{ user }}
    <div>name: {{ user.name }}</div>
    <div>age: {{ user.age }}</div>
  </div>

  <button @click="onClickAction">Button</button>
</template>

<style>
/* ここにCSSを追加する */
</style>
