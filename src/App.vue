<script setup>
import { ref, onMounted, computed, watch } from "vue";
import ChatAgent from "./components/ChatAgent.vue";
import profileImage from "./assets//img/pp.jpg";
import hikingImage from "./assets/img/hiking.jpg";
import readingImage from "./assets/img/reading.jpg";
import runningImage from "./assets/img/running.jpg";
import codingImage from "./assets/img/coding.png";

const selectedCategory = ref("topy");

// Typing Effect
const typedRole = ref("");
const roles = ["Fullstack Developer", "AI Enthusiast", "System Analyst"];
let roleIndex = 0;
let charIndex = 0;
let isDeleting = false;

const typeRole = () => {
  const currentRole = roles[roleIndex];
  if (isDeleting) {
    typedRole.value = currentRole.substring(0, charIndex - 1);
    charIndex--;
  } else {
    typedRole.value = currentRole.substring(0, charIndex + 1);
    charIndex++;
  }

  if (!isDeleting && charIndex === currentRole.length) {
    isDeleting = true;
    setTimeout(typeRole, 2000);
  } else if (isDeleting && charIndex === 0) {
    isDeleting = false;
    roleIndex = (roleIndex + 1) % roles.length;
    setTimeout(typeRole, 500);
  } else {
    setTimeout(typeRole, isDeleting ? 50 : 100);
  }
};

// Scroll Animation
const observeElements = () => {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add("show-content");
        }
      });
    },
    { threshold: 0.1 }
  );

  const hiddenElements = document.querySelectorAll(".hidden-content");
  hiddenElements.forEach((el) => observer.observe(el));
};



const filteredProjects = computed(() => {
  console.log(selectedCategory.value);
  console.log(projects.filter((p) => p.category === selectedCategory.value));
  return projects.filter((p) => p.category === selectedCategory.value);
});

const selectedCompanyName = computed(() => {
  const experience = experiences.value.find(
    (exp) => exp.id === selectedCategory.value,
  );
  return experience ? experience.company : "Project Lainnya";
});

const getSkillLogo = (tagName) => {
  const skill = skills.find(s => s.name.toLowerCase() === tagName.toLowerCase());
  return skill ? skill.logo : null;
};

