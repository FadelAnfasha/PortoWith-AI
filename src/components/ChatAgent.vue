<script setup>
import { ref, nextTick, watch } from "vue";
import { GoogleGenerativeAI } from "@google/generative-ai";
import { marked } from "marked"; // Import library

const visible = ref(false);
const chatContainer = ref(null);
const userInput = ref("");
const loading = ref(false);
const isOnline = ref(false); // Online/offline status
const apiKey = import.meta.env.VITE_GEMINI_API_KEY;

// Check API status when component is mounted
const checkStatus = async () => {
  if (!apiKey) {
    isOnline.value = false;
    return;
  }

  try {
    // Initialize dummy model to check connection/key
    const genAI = new GoogleGenerativeAI(apiKey);
    const model = genAI.getGenerativeModel({ model: "gemini-2.5-flash" });
    // Simple ping
    // await model.generateContent("ping");
    isOnline.value = true;
  } catch (error) {
    console.error("Gemini connection failed:", error);
    isOnline.value = false;
  }
};

// Auto scroll to bottom
const scrollToBottom = async () => {
  await nextTick();
  if (chatContainer.value) {
    chatContainer.value.scrollTop = chatContainer.value.scrollHeight;
  }
};

// Watch visibility to check status when opened for the first time
watch(visible, (newVal) => {
  if (newVal) {
    checkStatus();
    scrollToBottom();
  }
});

const messages = ref([
  {
    role: "assistant",
    text: "Hello! I am Fadel's AI Assistant. How can I help you regarding Fadel's Laravel or AI projects?",
  },
]);

const bioFadel = `
NAME: Fadel Anfasha Putra
ROLE: Fullstack Developer & AI Enthusiast
CORE SKILLS:
- Backend: Laravel (PHP)
- Frontend: Vue.js, JavaScript, Tailwind CSS
- AI/Data: Python, TensorFlow, Pandas, Automation Scripts

WORK HISTORY:
a. Telkom Indonesia (Witel Karawang) 
   Year: 2020
   Position: Internship
   Achievement: Built a Telegram ChatBot for monitoring customer subscriptions.
b. ProCodeCG:
   Year: 2021
   Position: Internship
   Achievement: building labeling blueprint for machine learning models to distinguish house corner types.
c. PT. Topy Palingda Manufacturing Indonesia
   Year: 2024-2026
   Position: Supervisor of Finance, Accounting, & Tax Department
   Achievements:  - Built an internal web application for calculating raw material price variances.
                 - Added specialized modules for production cost calculation.
                 - Implemented a "Request for Service" system in the internal app.
                 - Developed Excel macros for Statement of Account automation.
                 - Developed macros for tax reporting (Inbound/Outbound).
                 - Developed macros for withholding tax certificates.
                 - Modernized tax invoice submission to a hybrid system (Online/Offline).
                 - Digitalized company entertainment forms.

COMMUNICATION STYLE:
- Friendly, professional, and to-the-point.
- Use professional yet approachable English.
- If asked about topics outside of coding or Fadel's profile, politely redirect them to contact Fadel directly via WhatsApp, Email, or LinkedIn.

RESPONSE FORMAT:
- Use bullet points (-) for listing skills or experiences.
- Use clear line breaks between points.
- Use Bold (**) only for titles or critical points.
- DO NOT write lists in single long paragraphs.
`;

const sendMessage = async () => {
  if (!userInput.value.trim()) return;

  if (!isOnline.value) {
    messages.value.push({
      role: "assistant",
      text: "⚠️ **Offline mode**: Invalid API Key or connection issues. Check your .env file.",
    });
    scrollToBottom();
    return;
  }

  const userText = userInput.value;
  messages.value.push({ role: "user", text: userText });
  userInput.value = "";
  loading.value = true;

  scrollToBottom();

  try {
    const genAI = new GoogleGenerativeAI(apiKey);
    const model = genAI.getGenerativeModel({
      model: "gemini-2.5-flash",
      systemInstruction: `You are Fadel Anfasha Putra's virtual assistant. Here is Fadel's bio: ${bioFadel}. Answer in neat Markdown format.`,
    });

    const result = await model.generateContent(userText);
    const response = await result.response;
    const text = response.text();

    messages.value.push({ role: "assistant", text: text });
  } catch (error) {
    console.error("Gemini Error:", error);
    isOnline.value = false; // Set offline on error
    messages.value.push({
      role: "assistant",
      text: "❌ Failed to connect to Google Gemini. Check your internet connection.",
    });
  } finally {
    loading.value = false;
    scrollToBottom();
  }
};

