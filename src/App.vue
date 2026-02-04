<script setup>
import { ref, onMounted } from "vue";
import Button from "primevue/button";
import Card from "primevue/card";
import Tag from "primevue/tag";
import Timeline from "primevue/timeline";
import ChatAgent from "./components/ChatAgent.vue"; // Import komponen baru

const isLoaded = ref(false);
onMounted(() => {
  isLoaded.value = true;
});

const name = "Fadel Anfasha Putra";
const role = "Fullstack Developer & AI Enthusiast";

const openLink = (url) => {
  window.open(url, "_blank");
};

const contactWA = () => {
  const message =
    "Halo Fadel, saya melihat portofolio Anda dan tertarik untuk bekerja sama.";
  window.open(
    `https://wa.me/628983692104?text=${encodeURIComponent(message)}`,
    "_blank",
  );
};

const experiences = ref([
  {
    status: "Fullstack Developer",
    date: "2023 - Present",
    icon: "pi pi-briefcase",
    desc: "Developing enterprise solutions with Laravel & Vue.",
  },
  {
    status: "Data Automation Specialist",
    date: "2021 - 2023",
    icon: "pi pi-cog",
    desc: "Optimizing workflows using Python and VBA.",
  },
]);

const skills = [
  {
    name: "PHP",
    level: "Expert",
    logo: "https://cdn.jsdelivr.net/gh/devicons/devicon/icons/php/php-original.svg",
  },
  {
    name: "Laravel",
    level: "Expert",
    logo: "https://cdn.jsdelivr.net/gh/devicons/devicon/icons/laravel/laravel-original.svg",
  },
  {
    name: "Vue.js",
    level: "Advanced",
    logo: "https://cdn.jsdelivr.net/gh/devicons/devicon/icons/vuejs/vuejs-original.svg",
  },
  {
    name: "JavaScript",
    level: "Advanced",
    logo: "https://cdn.jsdelivr.net/gh/devicons/devicon/icons/javascript/javascript-original.svg",
  },
  {
    name: "Python",
    level: "Intermediate",
    logo: "https://cdn.jsdelivr.net/gh/devicons/devicon/icons/python/python-original.svg",
  },
  {
    name: "TensorFlow",
    level: "Intermediate",
    logo: "https://cdn.jsdelivr.net/gh/devicons/devicon/icons/tensorflow/tensorflow-original.svg",
  },
  {
    name: "Pandas",
    level: "Intermediate",
    logo: "https://cdn.jsdelivr.net/gh/devicons/devicon/icons/pandas/pandas-original.svg",
  },
  {
    name: "VBA Automation",
    level: "Expert",
    // Menggunakan logo Excel 2019-2026 yang modern
    logo: "https://upload.wikimedia.org/wikipedia/commons/3/34/Microsoft_Office_Excel_%282019%E2%80%93present%29.svg",
  },
];

const projects = [
  {
    title: "Neural Network Engine",
    desc: "Sistem otomasi berbasis AI untuk klasifikasi data perusahaan.",
    tags: ["Python", "Tensorflow"],
  },
  {
    title: "Internal ERP System",
    desc: "Integrasi sistem backend Laravel dengan automasi spreadsheet.",
    tags: ["Laravel", "VBA"],
  },
];
</script>

