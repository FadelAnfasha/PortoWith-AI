<script setup>
import { ref, onMounted, computed } from "vue";
import Button from "primevue/button";
import Tag from "primevue/tag";
import Timeline from "primevue/timeline";
import ChatAgent from "./components/ChatAgent.vue"; // Import komponen baru
import profileImage from "./assets//img/pp.jpg";
const selectedCategory = ref("topy");

const filteredProjects = computed(() => {
  return projects.filter((p) => p.category === selectedCategory.value);
});

const selectedCompanyName = computed(() => {
  const experience = experiences.value.find(
    (exp) => exp.id === selectedCategory.value,
  );
  return experience ? experience.company : "Project Lainnya";
});

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
    name: "Ollama",
    level: "Beginner",
    logo: "https://ollama.com/public/ollama.png",
  },
  // {
  //   name: "VBA Automation",
  //   level: "Expert",
  //   // Menggunakan logo Excel 2019-2026 yang modern
  //   logo: "https://upload.wikimedia.org/wikipedia/commons/3/34/Microsoft_Office_Excel_%282019%E2%80%93present%29.svg",
  // },
];

const experiences = ref([
  {
    id: "topy",
    status: "Finance, Accounting, & Tax Dept. Supervisor",
    company: "PT. Topy Palingda Manufacturing Indonesia",
    date: "2024 - 2026",
    icon: "pi pi-chart-line",
    desc: "Leading the digitalization of financial processes and developing web-based internal applications to enhance departmental efficiency.",
  },
  {
    id: "procodecg",
    status: "AI Internship",
    company: "ProCodeCG",
    date: "2021",
    icon: "pi pi-database",
    desc: "Focused on data preparation (labeling) to train machine learning models for architectural building recognition.",
  },
  {
    id: "telkom",
    status: "Internship",
    company: "Telkom Indonesia (Karawang Region)",
    date: "2020",
    icon: "pi pi-comment",
    desc: "Built automated chatbot solutions for customer subscription monitoring systems.",
  },
]);

const projects = [
  // Projects at PT. Topy
  {
    title: "Web-Based Internal Finance App",
    desc: "An application to calculate price variances between actual and standard raw materials, as well as production processing costs for all finished goods.",
    tags: ["Laravel", "Vue.js", "Finance"],
    category: "topy",
  },
  {
    title: "Hybrid Tax Invoice System",
    desc: "Transformed the tax invoice exchange system from manual (offline) to a hybrid online-offline system.",
    tags: ["Digital Transformation", "Tax"],
    category: "topy",
  },
  {
    title: "Accounting Automation Macros",
    desc: "Automating the generation of Statements of Account, tax reports, and withholding tax slips using VBA Excel Macros.",
    tags: ["VBA", "Excel", "Automation"],
    category: "topy",
  },
  {
    title: "Digital Entertainment Form",
    desc: "Digitalized manual entertainment request forms into an integrated digital platform.",
    tags: ["Laravel", "Bootstrap", "Digitalization"],
    category: "topy",
  },

  // Project at ProCodeCG
  {
    title: "ML Training Data: Building Blueprint",
    desc: "Executed data labeling for building blueprints to train ML model accuracy in detecting house corner types.",
    tags: ["Machine Learning", "Data Labeling"],
    category: "procodecg",
  },

  // Project at Telkom
  {
    title: "Telegram Monitoring Bot",
    desc: "A Telegram chatbot for real-time monitoring of customer subscription statuses.",
    tags: ["Telegram Webhook", "Monitoring"],
    category: "telkom",
  },
];
</script>

<template>
  <!-- <ChatAgent /> -->
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
          <div class="mb-8 flex justify-center">
            <div class="relative group">
              <div
                class="absolute -inset-1 bg-gradient-to-r from-blue-600 to-indigo-600 rounded-full blur opacity-25 group-hover:opacity-50 transition duration-1000"
              ></div>
              <img
                :src="profileImage"
                alt="Fadel's Profile"
                class="relative w-128 h-124 rounded-full border-2 border-slate-800 object-cover shadow-2xl"
              />
            </div>
          </div>
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
            <template #content="slotProps">
              <div
                @click="selectedCategory = slotProps.item.id"
                :class="[
                  'mb-10 ml-4 p-4 rounded-xl cursor-pointer transition-all duration-300 border border-transparent',
                  selectedCategory === slotProps.item.id
                    ? 'bg-blue-600/10 border-blue-500/50 shadow-[0_0_20px_rgba(37,99,235,0.1)]'
                    : 'hover:bg-slate-800/50',
                ]"
              >
                <h3
                  :class="[
                    'font-bold text-lg',
                    selectedCategory === slotProps.item.id
                      ? 'text-blue-400'
                      : 'text-white',
                  ]"
                >
                  {{ slotProps.item.status }}
                </h3>
                <small class="text-slate-500 block mb-2">{{
                  slotProps.item.date
                }}</small>
                <p class="text-slate-400 text-sm leading-relaxed">
                  {{ slotProps.item.desc }}
                </p>
              </div>
            </template>
          </Timeline>
        </section>

        <section>
          <div class="flex items-center justify-between mb-8">
            <h2
              class="text-xs uppercase tracking-[0.3em] text-slate-500 font-bold"
            >
              Projects during this period
            </h2>

            <Tag
              :value="selectedCompanyName"
              class="!bg-blue-600/20 !text-blue-400 !px-4 !py-1.5 !rounded-lg"
            />
          </div>

          <div class="grid md:grid-cols-2 gap-6">
            <div
              v-for="p in filteredProjects"
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