const renderMarkdown = (text) => {
  return marked.parse(text);
};
</script>

<template>
  <div class="fixed bottom-6 right-6 z-[100]">
    <v-btn @click="visible = !visible" :icon="visible ? 'mdi-close' : 'mdi-message-text'" color="blue" size="large"
      class="shadow-2xl"></v-btn>

    <div v-if="visible"
      class="absolute bottom-20 right-0 w-80 md:w-96 bg-slate-900 border border-slate-800 rounded-2xl shadow-2xl flex flex-col overflow-hidden animate-fade-in-up">
      <div class="p-4 bg-slate-900 border-b border-slate-800 flex justify-between items-center shadow-md z-10">
        <div class="flex items-center gap-2">
          <v-avatar color="blue" size="32">
            <v-icon icon="mdi-robot" color="white" size="small"></v-icon>
          </v-avatar>
          <div>
            <span class="font-bold text-sm text-white block leading-tight">PortoBot AI</span>
            <span class="text-[10px] text-slate-400">Powered by Gemini</span>
          </div>
        </div>
        <!-- <div class="flex items-center gap-1.5 bg-slate-800 px-2 py-1 rounded-full border border-slate-700">
          <div
            :class="['w-2 h-2 rounded-full animate-pulse', isOnline ? 'bg-green-500 shadow-[0_0_8px_rgba(34,197,94,0.6)]' : 'bg-red-500']">
          </div>
          <span :class="['text-[10px] font-medium', isOnline ? 'text-green-400' : 'text-red-400']">
            {{ isOnline ? 'Online' : 'Offline' }}
          </span>
        </div> -->
      </div>

      <div ref="chatContainer"
        class="h-96 overflow-y-auto p-4 space-y-4 text-sm flex flex-col bg-slate-950 scroll-smooth">
        <div v-for="(msg, i) in messages" :key="i" :class="[
          'max-w-[85%] rounded-2xl p-3.5 shadow-sm transition-all duration-300',
          msg.role === 'user'
            ? 'self-end bg-blue-600 text-white rounded-br-none'
            : 'self-start bg-slate-800 text-slate-200 rounded-bl-none border border-slate-700'
        ]">
          <div v-if="msg.role === 'assistant'" v-html="renderMarkdown(msg.text)" class="markdown-content"></div>
          <div v-else>{{ msg.text }}</div>
        </div>

        <div v-if="loading"
          class="self-start bg-slate-800 border border-slate-700 p-3 rounded-2xl rounded-bl-none flex items-center gap-1 w-16 h-10">
          <span class="w-1.5 h-1.5 bg-slate-400 rounded-full animate-bounce"></span>
          <span class="w-1.5 h-1.5 bg-slate-400 rounded-full animate-bounce delay-100"></span>
          <span class="w-1.5 h-1.5 bg-slate-400 rounded-full animate-bounce delay-200"></span>
        </div>
      </div>

      <div class="p-3 bg-slate-900 border-t border-slate-800 flex gap-2 items-center">
        <v-text-field v-model="userInput" @keyup.enter="sendMessage" placeholder="Ask about Fadel's experience..."
          variant="solo-filled" density="compact" hide-details class="flex-1 rounded-lg" bg-color="slate-800"
          base-color="white" color="blue" :disabled="loading">
          <template v-slot:append-inner>
            <v-btn icon="mdi-send" size="small" variant="text" color="blue" @click="sendMessage"
              :disabled="!userInput.trim() || loading"></v-btn>
          </template>
        </v-text-field>
      </div>
    </div>
  </div>
</template>

<style scoped>
.animate-fade-in-up {
  animation: fadeInUp 0.3s ease-out;
}

.markdown-content :deep(ul) {
  list-style-type: disc;
  margin-left: 1.5rem;
  margin-bottom: 0.5rem;
}

.markdown-content :deep(strong) {
  font-weight: bold;
  color: #60a5fa;
  /* Blue color for aesthetic */
}

.markdown-content :deep(p) {
  margin-bottom: 0.5rem;
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