<template>
  <div class="min-h-screen bg-[#020617] ...">
    <ChatAgent />
  </div>
  <div
    class="min-h-screen bg-[#020617] text-slate-200 selection:bg-blue-500/30 font-sans relative overflow-hidden"
  >
    <div class="absolute inset-0 overflow-hidden pointer-events-none">
      <div
        class="absolute -top-[10%] -left-[10%] w-[40%] h-[40%] bg-blue-600/20 blur-[120px] rounded-full animate-blob"
      ></div>
      <div
        class="absolute top-[20%] -right-[10%] w-[35%] h-[35%] bg-indigo-600/20 blur-[120px] rounded-full animate-blob animation-delay-2000"
      ></div>
      <div
        class="absolute -bottom-[10%] left-[20%] w-[30%] h-[30%] bg-purple-600/15 blur-[120px] rounded-full animate-blob animation-delay-4000"
      ></div>
    </div>
    <div class="relative z-10"></div>
    <Transition name="fade">
      <div v-if="isLoaded" class="max-w-4xl mx-auto px-6 py-20">
        <header class="text-center mb-24">
          <div
            class="inline-block px-4 py-1.5 rounded-full border border-slate-700 bg-slate-800/50 text-blue-400 text-sm font-medium mb-6 animate-pulse"
          >
            Ready for Collaboration
          </div>

          <h1
            class="text-6xl font-black mb-6 tracking-tighter bg-gradient-to-r from-white via-slate-300 to-slate-500 bg-clip-text text-transparent"
          >
            {{ name }}
          </h1>
          <h2>{{ role }}</h2>

          <p
            class="text-xl text-slate-400 mb-10 max-w-xl mx-auto leading-relaxed"
          >
            Building robust web applications and intelligent automation systems.
          </p>

          <div class="flex flex-wrap justify-center gap-3 mb-8">
            <Button
              label="Hire Me"
              icon="pi pi-whatsapp"
              @click="contactWA"
              class="!bg-green-600 !border-none hover:!bg-green-700 hover:!scale-105 transition-all p-button-rounded px-6"
            />

            <Button
              icon="pi pi-github"
              @click="openLink('https://github.com/FadelAnfasha')"
              class="p-button-rounded p-button-outlined !border-slate-700 !text-white hover:!bg-slate-800"
            />

            <Button
              icon="pi pi-linkedin"
              @click="openLink('https://linkedin.com/in/fadelanfashap')"
              class="p-button-rounded p-button-outlined !border-slate-700 !text-blue-400 hover:!bg-slate-800"
            />

            <Button
              icon="pi pi-instagram"
              @click="openLink('https://instagram.com/fadelanfasha')"
              class="p-button-rounded p-button-outlined !border-slate-700 !text-pink-400 hover:!bg-slate-800"
            />
          </div>
        </header>

        <section class="mb-24">
          <h2
            class="text-xs uppercase tracking-[0.3em] text-slate-500 font-bold mb-8"
          >
            Specialized In
          </h2>
          <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
            <div
              v-for="skill in skills"
              :key="skill.name"
              class="group p-5 bg-slate-900/50 border border-slate-800 rounded-2xl hover:border-blue-500/50 hover:bg-slate-800 transition-all cursor-default flex items-center gap-4"
            >
              <img
                :src="skill.logo"
                :alt="skill.name"
                class="w-8 h-8 object-contain group-hover:scale-110 transition-transform shadow-sm"
              />

              <div>
                <p
                  class="text-sm font-bold text-slate-100 group-hover:text-blue-400"
                >
                  {{ skill.name }}
                </p>
                <p class="text-[10px] text-slate-500 uppercase tracking-wider">
                  {{ skill.level }}
                </p>
              </div>
            </div>
          </div>
        </section>

        <section class="mb-24">
          <h2
            class="text-xs uppercase tracking-[0.3em] text-slate-500 font-bold mb-12"
          >
            Career Journey
          </h2>
          <Timeline :value="experiences" class="customized-timeline">
            <template #marker="slotProps">
              <span
                class="flex w-8 h-8 items-center justify-center text-white rounded-full z-10 shadow-lg bg-blue-600"
              >
                <i :class="slotProps.item.icon"></i>
              </span>
            </template>
            <template #content="slotProps">
              <div class="mb-10 ml-4">
                <h3 class="font-bold text-lg text-white">
                  {{ slotProps.item.status }}
                </h3>
                <small class="text-blue-400 block mb-2">{{
                  slotProps.item.date
                }}</small>
                <p class="text-slate-400 leading-relaxed">
                  {{ slotProps.item.desc }}
                </p>
              </div>
            </template>
          </Timeline>
        </section>

        <section>
          <h2
            class="text-xs uppercase tracking-[0.3em] text-slate-500 font-bold mb-8"
          >
            Selected Projects
          </h2>
          <div class="grid md:grid-cols-2 gap-6">
            <div
              v-for="p in projects"
              :key="p.title"
              class="p-6 bg-slate-900/40 border border-slate-800 rounded-3xl hover:-translate-y-2 transition-all duration-300 shadow-xl"
            >
              <h4 class="text-xl font-bold text-white mb-3">{{ p.title }}</h4>
              <p class="text-slate-400 text-sm mb-5 leading-relaxed">
                {{ p.desc }}
              </p>
              <div class="flex gap-2">
                <Tag
                  v-for="tag in p.tags"
                  :key="tag"
                  :value="tag"
                  class="!bg-slate-800 !text-slate-300 !text-[10px]"
                />
              </div>
            </div>
          </div>
        </section>
      </div>
    </Transition>

    <footer class="py-10 text-center border-t border-slate-900 mt-20">
      <p class="text-slate-600 text-sm">© 2026 Crafted by Fadel</p>
    </footer>
  </div>
</template>

<style>
@import "tailwindcss";

/* Definisi Animasi Floating Blob */
@keyframes blob {
  0% {
    transform: translate(0px, 0px) scale(1);
  }
  33% {
    transform: translate(30px, -50px) scale(1.1);
  }
  66% {
    transform: translate(-20px, 20px) scale(0.9);
  }
  100% {
    transform: translate(0px, 0px) scale(1);
  }
}

.animate-blob {
  animation: blob 7s infinite alternate ease-in-out;
}

.animation-delay-2000 {
  animation-delay: 2s;
}

.animation-delay-4000 {
  animation-delay: 4s;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 1s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

/* Custom Timeline Style */
.p-timeline-event-opposite {
  display: none;
}
.p-timeline-event-content {
  padding-left: 1rem;
}
</style>
