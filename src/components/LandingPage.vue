<template>
  <div class="landing-page">
    <!-- Right Side Navigation Menu -->
    <nav class="side-nav" ref="sideNav">
      <ul class="nav-list">
        <li 
          v-for="(section, index) in sections" 
          :key="section.id"
          :class="['nav-item', { active: activeSection === section.id }]"
        >
          <a 
            :href="`#${section.id}`" 
            @click.prevent="scrollToSection(section.id)"
            class="nav-link"
          >
            <span class="nav-dot"></span>
            <span class="nav-label">{{ section.label }}</span>
          </a>
        </li>
      </ul>
    </nav>

    <!-- UNIR - Masterclass Section -->
    <section id="masterclass" class="section masterclass-section" ref="masterclassSection">
      <div class="section-content">
        <div class="text-content">
          <h1 class="main-title">UNIR - Masterclass</h1>
          <p class="subtitle">How to survive kit</p>
        </div>
      </div>
      <div class="image-content">
        <img 
          :src="characterImage" 
          alt="3D Character" 
          class="character-image"
        />
      </div>
      <button 
        class="scroll-arrow-btn" 
        @click="scrollToNextSection"
        aria-label="Scroll to next section"
      >
        <svg class="arrow-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M6 9l6 6 6-6"/>
        </svg>
      </button>
    </section>

    <!-- Who I am Section -->
    <section id="who-i-am" class="section who-i-am-section" ref="whoIAmSection">
      <div class="section-content">
        <div class="text-content">
          <h2 class="section-title">Who I am</h2>
          <p class="section-subtitle">The participants of this hackathon</p>
        </div>
        <div class="profile-content">
          <div class="profile-image-wrapper">
            <img 
              :src="profileImage" 
              alt="Alberto González" 
              class="profile-image"
            />
          </div>
          <div class="profile-info">
            <h3 class="profile-name">Alberto González</h3>
            <p class="profile-title">Full Stack Engineer</p>
            <div class="profile-social">
              <a href="https://linkedin.com/in/albegosu" target="_blank" class="social-link">
                <span class="social-handle">/albegosu/</span>
                <svg class="linkedin-icon" viewBox="0 0 24 24" fill="currentColor">
                  <path d="M19 0h-14c-2.761 0-5 2.239-5 5v14c0 2.761 2.239 5 5 5h14c2.762 0 5-2.239 5-5v-14c0-2.761-2.238-5-5-5zm-11 19h-3v-11h3v11zm-1.5-12.268c-.966 0-1.75-.79-1.75-1.764s.784-1.764 1.75-1.764 1.75.79 1.75 1.764-.783 1.764-1.75 1.764zm13.5 12.268h-3v-5.604c0-3.368-4-3.113-4 0v5.604h-3v-11h3v1.765c1.396-2.586 7-2.777 7 2.476v6.759z"/>
                </svg>
              </a>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import characterImage from '../assets/Gon_5_Standing_Outline0003.png'
import profileImage from '../assets/Alberto González 1.png'

const sections = ref([
  { id: 'masterclass', label: 'Masterclass' },
  { id: 'who-i-am', label: 'Who I am' }
])

const activeSection = ref('masterclass')
const masterclassSection = ref(null)
const whoIAmSection = ref(null)
const sideNav = ref(null)

const scrollToSection = (sectionId) => {
  const section = document.getElementById(sectionId)
  if (section) {
    section.scrollIntoView({ behavior: 'smooth', block: 'start' })
    activeSection.value = sectionId
  }
}

const scrollToNextSection = () => {
  const currentIndex = sections.value.findIndex(s => s.id === activeSection.value)
  const nextIndex = currentIndex + 1
  
  if (nextIndex < sections.value.length) {
    scrollToSection(sections.value[nextIndex].id)
  }
}

