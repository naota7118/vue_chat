<script setup>
import { ref } from 'vue'
import OpenAI from 'openai'

const client = new OpenAI({
  apiKey: import.meta.env.VITE_OPENAI_API_KEY,
  dangerouslyAllowBrowser: true,
})

const message = ref('')

// ChatGPTの返答の初期化
let chat_response = ref('')

// ChatGPT APIにメッセージを送る
const send = async () => {
  const response = await client.chat.completions.create({
    messages: [{ role: 'user', content: message.value }],
    model: 'gpt-4o',
  })
  chat_response.value = response.choices[0].message.content
}
</script>

<template>
  <div>
    <p>ChatGPTに送る内容を入力してください。</p>
    <textarea v-model="message" type="text" />
    <button @click="send">送信</button>
  </div>
  <div v-if="chat_response">
    <p>{{ chat_response }}</p>
  </div>
</template>

<style scoped>
button {
  margin-left: 10px;
}
</style>