const isLoaded = ref(false);
onMounted(() => {
  isLoaded.value = true;
  typeRole(); // Start typing effect
  setTimeout(() => {
    observeElements(); // Initialize scroll observer
  }, 100);
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

const hobbies = [
  {
    name: "Hiking",
    icon: "mdi-hiking",
    desc: "Exploring nature around java island.",
    img: hikingImage
  },
  {
    name: "Reading",
    icon: "mdi-book-open-page-variant-outline",
    desc: "Reading comics, manga, etc.",
    img: readingImage
  },
  {
    name: "Running",
    icon: "mdi-run-fast",
    desc: "Maintaining health.",
    img: runningImage
  },
  {
    name: "Coding",
    icon: "mdi-code-tags",
    desc: "Exploring new technologies.",
    img: codingImage
  },
];
</script>

<template>
  <ChatAgent />
  <div class="min-h-screen bg-[#020617] text-slate-200 selection:bg-blue-500/30 font-sans relative overflow-hidden">
    <div class="absolute inset-0 overflow-hidden pointer-events-none">
      <div
        class="absolute -top-[10%] -left-[10%] w-[40%] h-[40%] bg-blue-600/20 blur-[120px] rounded-full animate-blob">
      </div>
      <div
        class="absolute top-[20%] -right-[10%] w-[35%] h-[35%] bg-indigo-600/20 blur-[120px] rounded-full animate-blob animation-delay-2000">
      </div>
      <div
        class="absolute -bottom-[10%] left-[20%] w-[30%] h-[30%] bg-purple-600/15 blur-[120px] rounded-full animate-blob animation-delay-4000">
      </div>
    </div>
    <div class="relative z-10"></div>
    <Transition name="fade">
      <div v-if="isLoaded" class="max-w-4xl mx-auto px-6 py-8 flex flex-col gap-y-8">
        <header class="text-center mb-24">
          <div class="mb-8 flex justify-center">
            <div class="relative group">
              <div
                class="absolute -inset-1 bg-gradient-to-r from-blue-600 to-indigo-600 rounded-full blur opacity-25 group-hover:opacity-50 transition duration-1000">
              </div>
              <v-img :src="profileImage" alt="Fadel's Profile" width="640" height="640" cover
                class="relative rounded-full border-2 border-slate-800 shadow-2xl"></v-img>
            </div>
          </div>
          <div
            class="inline-block px-4 py-1.5 rounded-full border border-slate-700 bg-slate-800/50 text-blue-400 text-sm font-medium mb-6 animate-pulse">
            Ready for Collaboration
          </div>

          <h1
            class="text-6xl font-black mb-6 tracking-tighter bg-gradient-to-r from-white via-slate-300 to-slate-500 bg-clip-text text-transparent">
            {{ name }}
          </h1>
          <h2 class="text-2xl text-blue-400 font-mono h-8">{{ typedRole }}<span class="animate-blink">|</span></h2>

          <p class="text-xl text-slate-400 mb-10 max-w-xl mx-auto leading-relaxed">
            Quick Learner, Problem Solver, High Curiosity
          </p>

          <div class="flex flex-wrap justify-center items-center gap-3 mb-8">
            <v-btn prepend-icon="mdi-whatsapp" @click="contactWA" color="green-darken-1" class="shadow-2xl px-6">
              Let's Talk
            </v-btn>

            <v-btn icon="mdi-github" @click="openLink('https://github.com/FadelAnfasha')" variant="tonal"
              color="white"></v-btn>

            <v-btn icon="mdi-linkedin" @click="openLink('https://linkedin.com/in/fadelanfashap')" variant="tonal"
              color="blue"></v-btn>

            <v-btn icon="mdi-instagram" @click="openLink('https://instagram.com/fadelanfasha')" variant="tonal"
              color="pink"></v-btn>
          </div>
        </header>

        <section>
          <h2 class="hidden-content text-xs uppercase tracking-[0.3em] text-slate-500 font-bold mb-8">
            Specialized In
          </h2>
          <!-- Marquee Skills -->
          <div class="hidden-content relative w-full overflow-hidden mask-gradient">
            <div class="flex gap-4 w-max animate-marquee">
              <!-- Double the skills for seamless loop -->
              <div v-for="(skill, index) in [...skills, ...skills]" :key="index" class="group p-4 bg-slate-900/50 border border-slate-800 rounded-2xl 
         flex justify-center items-center gap-3 w-48 shrink-0">

                <div class="flex items-center gap-3 py-2">
                  <v-img :src="skill.logo" width="48" height="48"></v-img>
                  <div class="text-left">
                    <p class="text-md font-bold text-slate-100 leading-none mb-1">{{ skill.name }}</p>
                    <p class="text-sm text-slate-500 leading-none">{{ skill.level }}</p>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </section>

        <section>
          <h2 class="hidden-content text-xs uppercase tracking-[0.3em] text-slate-500 font-bold mb-12">
            Career Journey
          </h2>

          <v-timeline side="end" class="customized-timeline" align="center">
            <v-timeline-item v-for="(exp, index) in experiences" :key="index"
              :dot-color="selectedCategory === exp.id ? 'blue' : 'slate-600'" size="small">
              <v-card @click="selectedCategory = exp.id" variant="outlined" :class="[
                'transition-all duration-300 rounded-xl border-slate-800',
                selectedCategory === exp.id
                  ? 'bg-blue-600/10 border-blue-500/50 shadow-[0_0_20px_rgba(37,99,235,0.1)]'
                  : 'bg-slate-900/40 hover:bg-slate-800/50'
              ]">
                <v-card-text class="p-5">
                  <div class="flex flex-col">
                    <h3 :class="[
                      'font-bold text-lg mb-1 transition-colors',
                      selectedCategory === exp.id ? 'text-blue-400' : 'text-white',
                    ]">
                      {{ exp.status }}
                    </h3>

                    <div class="flex items-center gap-2 mb-3">
                      <v-icon size="small" color="slate-500">mdi-calendar-range</v-icon>
                      <small class="text-slate-500 font-medium uppercase tracking-wider">
                        {{ exp.date }}
                      </small>
                    </div>

                    <p class="text-slate-400 text-sm leading-relaxed">
                      {{ exp.desc }}
                    </p>
                  </div>
                </v-card-text>
              </v-card>
            </v-timeline-item>
          </v-timeline>
        </section>

        <section>
          <div class="hidden-content">
            <div class="flex items-center justify-between mb-8">
              <h2 class="text-xs uppercase tracking-[0.3em] text-slate-500 font-bold">
                Projects during this period
              </h2>

              <v-chip color="blue" variant="flat" class="px-4 py-1.5 rounded-lg">
                {{ selectedCompanyName }}
              </v-chip>
            </div>

            <!-- Projects Slide Group -->
            <v-slide-group :key="selectedCategory" show-arrows center-active class="projects-carousel">
              <v-slide-group-item v-for="(project, index) in filteredProjects" :key="index"
                v-slot="{ isSelected, toggle, selectedClass }">
                <div class="p-3">
                  <v-card @click="toggle" variant="outlined" width="360" height="280" :class="[
                    'flex flex-col rounded-3xl bg-slate-900/40 transition-all duration-300 shadow-xl cursor-pointer',
                    isSelected
                      ? 'border-blue-500 ring-1 ring-blue-500 scale-105 bg-slate-800/80 z-10'
                      : 'border-slate-800 hover:bg-slate-800/60 hover:-translate-y-2'
                  ]">
                    <v-card-text class="p-6 flex flex-col h-full">
                      <div class="flex justify-between items-start mb-4">
                        <h4 :class="[
                          'text-xl font-bold leading-tight transition-colors',
                          isSelected ? 'text-blue-400' : 'text-white'
                        ]">
                          {{ project.title }}
                        </h4>
                        <v-icon :color="isSelected ? 'blue' : 'blue-lighten-3'" :size="isSelected ? 'default' : 'small'"
                          class="transition-all">
                          mdi-rocket-launch-outline
                        </v-icon>
                      </div>

                      <p class="text-slate-400 text-sm mb-6 leading-relaxed flex-grow line-clamp-3">
                        {{ project.desc }}
                      </p>

                      <div class="flex gap-2 flex-wrap mt-auto">
                        <v-chip v-for="tag in project.tags" :key="tag" size="small"
                          class="p-2 font-medium text-slate-300" variant="tonal" :prepend-avatar="getSkillLogo(tag)">
                          {{ tag }}
                        </v-chip>

                      </div>
                    </v-card-text>
                  </v-card>
                </div>
              </v-slide-group-item>
            </v-slide-group>

          </div>
        </section>

        <!-- Hobbies Section -->
        <section>
          <h2 class="hidden-content text-xs uppercase tracking-[0.3em] text-slate-500 font-bold mb-12">
            Hobbies & Interests
          </h2>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <v-hover v-for="(hobby, index) in hobbies" :key="index" v-slot="{ isHovering, props }">
              <v-card v-bind="props" :elevation="isHovering ? 12 : 2"
                :class="['transition-all duration-300 rounded-2xl overflow-hidden cursor-default', isHovering ? '-translate-y-2' : '']"
                height="250">
                <v-img :src="hobby.img" :alt="hobby.name" cover class="align-end h-full text-white"
                  gradient="to bottom, rgba(2,6,23,0) 0%, rgba(2,6,23,0.8) 50%, rgba(2,6,23,0.95) 100%">
                  <template v-slot:placeholder>
                    <div class="d-flex align-center justify-center fill-height">
                      <v-progress-circular indeterminate color="blue-lighten-3"></v-progress-circular>
                    </div>
                  </template>

                  <v-card-text class="flex flex-col h-full justify-end p-6 relative z-10">
                    <div class="transform transition-all duration-500"
                      :class="isHovering ? 'translate-y-0' : 'translate-y-4'">
                      <v-icon :icon="hobby.icon" size="x-large" color="blue-400"
                        class="mb-3 transition-transform duration-300" :class="isHovering ? 'scale-110' : ''"></v-icon>
                      <h3 class="text-2xl font-bold mb-1">{{ hobby.name }}</h3>

                      <div class="overflow-hidden transition-all duration-500 ease-in-out"
                        :style="{ maxHeight: isHovering ? '100px' : '0px', opacity: isHovering ? 1 : 0 }">
                        <p class="text-slate-300 text-sm mt-2 leading-relaxed">
                          {{ hobby.desc }}
                        </p>
                      </div>
                    </div>
                  </v-card-text>
                </v-img>
              </v-card>
            </v-hover>
          </div>
        </section>
      </div>
    </Transition>

    <footer class="py-10 text-center border-t border-slate-900 mt-20">
      <p class="text-slate-600 text-sm">© 2026 Fadel</p>
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

.projects-carousel :deep(.v-slide-group__content) {
  justify-content: center !important;
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



/* Animations */
@keyframes blink {
  50% {
    opacity: 0;
  }
}

.animate-blink {
  animation: blink 1s step-end infinite;
}

@keyframes marquee {
  0% {
    transform: translateX(0);
  }

  100% {
    transform: translateX(-50%);
  }
}

.animate-marquee {
  animation: marquee 30s linear infinite;
}

.mask-gradient {
  mask-image: linear-gradient(to right, transparent, black 10%, black 90%, transparent);
  -webkit-mask-image: linear-gradient(to right, transparent, black 10%, black 90%, transparent);
}

/* Scroll Reveal */
.hidden-content {
  opacity: 0;
  transform: translateY(30px);
  transition: all 1s ease-out;
}

.show-content {
  opacity: 1;
  transform: translateY(0);
}
</style>