const handleScroll = () => {
  const scrollPosition = window.scrollY + window.innerHeight / 2

  sections.value.forEach((section) => {
    const element = document.getElementById(section.id)
    if (element) {
      const top = element.offsetTop
      const bottom = top + element.offsetHeight

      if (scrollPosition >= top && scrollPosition < bottom) {
        activeSection.value = section.id
      }
    }
  })
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
  handleScroll() // Initial check
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<style scoped>
.landing-page {
  min-height: 100vh;
  background: whitesmoke;
  position: relative;
  font-family: 'Switzer', sans-serif;
}

.section {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 80px 40px;
  position: relative;
}

.section-content {
  max-width: 1400px;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 60px;
}

/* Masterclass Section */
.masterclass-section {
  background: whitesmoke;
  position: relative;
}

.masterclass-section .section-content {
  align-items: flex-start;
  justify-content: flex-start;
}

.text-content {
  flex: 1;
  z-index: 2;
}

.main-title {
  font-size: 4rem;
  font-weight: 700;
  color: #1a1a2e;
  margin-bottom: 1rem;
  line-height: 1.2;
  letter-spacing: -0.02em;
}

.subtitle {
  font-size: 1.5rem;
  font-weight: 400;
  color: #666;
  margin-top: 0.5rem;
}

.masterclass-section .image-content {
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  pointer-events: none;
  z-index: 1;
}

.character-image {
  width: auto;
  height: 90vh;
  max-width: none;
  object-fit: contain;
  object-position: bottom center;
  filter: grayscale(100%);
}

.scroll-arrow-btn {
  position: absolute;
  bottom: 30px;
  left: 50%;
  transform: translateX(-50%);
  background: transparent;
  border: 2px solid #000;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
  z-index: 10;
  padding: 0;
}

.scroll-arrow-btn:hover {
  background: #000;
  transform: translateX(-50%) translateY(-5px);
}

.scroll-arrow-btn:hover .arrow-icon {
  stroke: #fff;
}

.arrow-icon {
  width: 24px;
  height: 24px;
  stroke: #000;
  transition: stroke 0.3s ease;
}

/* Who I am Section */
.who-i-am-section {
  background: whitesmoke;
}

.section-title {
  font-size: 3rem;
  font-weight: 700;
  color: #1a1a2e;
  margin-bottom: 0.5rem;
  line-height: 1.2;
}

.section-subtitle {
  font-size: 1.2rem;
  font-weight: 400;
  color: #666;
  margin-bottom: 3rem;
}

.profile-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
  width: 100%;
}

.profile-image-wrapper {
  width: 280px;
  height: 280px;
  border-radius: 50%;
  overflow: hidden;
  border: 3px solid #1a1a2e;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
}

.profile-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  filter: grayscale(100%);
}

.profile-info {
  text-align: center;
}

.profile-name {
  font-size: 2rem;
  font-weight: 700;
  color: #1a1a2e;
  margin-bottom: 0.5rem;
}

.profile-title {
  font-size: 1.2rem;
  font-weight: 400;
  color: #666;
  margin-bottom: 1.5rem;
}

.profile-social {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

.social-link {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  text-decoration: none;
  color: #1a1a2e;
  font-size: 1rem;
  transition: opacity 0.3s ease;
}

.social-link:hover {
  opacity: 0.7;
}

.social-handle {
  font-weight: 500;
}

.linkedin-icon {
  width: 20px;
  height: 20px;
  fill: #0077b5;
}

/* Right Side Navigation */
.side-nav {
  position: fixed;
  right: 40px;
  top: 50%;
  transform: translateY(-50%);
  z-index: 1000;
}

.nav-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.nav-item {
  position: relative;
}

.nav-link {
  display: flex;
  align-items: center;
  gap: 12px;
  text-decoration: none;
  color: #d3d3d3;
  transition: all 0.3s ease;
  cursor: pointer;
}

.nav-link:focus {
  outline: none;
}

.nav-dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background: #d3d3d3;
  border: 2px solid whitesmoke;
  transition: all 0.3s ease;
  flex-shrink: 0;
}

.nav-label {
  font-size: 0.9rem;
  font-weight: 500;
  opacity: 0;
  transform: translateX(-10px);
  transition: all 0.3s ease;
  white-space: nowrap;
  color: inherit;
}

.nav-item:hover .nav-label,
.nav-item.active .nav-label,
.nav-item:focus-within .nav-label {
  opacity: 1;
  transform: translateX(0);
}

.nav-item:hover .nav-link,
.nav-item.active .nav-link,
.nav-item:focus-within .nav-link {
  color: #000;
}

.nav-item:hover .nav-dot,
.nav-item.active .nav-dot,
.nav-item:focus-within .nav-dot {
  background: #000;
  transform: scale(1.2);
  border-color: #000;
}

.nav-item.active .nav-dot {
  background: #000;
  border-color: #000;
}

/* Responsive Design */
@media (max-width: 1024px) {
  .section-content {
    flex-direction: column;
    text-align: center;
  }

  .main-title {
    font-size: 3rem;
  }

  .character-image {
    height: 70vh;
  }

  .side-nav {
    right: 20px;
  }
}

@media (max-width: 768px) {
  .section {
    padding: 60px 20px;
  }

  .main-title {
    font-size: 2.5rem;
  }

  .section-title {
    font-size: 2rem;
  }

  .subtitle {
    font-size: 1.2rem;
  }

  .character-image {
    height: 60vh;
  }

  .profile-image-wrapper {
    width: 220px;
    height: 220px;
  }

  .profile-name {
    font-size: 1.5rem;
  }

  .side-nav {
    right: 15px;
  }

  .nav-label {
    display: none;
  }
}

@media (max-width: 480px) {
  .main-title {
    font-size: 2rem;
  }

  .section-title {
    font-size: 1.75rem;
  }
}
</style>
