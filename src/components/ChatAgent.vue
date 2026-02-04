<script setup>
import { ref } from "vue";
import { GoogleGenerativeAI } from "@google/generative-ai";
import Button from "primevue/button";

const visible = ref(false);
const userInput = ref("");
const messages = ref([
  {
    role: "assistant",
    text: "Halo! Saya AI Assistant Fadel. Ada yang bisa saya bantu terkait proyek Laravel atau AI Fadel?",
  },
]);
const loading = ref(false);

// Inisialisasi AI (Ambil Key dari Environment Variable)
const genAI = new GoogleGenerativeAI(import.meta.env.VITE_GEMINI_API_KEY);
const model = genAI.getGenerativeModel({
  model: "gemini-1.5-flash",
  systemInstruction:
    "Kamu adalah asisten virtual Fadel Anfasha Putra. Jawablah pertanyaan pengunjung tentang pengalaman Fadel di Laravel, Vue, dan AI dengan ramah dan profesional.",
});

const sendMessage = async () => {
  if (!userInput.value.trim()) return;

  const userText = userInput.value;
  messages.value.push({ role: "user", text: userText });
  userInput.value = "";
  loading.value = true;

  try {
    const result = await model.generateContent(userText);
    const response = await result.response;
    messages.value.push({ role: "assistant", text: response.text() });
  } catch (error) {
    messages.value.push({
      role: "assistant",
      text: "Maaf, sepertinya saya sedang offline. Coba lagi nanti ya!",
    });
  } finally {
    loading.value = false;
  }
};
</script>

<template>
  <div class="fixed bottom-6 right-6 z-[100]">
    <Button
      @click="visible = !visible"
      :icon="visible ? 'pi pi-times' : 'pi pi-comment'"
      rounded
      class="shadow-2xl !w-14 !h-14 !bg-blue-600 !border-none"
    />

    <div
      v-if="visible"
      class="absolute bottom-20 right-0 w-80 md:w-96 bg-slate-900 border border-slate-800 rounded-2xl shadow-2xl flex flex-col overflow-hidden animate-fade-in-up"
    >
      <div
        class="p-4 bg-slate-800 border-b border-slate-700 font-bold text-sm flex justify-between"
      >
        <span>AI Assistant Fadel</span>
        <span class="text-green-400">● Online</span>
      </div>

      <div class="h-80 overflow-y-auto p-4 space-y-4 text-sm flex flex-col">
        <div
          v-for="(msg, i) in messages"
          :key="i"
          :class="
            msg.role === 'user'
              ? 'self-end bg-blue-600 text-white'
              : 'self-start bg-slate-800 text-slate-200'
          "
          class="max-w-[80%] p-3 rounded-2xl"
        >
          {{ msg.text }}
        </div>
        <div
          v-if="loading"
          class="self-start bg-slate-800 p-3 rounded-2xl animate-pulse"
        >
          Mengetik...
        </div>
      </div>

      <div class="p-4 bg-slate-800 flex gap-2">
        <input
          v-model="userInput"
          @keyup.enter="sendMessage"
          placeholder="Tanya sesuatu..."
          class="flex-1 bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-xs outline-none focus:border-blue-500"
        />
        <Button
          icon="pi pi-send"
          @click="sendMessage"
          class="!bg-blue-600 !border-none !w-10 !h-10"
        />
      </div>
    </div>
  </div>
</template>

<style scoped>
.animate-fade-in-up {
  animation: fadeInUp 0.3s ease-out;
}
@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
