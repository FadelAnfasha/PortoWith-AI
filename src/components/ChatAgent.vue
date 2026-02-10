<script setup>
import { ref, nextTick, watch } from "vue";
import { GoogleGenerativeAI } from "@google/generative-ai";
import { marked } from "marked"; // Import library-nya
import Button from "primevue/button";

const visible = ref(false);
const chatContainer = ref(null);
const userInput = ref("");
const loading = ref(false);

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

const scrollToBottom = async () => {
  await nextTick(); // Menunggu Vue selesai merender elemen baru
  if (chatContainer.value) {
    chatContainer.value.scrollTop = chatContainer.value.scrollHeight;
  }
};
const sendMessage = async () => {
  if (!userInput.value.trim()) return;

  const userText = userInput.value;
  messages.value.push({ role: "user", text: userText });
  userInput.value = "";
  loading.value = true; // Aktifkan loading

  scrollToBottom(); // Scroll setelah user kirim pesan

  try {
    const response = await fetch("http://localhost:11434/api/generate", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        model: "llama3.2",
        system: `Kamu adalah asisten virtual Fadel Anfasha Putra. Berikut adalah biodata Fadel: ${bioFadel}`,
        prompt: userText,
        stream: false,
      }),
    });

    const data = await response.json();
    messages.value.push({ role: "assistant", text: data.response });
  } catch (error) {
    messages.value.push({
      role: "assistant",
      text: "Maaf, server lokal saya sedang mati.",
    });
  } finally {
    loading.value = false;
    scrollToBottom(); // Scroll setelah asisten menjawab
  }
};

const renderMarkdown = (text) => {
  return marked.parse(text);
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
        class="p-4 bg-slate-800 border-b border-slate-700 font-bold text-sm text-white flex justify-between"
      >
        <span>AI Assistant Fadel</span>
        <span class="text-green-400">● Online</span>
      </div>

      <div
        ref="chatContainer"
        class="h-80 overflow-y-auto p-4 space-y-4 text-sm flex flex-col scroll-smooth"
      >
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
          <div
            v-if="msg.role === 'assistant'"
            v-html="renderMarkdown(msg.text)"
            class="markdown-content"
          ></div>
          <div v-else>{{ msg.text }}</div>
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
          class="flex-1 bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-xs text-white outline-none focus:border-blue-500 placeholder:text-slate-500"
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

.markdown-content :deep(ul) {
  list-style-type: disc;
  margin-left: 1.5rem;
  margin-bottom: 0.5rem;
}

.markdown-content :deep(strong) {
  font-weight: bold;
  color: #60a5fa; /* Warna biru biar cantik */
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
