<script setup>
import { ref } from 'vue'
import OpenAI from 'openai'

const client = new OpenAI({
  apiKey: import.meta.env.VITE_OPENAI_API_KEY,
  dangerouslyAllowBrowser: true,
})

// フォームに入力したテキストの初期化(リアクティブシステムに含めるため)
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
  <div class="container">
    <div class="chat_area">
      <div>
        <textarea
          v-model="message"
          type="text"
          placeholder="ChatGPTに送る内容を入力してください。"
          class="message"
        />
        <button @click="send">送信</button>
      </div>
      <div v-if="chat_response" class="chat_response">
        <p>{{ chat_response }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
button {
  margin-left: 10px;
}
.container {
  display: flex;
  align-items: center;
  justify-content: center;
}
.chat_area {
  display: flex;
  flex-direction: column;
}
.message,
.chat_area {
  width: 300px;
  height: 100px;
}

.message {
  background: #f5f5f5;
  padding: 10px;
  border: none;
  border-radius: 10px;
  resize: none;
}
</style>
