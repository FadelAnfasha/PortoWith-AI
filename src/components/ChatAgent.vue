<script setup>
import { ref, nextTick, watch } from "vue";
import { GoogleGenerativeAI } from "@google/generative-ai";
import { marked } from "marked"; // Import library-nya

const visible = ref(false);
const chatContainer = ref(null);
const userInput = ref("");
const loading = ref(false);
const isOnline = ref(false); // Status online/offline
const apiKey = import.meta.env.VITE_GEMINI_API_KEY;

// Cek status API saat komponen dimount
const checkStatus = async () => {
  if (!apiKey) {
    isOnline.value = false;
    return;
  }

  try {
    // Coba inisialisasi model dummy untuk cek koneksi/key
    const genAI = new GoogleGenerativeAI(apiKey);
    const model = genAI.getGenerativeModel({ model: "gemini-1.5-flash" });
    // Ping simple
    await model.generateContent("ping");
    isOnline.value = true;
  } catch (error) {
    console.error("Gemini connection failed:", error);
    isOnline.value = false;
  }
};

// Auto scroll ke bawah
const scrollToBottom = async () => {
  await nextTick();
  if (chatContainer.value) {
    chatContainer.value.scrollTop = chatContainer.value.scrollHeight;
  }
};

// Watch visibility untuk cek status saat dibuka pertama kali
watch(visible, (newVal) => {
  if (newVal) {
    checkStatus();
    scrollToBottom();
  }
});

const messages = ref([
  {
    role: "assistant",
    text: "Halo! Saya AI Assistant Fadel. Ada yang bisa saya bantu terkait proyek Laravel atau AI Fadel?",
  },
]);

const bioFadel = `
NAMA: Fadel Anfasha Putra
PERAN: Fullstack Developer & AI Enthusiast
KEAHLIAN UTAMA:
- Backend: Laravel (PHP)
- Frontend: Vue.js, JavaScript, Tailwind CSS
- AI/Data: Python,TensorFlow, Pandas, Automation Scripts

RIWAYAT PEKERJAAN:
a.Telkom Indonesia (Witel Karawang) 
  Tahun : 2020
  Posisi : Internship
  Pencapaian : Membuat ChatBot pada aplikasi telegram untuk monitoring customer subscription.
b. ProCodeCG:
  Tahun : 2021
  Posisi : Internship
  Pencapaian : Labeling blueprint bangunan untuk melatih model machine learning agar dapat membedakan jenis sudut-sudut rumah.
c. PT. Topy Palingda Manufacturing Indonesia
  Tahun : 2024-2026
  Posisi : Supervisor Departemen Finance, Accounting, & Tax
  Pencapaian :  -Membuat aplikasi internal departemen berbasis web untuk menghitung selisih harga bahan baku yang digunakan secara aktual dengan standard.
                -Menambahkan aplikasi untung menghitung biaya proses dalam produksi seluruh finish good pada aplikasi internal departemen.
                -Menambahkan aplikasi Request for service pada aplikasi internal departemen.
                -Membuat macro excel untuk otomatisasi pembuatan Statement of Account.
                -Membuat macro excel untuk otomatisasi pembuatan laporan pajak masuk dan keluar.
                -Membuat macro excel untuk otomatisasi pembuatan laporan bukti potong.
                -Membuat sistem penukaran faktur pajak menjadi hybrid (Online dan Offline) dari yang sebelumnya offline.
                -Mendigitalisasi form entertainment menjadi form digital pada aplikasi internal perusahaan.

GAYA KOMUNIKASI:
- Ramah, profesional, dan to-the-point.
- Gunakan Bahasa Indonesia yang santai tapi sopan.
- Jika ditanya hal di luar topik koding/Fadel, arahkan kembali dengan sopan untuk menghubungi langsung baik via WA,Email atau LinkedIn.

FORMAT JAWABAN:
- Gunakan bullet points (-) untuk daftar keahlian atau pengalaman.
- Gunakan baris baru (Enter) yang jelas antar poin.
- Gunakan Bold (**) hanya untuk judul atau poin penting.
- JANGAN menulis daftar dalam satu paragraf panjang.
`;

// scrollToBottom moved up
const sendMessage = async () => {
  if (!userInput.value.trim()) return;

  if (!isOnline.value) {
    messages.value.push({
      role: "assistant",
      text: "⚠️ **Offline mode**: API Key invalid atau koneksi bermasalah. Cek .env file anda.",
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
      model: "gemini-1.5-flash",
      systemInstruction: `Kamu adalah asisten virtual Fadel Anfasha Putra. Berikut adalah biodata Fadel: ${bioFadel}. Jawablah dengan format Markdown yang rapi.`,
    });

    const result = await model.generateContent(userText);
    const response = await result.response;
    const text = response.text();

    messages.value.push({ role: "assistant", text: text });
  } catch (error) {
    console.error("Gemini Error:", error);
    isOnline.value = false; // Set offline jika error
    messages.value.push({
      role: "assistant",
      text: "❌ Gagal terhubung ke Google Gemini. Cek koneksi internet anda.",
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
        <div class="flex items-center gap-1.5 bg-slate-800 px-2 py-1 rounded-full border border-slate-700">
          <div
            :class="['w-2 h-2 rounded-full animate-pulse', isOnline ? 'bg-green-500 shadow-[0_0_8px_rgba(34,197,94,0.6)]' : 'bg-red-500']">
          </div>
          <span :class="['text-[10px] font-medium', isOnline ? 'text-green-400' : 'text-red-400']">
            {{ isOnline ? 'Online' : 'Offline' }}
          </span>
        </div>
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
        <v-text-field v-model="userInput" @keyup.enter="sendMessage" placeholder="Tanya tentang pengalaman Fadel..."
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
  /* Warna biru biar cantik */
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
