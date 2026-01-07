<template>
  <div class="landing-page">
    <!-- Language Toggle Button -->
    <Transition name="fade">
      <button 
        v-if="activeSection === 'masterclass'"
        class="language-toggle" 
        @click="toggleLanguage"
        :aria-label="currentLanguage === 'en' ? 'Cambiar a español' : 'Switch to English'"
        title="Toggle language"
      >
        <span class="language-code">{{ currentLanguage.toUpperCase() }}</span>
      </button>
    </Transition>

    <!-- Chronometer -->
    <div class="chronometer" v-if="chronometerRunning || elapsedTime > 0">
      <div class="chronometer-display">
        {{ formatTime(elapsedTime) }}
      </div>
    </div>

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
          <h1 class="main-title">UNIR - <span class="light-text">Masterclass</span></h1>
          <p class="subtitle">{{ t.masterclass.subtitle }}</p>
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
        v-if="hasNextSection('masterclass')"
        class="scroll-arrow-btn" 
        @click="startChronometer"
        :aria-label="t.aria.scrollNext"
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
          <h2 class="section-title">{{ t.whoIAm.title }}</h2>
          <p class="section-subtitle">{{ t.whoIAm.subtitle }}</p>
        </div>
        <div class="profile-content">
          <div class="profile-image-wrapper">
            <img 
              :src="profileImage" 
              alt="Alberto González" 
              class="profile-image"
            />
            <div class="profile-hover-overlay">
              <svg class="arrows-svg" width="100%" height="100%" style="position: absolute; top: 0; left: 0; overflow: visible; pointer-events: none;">
                <defs>
                  <marker id="arrowhead-profile" markerWidth="12" markerHeight="12" refX="11" refY="4" orient="auto" markerUnits="strokeWidth">
                    <path d="M 0,2 Q 2,4 4,4 Q 6,4 8,3 Q 10,2 12,4" stroke="#1a1a2e" stroke-width="1.5" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
                  </marker>
                  <filter id="rough-paper" x="-20%" y="-20%" width="140%" height="140%">
                    <feTurbulence type="fractalNoise" baseFrequency="0.9" numOctaves="3" result="noise"/>
                    <feDisplacementMap in="SourceGraphic" in2="noise" scale="1" />
                  </filter>
                </defs>
              </svg>
              <div class="hover-item item-1">
                <svg class="arrow-line" viewBox="0 0 200 50" preserveAspectRatio="none">
                  <path d="M 0,23 Q 15,20 30,22 Q 45,24 60,23 Q 75,22 90,24 Q 105,26 120,24 Q 135,22 150,23 Q 165,24 180,22 Q 195,20 200,25" 
                        stroke="#1a1a2e" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" marker-end="url(#arrowhead-profile)" opacity="0.9"/>
                </svg>
                <div class="hover-label">
                  <div class="label-line-1">{{ currentLanguage === 'en' ? t.whoIAm.profileHover.industrialDesign : 'Diseño Industrial' }}</div>
                  <div class="label-line-2">ESAPA</div>
                </div>
              </div>
              <div class="hover-item item-2">
                <svg class="arrow-line" viewBox="0 0 200 50" preserveAspectRatio="none">
                  <path d="M 0,25 Q 18,23 35,25 Q 52,27 70,25 Q 88,23 105,24 Q 122,25 140,26 Q 158,27 175,25 Q 192,23 200,25" 
                        stroke="#1a1a2e" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" marker-end="url(#arrowhead-profile)" opacity="0.9"/>
                </svg>
                <div class="hover-label">
                  <div class="label-line-1">{{ currentLanguage === 'en' ? t.whoIAm.profileHover.masterFullStack : 'Máster Full Stack' }}</div>
                  <div class="label-line-2">UNIR</div>
                </div>
              </div>
              <div class="hover-item item-3">
                <svg class="arrow-line" viewBox="0 0 200 50" preserveAspectRatio="none">
                  <path d="M 0,27 Q 20,24 40,26 Q 60,28 80,27 Q 100,26 120,25 Q 140,24 160,26 Q 180,28 200,25" 
                        stroke="#1a1a2e" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" marker-end="url(#arrowhead-profile)" opacity="0.9"/>
                </svg>
                <div class="hover-label">
                  <div class="label-line-1">{{ currentLanguage === 'en' ? t.whoIAm.profileHover.bootcampFullStack : 'Bootcamp Full Stack' }}</div>
                  <div class="label-line-2">Factoría F5</div>
                </div>
              </div>
              <div class="hover-item item-4">
                <svg class="arrow-line" viewBox="0 0 200 50" preserveAspectRatio="none">
                  <path d="M 0,24 Q 16,26 32,24 Q 48,22 64,23 Q 80,24 96,25 Q 112,26 128,25 Q 144,24 160,23 Q 176,22 192,24 Q 196,24 200,25" 
                        stroke="#1a1a2e" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" marker-end="url(#arrowhead-profile)" opacity="0.9"/>
                </svg>
                <div class="hover-label">
                  <div class="label-line-1">{{ currentLanguage === 'en' ? t.whoIAm.profileHover.trialAndError : 'Prueba y error' }}</div>
                  <div class="label-line-2">La vida</div>
                </div>
              </div>
            </div>
          </div>
          <div class="profile-info">
            <h3 class="profile-name">Alberto González</h3>
            <p class="profile-title">{{ t.whoIAm.profileTitle }}</p>
            <div class="profile-social">
              <a href="https://www.linkedin.com/in/albegosu/" target="_blank" class="social-link">
                <span class="social-handle">/albegosu</span>
                <svg class="linkedin-icon" viewBox="0 0 24 24" fill="currentColor">
                  <path d="M19 0h-14c-2.761 0-5 2.239-5 5v14c0 2.761 2.239 5 5 5h14c2.762 0 5-2.239 5-5v-14c0-2.761-2.238-5-5-5zm-11 19h-3v-11h3v11zm-1.5-12.268c-.966 0-1.75-.79-1.75-1.764s.784-1.764 1.75-1.764 1.75.79 1.75 1.764-.783 1.764-1.75 1.764zm13.5 12.268h-3v-5.604c0-3.368-4-3.113-4 0v5.604h-3v-11h3v1.765c1.396-2.586 7-2.777 7 2.476v6.759z"/>
                </svg>
              </a>
            </div>
          </div>
        </div>
      </div>
      <button 
        v-if="hasNextSection('who-i-am')"
        class="scroll-arrow-btn" 
        @click="scrollToNextSection"
        :aria-label="t.aria.scrollNext"
      >
        <svg class="arrow-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M6 9l6 6 6-6"/>
        </svg>
      </button>
    </section>

    <!-- Where I've worked Section -->
    <section id="where-worked" class="section content-section">
      <div class="ellipse-bg" aria-hidden="true">
        <img :src="ellipseImage" alt="" class="ellipse-img" />
      </div>
      <div class="section-content">
          <div class="where-worked-header">
            <h2 class="section-title">{{ t.whereWorked.title }}</h2>
            <p class="section-subtitle">{{ t.whereWorked.subtitle }}</p>
          </div>
        <div class="text-content">
          <div class="timeline-container-full">
            <div class="horizontal-timeline">
              <div class="timeline-line"></div>
              <div 
                v-for="(job, index) in workExperience" 
                :key="index"
                :class="['timeline-item', index % 2 === 0 ? 'timeline-item-top' : 'timeline-item-bottom']"
              >
                <div class="timeline-marker-wrapper">
                  <div :class="['timeline-marker', index % 2 === 0 ? 'marker-top' : 'marker-bottom']"></div>
                  <div class="timeline-year">{{ job.year }}</div>
                  <div class="timeline-connector"></div>
                </div>
                <div class="timeline-content">
                  <div class="job-position">{{ job.position }}</div>
                  <div class="job-company">{{ job.company }}</div>
                  <div class="job-location">{{ job.location }}</div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <button 
        v-if="hasNextSection('where-worked')"
        class="scroll-arrow-btn" 
        @click="scrollToNextSection"
        :aria-label="t.aria.scrollNext"
      >
        <svg class="arrow-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M6 9l6 6 6-6"/>
        </svg>
      </button>
    </section>

    <!-- Why I studied UNIR Master Section -->
    <section id="why-studied" class="section content-section">
      <div class="ellipse-bg" aria-hidden="true">
        <img :src="ellipseImage" alt="" class="ellipse-img" />
      </div>
      <div class="section-content">
        <div class="text-content">
          <h2 class="section-title">{{ t.whyStudied.title }}</h2>
          <p class="section-subtitle">{{ t.whyStudied.subtitle }}</p>
          <ul class="why-studied-list">
            <li class="why-studied-item">
              <h3 class="why-studied-point-title">{{ t.whyStudied.stack.title }}</h3>
              <p class="why-studied-point-text">{{ t.whyStudied.stack.text }}</p>
            </li>
            <li class="why-studied-item">
              <h3 class="why-studied-point-title">{{ t.whyStudied.linkedIn.title }}</h3>
              <p class="why-studied-point-text">{{ t.whyStudied.linkedIn.text }}</p>
            </li>
          </ul>
        </div>
      </div>
      <button 
        v-if="hasNextSection('why-studied')"
        class="scroll-arrow-btn" 
        @click="scrollToNextSection"
        :aria-label="t.aria.scrollNext"
      >
        <svg class="arrow-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M6 9l6 6 6-6"/>
        </svg>
      </button>
    </section>

    <!-- When I studied UNIR Master Section -->
    <section id="when-studied" class="section when-studied-section">
      <div class="image-content-top">
        <img 
          :src="platonicImage" 
          alt="Platonic Dodecahedron" 
          class="platonic-image"
        />
      </div>
      <div class="section-content">
        <div class="text-content">
          <h2 class="section-title">{{ t.whenStudied.title }}</h2>
          <p class="section-subtitle">{{ t.whenStudied.subtitle }}</p>
          <div class="steps-container">
            <div class="steps-line"></div>
            <div class="steps-wrapper">
              <div class="step-item">
                <div class="step-marker">
                  <div class="step-number">1</div>
                </div>
                <div class="step-content">
                  <h3 class="step-title">{{ t.whenStudied.faculty }}</h3>
                </div>
              </div>
              <div class="step-item">
                <div class="step-marker">
                  <div class="step-number">2</div>
                </div>
                <div class="step-content">
                  <h3 class="step-title">{{ t.whenStudied.content }}</h3>
                </div>
              </div>
              <div class="step-item">
                <div class="step-marker">
                  <div class="step-number">3</div>
                </div>
                <div class="step-content">
                  <h3 class="step-title">{{ t.whenStudied.support }}</h3>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <button 
        v-if="hasNextSection('when-studied')"
        class="scroll-arrow-btn" 
        @click="scrollToNextSection"
        :aria-label="t.aria.scrollNext"
      >
        <svg class="arrow-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M6 9l6 6 6-6"/>
        </svg>
      </button>
    </section>

    <!-- Master vs. Bootcamp Section -->
    <section id="master-vs-bootcamp" class="section content-section">
      <div class="ellipse-bg" aria-hidden="true">
        <img :src="ellipseImage" alt="" class="ellipse-img" />
      </div>
      <div class="section-content">
        <div class="text-content">
          <h2 class="section-title">{{ t.masterVsBootcamp.title }}</h2>
          <p class="section-subtitle">{{ t.masterVsBootcamp.subtitle }}</p>
          <div class="comparison-container">
            <div class="comparison-column">
              <h3 class="comparison-title">{{ t.masterVsBootcamp.master.title }}</h3>
              <ul class="comparison-list">
                <li class="comparison-item">
                  <h4 class="comparison-point-title">{{ t.masterVsBootcamp.master.contentStructure.title }}</h4>
                  <p class="comparison-point-text">{{ t.masterVsBootcamp.master.contentStructure.text }}</p>
                </li>
                <li class="comparison-item">
                  <h4 class="comparison-point-title">{{ t.masterVsBootcamp.master.formality.title }}</h4>
                  <p class="comparison-point-text">{{ t.masterVsBootcamp.master.formality.text }}</p>
                </li>
              </ul>
            </div>
            <div class="comparison-column">
              <h3 class="comparison-title">{{ t.masterVsBootcamp.bootcamp.title }}</h3>
              <ul class="comparison-list">
                <li class="comparison-item">
                  <h4 class="comparison-point-title">{{ t.masterVsBootcamp.bootcamp.collaboration.title }}</h4>
                  <p class="comparison-point-text">{{ t.masterVsBootcamp.bootcamp.collaboration.text }}</p>
                </li>
                <li class="comparison-item">
                  <h4 class="comparison-point-title">{{ t.masterVsBootcamp.bootcamp.github.title }}</h4>
                  <p class="comparison-point-text">{{ t.masterVsBootcamp.bootcamp.github.text }}</p>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
      <button 
        v-if="hasNextSection('master-vs-bootcamp')"
        class="scroll-arrow-btn" 
        @click="scrollToNextSection"
        :aria-label="t.aria.scrollNext"
      >
        <svg class="arrow-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M6 9l6 6 6-6"/>
        </svg>
      </button>
    </section>

    <!-- First job Section -->
    <section id="first-job" class="section content-section first-job-section">
      <div class="ellipse-bg" aria-hidden="true">
        <img :src="ellipseImage" alt="" class="ellipse-img" />
      </div>
      <div class="section-content">
        <div class="text-content">
          <h2 class="section-title">{{ t.firstJob.title }}</h2>
          <p class="section-subtitle">{{ t.firstJob.subtitle }}</p>
          <ul class="why-studied-list">
            <li class="why-studied-item">
              <h3 class="why-studied-point-title">{{ t.firstJob.doors.title }}</h3>
              <p class="why-studied-point-text">{{ t.firstJob.doors.text }}</p>
            </li>
            <li class="why-studied-item">
              <h3 class="why-studied-point-title">{{ t.firstJob.learning.title }}</h3>
              <p class="why-studied-point-text">{{ t.firstJob.learning.text }}</p>
            </li>
            <li class="why-studied-item minsait-logo-item">
              <img
                :src="minsaitLogo"
                alt="Minsait"
                class="minsait-logo"
              />
            </li>
          </ul>
        </div>
        <div class="first-job-image-content">
          <img 
            :src="rectangle28" 
            alt="" 
            class="rectangle-28"
          />
        </div>
      </div>
      <button 
        v-if="hasNextSection('first-job')"
        class="scroll-arrow-btn" 
        @click="scrollToNextSection"
        :aria-label="t.aria.scrollNext"
      >
        <svg class="arrow-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M6 9l6 6 6-6"/>
        </svg>
      </button>
    </section>

    <!-- Soft Skills Section -->
    <section id="soft-skills" class="section content-section">
      <div class="ellipse-bg" aria-hidden="true">
        <img :src="ellipseImage" alt="" class="ellipse-img" />
      </div>
      <div class="section-content">
        <div class="text-content">
          <h2 class="section-title">{{ t.softSkills.title }}</h2>
          <p class="section-subtitle">{{ t.softSkills.subtitle }}</p>
          <ul class="why-studied-list">
            <li class="why-studied-item">
              <h3 class="why-studied-point-title">{{ t.softSkills.communication.title }}</h3>
              <p class="why-studied-point-text">{{ t.softSkills.communication.text }}</p>
            </li>
            <li class="why-studied-item">
              <h3 class="why-studied-point-title">{{ t.softSkills.adaptability.title }}</h3>
              <p class="why-studied-point-text">{{ t.softSkills.adaptability.text }}</p>
            </li>
            <li class="why-studied-item">
              <h3 class="why-studied-point-title">{{ t.softSkills.problemSolving.title }}</h3>
              <p class="why-studied-point-text">{{ t.softSkills.problemSolving.text }}</p>
            </li>
            <li class="why-studied-item">
              <h3 class="why-studied-point-title">{{ t.softSkills.teamwork.title }}</h3>
              <p class="why-studied-point-text">{{ t.softSkills.teamwork.text }}</p>
            </li>
          </ul>
        </div>
      </div>
      <button 
        v-if="hasNextSection('soft-skills')"
        class="scroll-arrow-btn" 
        @click="scrollToNextSection"
        :aria-label="t.aria.scrollNext"
      >
        <svg class="arrow-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M6 9l6 6 6-6"/>
        </svg>
      </button>
    </section>

    <!-- Resizes Section -->
    <section id="resizes" class="section resizes-section">
      <div class="section-content">
        <div class="text-content resizes-center-content">
          <div class="resizes-logo-wrapper" :class="{ 'dropdown-active': resizesDropdownOpen }" ref="resizesDropdownWrapper">
            <button 
              @click="toggleResizesDropdown"
              class="resizes-logo-link"
              type="button"
              :aria-expanded="resizesDropdownOpen"
              aria-label="Toggle Resizes menu"
            >
              <img 
                :src="resizesLogo" 
                alt="Resizes" 
                class="resizes-logo"
              />
            </button>
            <div 
              class="resizes-dropdown"
              v-show="resizesDropdownOpen"
            >
              <a 
                href="https://docs.resiz.es/dash-docs/intro/overview" 
                target="_blank" 
                rel="noopener noreferrer"
                class="dropdown-item dropdown-item-1"
              >
                <span class="dropdown-icon">📚</span>
                <span class="dropdown-text">Docs</span>
              </a>
              <a 
                href="https://github.com/resizes" 
                target="_blank" 
                rel="noopener noreferrer"
                class="dropdown-item dropdown-item-2"
              >
                <span class="dropdown-icon">🐙</span>
                <span class="dropdown-text">GitHub</span>
              </a>
              <a 
                href="https://www.linkedin.com/company/resizes" 
                target="_blank" 
                rel="noopener noreferrer"
                class="dropdown-item dropdown-item-3"
              >
                <span class="dropdown-icon">💼</span>
                <span class="dropdown-text">LinkedIn</span>
              </a>
            </div>
          </div>
          <p class="section-subtitle">{{ t.resizes.subtitle }}</p>
        </div>
      </div>
      <div class="dash-container">
        <a 
          href="https://dash.resiz.es/" 
          target="_blank" 
          rel="noopener noreferrer"
          class="dash-logo-link"
        >
          <img 
            :src="dashLogo" 
            alt="Dash" 
            class="dash-logo"
          />
        </a>
      </div>
      <Transition name="toast-slide">
        <a
          v-if="resizesNotificationVisible"
          href="mailto:hello@resiz.es"
          class="notification-content"
          aria-label="Contact Resizes"
        >
          <img
            :src="notificationImage"
            alt="Contact Resizes"
            class="notification-image"
          />
        </a>
      </Transition>
      <div class="image-content resizes-tube-image">
        <img 
          :src="tubeImage" 
          alt="3D Character" 
          class="character-image"
        />
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, watch, computed } from 'vue'
import characterImage from '../assets/Gon_5_Standing_Outline0003.png'
import profileImage from '../assets/Alberto González 1.png'
import resizesLogo from '../assets/Resizes.png'
import dashLogo from '../assets/dash.png'
import rectangle28 from '../assets/Rectangle 28.png'
import minsaitLogo from '../assets/minsait-logo-dark.svg'
import tubeImage from '../assets/Tube_Medium_Standing.png'
import platonicImage from '../assets/Platonic_2_-_Dodeca.png'
import notificationImage from '../assets/Notification.png'
import ellipseImage from '../assets/Ellipse.png'

// Language state
const currentLanguage = ref('en')

// Translations
const translations = {
  en: {
    masterclass: {
      subtitle: 'How to survive Kit'
    },
    whoIAm: {
      title: 'Who I am',
      subtitle: 'From Industrial Design to Web Development',
      profileTitle: 'Full Stack Engineer',
      profileHover: {
        industrialDesign: 'Industrial Design',
        masterFullStack: 'Master Full Stack',
        bootcampFullStack: 'Bootcamp Full Stack',
        trialAndError: 'Trial and Error'
      }
    },
    whereWorked: {
      title: 'Where I\'ve worked',
      subtitle: 'My professional journey'
    },
    whyStudied: {
      title: 'Why I Studied the UNIR Master',
      subtitle: 'The motivation behind my choice',
      stack: {
        title: 'Stack',
        text: 'The comprehensive technology stack covered in the program aligned perfectly with my career goals and the current industry demands.'
      },
      linkedIn: {
        title: 'LinkedIn Research of UNIR Alumni',
        text: 'Researching UNIR Master alumni on LinkedIn showed impressive career trajectories and successful transitions into tech roles, which validated the program\'s effectiveness.'
      }
    },
    whenStudied: {
      title: 'When I Studied the UNIR Master',
      subtitle: 'Keypoints of my experience',
      faculty: 'Expert Faculty',
      content: 'Educational Content',
      support: 'Support'
    },
    masterVsBootcamp: {
      title: 'Master vs. Bootcamp',
      subtitle: 'A comparative perspective',
      master: {
        title: 'Master',
        contentStructure: {
          title: 'Content Structure',
          text: 'The curriculum is well-organized and follows a clear structure, making it easier to follow a logical learning path.'
        },
        formality: {
          title: 'Formality of Activities',
          text: 'Assignments and assessments follow a more formal, traditional academic approach.'
        }
      },
      bootcamp: {
        title: 'Bootcamp',
        collaboration: {
          title: 'Enhanced Collaborative Work',
          text: 'There\'s a stronger emphasis on collaborative work, with more group projects that really push teamwork and peer learning.'
        },
        github: {
          title: 'Greater GitHub Usage',
          text: 'You\'ll use GitHub much more frequently for version control, managing projects, and showcasing your work—great practice with tools you\'ll actually use on the job.'
        }
      }
    },
    firstJob: {
      title: 'My First Job',
      subtitle: 'The beginning of my new career',
      doors: {
        title: '100 Doors',
        text: 'You need to knock on 100 doors to get an opportunity. Persistence and determination are key when starting your career journey.'
      },
      learning: {
        title: 'Learning Everywhere',
        text: 'My first job at Minsait wasn\'t about development, but I believe you can learn anywhere. Every experience contributes to your growth as a professional.'
      }
    },
    softSkills: {
      title: 'Soft Skills',
      subtitle: 'Beyond technical expertise',
      communication: {
        title: 'Communication',
        text: 'Being able to clearly express your ideas, ask questions when you don\'t understand, and listen actively to others is crucial. Even if you\'re new to the field, showing that you can communicate effectively demonstrates your potential to learn and collaborate.'
      },
      adaptability: {
        title: 'Adaptability',
        text: 'The tech industry changes rapidly. Employers value candidates who can adapt to new technologies, methodologies, and challenges. Show that you\'re willing to learn and embrace change, even when you\'re starting with limited knowledge.'
      },
      problemSolving: {
        title: 'Problem-Solving Attitude',
        text: 'You don\'t need to know everything, but showing that you approach problems with curiosity and persistence is valuable. Employers appreciate candidates who don\'t give up easily and are willing to research, ask for help, and try different solutions.'
      },
      teamwork: {
        title: 'Teamwork',
        text: 'Most tech jobs involve working with others. Being collaborative, respectful, and supportive of your teammates matters more than having all the technical answers. Show that you can contribute to a team environment, even as you\'re learning.'
      }
    },
    resizes: {
      subtitle: 'My place'
    },
    nav: {
      masterclass: 'Masterclass',
      whoIAm: 'Who I am',
      whereWorked: 'Where I\'ve worked',
      whyStudied: 'Why I studied',
      whenStudied: 'When I studied',
      masterVsBootcamp: 'Master vs. Bootcamp',
      firstJob: 'My First Job',
      softSkills: 'Soft Skills',
      resizes: 'Resizes'
    },
    aria: {
      scrollNext: 'Scroll to next section'
    }
  },
  es: {
    masterclass: {
      subtitle: 'Kit de supervivencia'
    },
    whoIAm: {
      title: 'Quién soy',
      subtitle: 'Del Diseño Industrial al Desarrollo Web',
      profileTitle: 'Ingeniero Full Stack',
      profileHover: {
        industrialDesign: 'Diseño Industrial',
        masterFullStack: 'Máster Full Stack',
        bootcampFullStack: 'Bootcamp Full Stack',
        trialAndError: 'Prueba y error'
      }
    },
    whereWorked: {
      title: 'Dónde he trabajado',
      subtitle: 'Mi trayectoria profesional'
    },
    whyStudied: {
      title: 'Por qué estudié el Máster de UNIR',
      subtitle: 'La motivación detrás de mi elección',
      stack: {
        title: 'Stack',
        text: 'El stack tecnológico completo cubierto en el programa se alineó perfectamente con mis objetivos profesionales y las demandas actuales de la industria.'
      },
      linkedIn: {
        title: 'Investigación de Alumni de UNIR en LinkedIn',
        text: 'Investigar a los alumni del Máster de UNIR en LinkedIn mostró trayectorias profesionales impresionantes y transiciones exitosas a roles tecnológicos, lo que validó la efectividad del programa.'
      }
    },
    whenStudied: {
      title: 'Cuándo estudié el Máster de UNIR',
      subtitle: 'Los puntos más importantes para mi',
      faculty: 'Profesorado experto',
      content: 'Contenido educativo',
      support: 'Apoyo'
    },
    masterVsBootcamp: {
      title: 'Máster vs. Bootcamp',
      subtitle: 'Una perspectiva comparativa',
      master: {
        title: 'Máster',
        contentStructure: {
          title: 'Estructura del contenido',
          text: 'El plan de estudios está bien organizado y sigue una estructura clara, lo que facilita seguir un camino de aprendizaje lógico.'
        },
        formality: {
          title: 'Formalidad de las actividades',
          text: 'Las tareas y evaluaciones siguen un enfoque académico más formal y tradicional.'
        }
      },
      bootcamp: {
        title: 'Bootcamp',
        collaboration: {
          title: 'Trabajo colaborativo mejorado',
          text: 'Hay un mayor énfasis en el trabajo colaborativo, con más proyectos grupales que realmente impulsan el trabajo en equipo y el aprendizaje entre pares.'
        },
        github: {
          title: 'Mayor uso de GitHub',
          text: 'Usarás GitHub con mucha más frecuencia para control de versiones, gestión de proyectos y mostrar tu trabajo: una excelente práctica con herramientas que realmente usarás en el trabajo.'
        }
      }
    },
    firstJob: {
      title: 'Mi primer trabajo',
      subtitle: 'El comienzo de mi nueva carrera',
      doors: {
        title: '100 Puertas',
        text: 'Necesitas tocar 100 puertas para obtener una oportunidad. La persistencia y la determinación son clave al comenzar tu trayectoria profesional.'
      },
      learning: {
        title: 'Aprender en todas partes',
        text: 'Mi primer trabajo en Minsait no era sobre desarrollo, pero creo que puedes aprender en cualquier lugar. Cada experiencia contribuye a tu crecimiento como profesional.'
      }
    },
    softSkills: {
      title: 'Habilidades blandas',
      subtitle: 'Más allá de la experiencia técnica',
      communication: {
        title: 'Comunicación',
        text: 'Ser capaz de expresar claramente tus ideas, hacer preguntas cuando no entiendes y escuchar activamente a otros es crucial. Incluso si eres nuevo en el campo, demostrar que puedes comunicarte eficazmente demuestra tu potencial para aprender y colaborar.'
      },
      adaptability: {
        title: 'Adaptabilidad',
        text: 'La industria tecnológica cambia rápidamente. Los empleadores valoran a los candidatos que pueden adaptarse a nuevas tecnologías, metodologías y desafíos. Demuestra que estás dispuesto a aprender y abrazar el cambio, incluso cuando estás comenzando con conocimientos limitados.'
      },
      problemSolving: {
        title: 'Actitud de resolución de problemas',
        text: 'No necesitas saber todo, pero demostrar que abordas los problemas con curiosidad y persistencia es valioso. Los empleadores aprecian a los candidatos que no se rinden fácilmente y están dispuestos a investigar, pedir ayuda y probar diferentes soluciones.'
      },
      teamwork: {
        title: 'Trabajo en equipo',
        text: 'La mayoría de los trabajos tecnológicos implican trabajar con otros. Ser colaborativo, respetuoso y solidario con tus compañeros de equipo importa más que tener todas las respuestas técnicas. Demuestra que puedes contribuir a un ambiente de equipo, incluso mientras estás aprendiendo.'
      }
    },
    resizes: {
      subtitle: 'Mi lugar'
    },
    nav: {
      masterclass: 'Masterclass',
      whoIAm: 'Quién soy',
      whereWorked: 'Dónde he trabajado',
      whyStudied: 'Por qué estudié',
      whenStudied: 'Cuándo estudié',
      masterVsBootcamp: 'Máster vs. Bootcamp',
      firstJob: 'Mi primer trabajo',
      softSkills: 'Habilidades blandas',
      resizes: 'Resizes'
    },
    aria: {
      scrollNext: 'Desplazarse a la siguiente sección'
    }
  }
}

// Computed translation helper
const t = computed(() => translations[currentLanguage.value])

// Toggle language function
const toggleLanguage = () => {
  currentLanguage.value = currentLanguage.value === 'en' ? 'es' : 'en'
}

// Computed work experience with translations
const workExperience = computed(() => [
  {
    position: currentLanguage.value === 'en' ? 'Sales Advisor' : 'Asesor de ventas',
    company: 'H&M, PULL&BEAR, Stradivarius, Decathlon',
    location: currentLanguage.value === 'en' ? 'Madrid, Gijón, Avilés and surrounding areas' : 'Madrid, Gijón, Avilés y alrededores',
    year: '2012 - 2017'
  },
  {
    position: currentLanguage.value === 'en' ? 'Industrial Designer' : 'Diseñador Industrial',
    company: 'enPieza eStudio',
    location: currentLanguage.value === 'en' ? 'Madrid and surrounding areas' : 'Madrid y alrededores',
    year: '2016 - 2017'
  },
  {
    position: currentLanguage.value === 'en' ? 'Senior CAD Specialist' : 'Especialista CAD Senior',
    company: 'Align Technology',
    location: currentLanguage.value === 'en' ? 'Madrid and surrounding areas' : 'Madrid y alrededores',
    year: '2018 - 2022'
  },
  {
    position: currentLanguage.value === 'en' ? 'Production Manager' : 'Responsable de producción',
    company: 'MYL ideas',
    location: currentLanguage.value === 'en' ? 'Asturias, Principado de Asturias, Spain' : 'Asturias, Principado de Asturias, España',
    year: '2022 - 2023'
  },
  {
    position: currentLanguage.value === 'en' ? 'Consultant and Analyst' : 'Consultor y Analista',
    company: 'Minsait',
    location: currentLanguage.value === 'en' ? 'Gijón, Principado de Asturias, Spain' : 'Gijón, Principado de Asturias, España',
    year: '2024 - 2025'
  },
  {
    position: currentLanguage.value === 'en' ? 'Full Stack Engineer' : 'Ingeniero Full Stack',
    company: 'Resizes',
    location: currentLanguage.value === 'en' ? 'Gijón, Principado de Asturias, Spain' : 'Gijón, Principado de Asturias, España',
    year: '2025'
  }
])

const sections = computed(() => [
  { id: 'masterclass', label: t.value.nav.masterclass },
  { id: 'who-i-am', label: t.value.nav.whoIAm },
  { id: 'where-worked', label: t.value.nav.whereWorked },
  { id: 'why-studied', label: t.value.nav.whyStudied },
  { id: 'when-studied', label: t.value.nav.whenStudied },
  { id: 'master-vs-bootcamp', label: t.value.nav.masterVsBootcamp },
  { id: 'first-job', label: t.value.nav.firstJob },
  { id: 'soft-skills', label: t.value.nav.softSkills },
  { id: 'resizes', label: t.value.nav.resizes }
])

const activeSection = ref('masterclass')
const masterclassSection = ref(null)
const whoIAmSection = ref(null)
const sideNav = ref(null)
const resizesDropdownWrapper = ref(null)

// Chronometer state
const chronometerRunning = ref(false)
const elapsedTime = ref(0)
let chronometerInterval = null
const startTime = ref(0)

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

const startChronometer = () => {
  if (!chronometerRunning.value) {
    chronometerRunning.value = true
    startTime.value = Date.now() - elapsedTime.value
    
    chronometerInterval = setInterval(() => {
      elapsedTime.value = Date.now() - startTime.value
    }, 10) // Update every 10ms for smooth display
  }
  
  // Also scroll to next section
  scrollToNextSection()
}

const formatTime = (milliseconds) => {
  const totalSeconds = Math.floor(milliseconds / 1000)
  const minutes = Math.floor(totalSeconds / 60)
  const seconds = totalSeconds % 60
  const centiseconds = Math.floor((milliseconds % 1000) / 10)
  
  return `${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}.${String(centiseconds).padStart(2, '0')}`
}

const hasNextSection = (sectionId) => {
  const currentIndex = sections.value.findIndex(s => s.id === sectionId)
  return currentIndex >= 0 && currentIndex < sections.value.length - 1
}

// Resizes notification (toast)
const resizesNotificationVisible = ref(false)
let resizesNotificationTimeout = null

// Resizes dropdown state
const resizesDropdownOpen = ref(false)

const toggleResizesDropdown = () => {
  resizesDropdownOpen.value = !resizesDropdownOpen.value
}

const handleClickOutside = (event) => {
  if (resizesDropdownWrapper.value && !resizesDropdownWrapper.value.contains(event.target)) {
    resizesDropdownOpen.value = false
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
  document.addEventListener('click', handleClickOutside)
})

watch(activeSection, (newSection) => {
  if (newSection !== 'resizes') {
    resizesNotificationVisible.value = false
    if (resizesNotificationTimeout) {
      clearTimeout(resizesNotificationTimeout)
      resizesNotificationTimeout = null
    }
    return
  }

  // Entering resizes section: show toast after 2 seconds
  resizesNotificationVisible.value = false
  if (resizesNotificationTimeout) clearTimeout(resizesNotificationTimeout)
  resizesNotificationTimeout = setTimeout(() => {
    resizesNotificationVisible.value = true
  }, 2000)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
  document.removeEventListener('click', handleClickOutside)
  if (chronometerInterval) {
    clearInterval(chronometerInterval)
  }
  if (resizesNotificationTimeout) {
    clearTimeout(resizesNotificationTimeout)
  }
})
</script>

<style scoped>
.landing-page {
  min-height: 100vh;
  background: whitesmoke;
  position: relative;
  font-family: 'Switzer', sans-serif;
}

/* Language Toggle Button */
.language-toggle {
  position: fixed;
  top: 20px;
  right: 20px;
  z-index: 1001;
  background: rgba(255, 255, 255, 0.9);
  border: 2px solid #1a1a2e;
  border-radius: 6px;
  padding: 8px 16px;
  cursor: pointer;
  font-family: 'Switzer', sans-serif;
  font-size: 0.9rem;
  font-weight: 600;
  color: #1a1a2e;
  transition: all 0.3s ease;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
  display: flex;
  align-items: center;
  justify-content: center;
  min-width: 50px;
  animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Transition for language toggle */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}

.language-toggle:hover {
  background: #1a1a2e;
  color: whitesmoke;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.12);
}

.language-code {
  letter-spacing: 0.05em;
  user-select: none;
}

/* Chronometer */
.chronometer {
  position: fixed;
  bottom: 20px;
  left: 20px;
  z-index: 1000;
  background: rgba(255, 255, 255, 0.9);
  border-radius: 6px;
  padding: 8px 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}

.chronometer-display {
  font-family: 'Switzer', monospace;
  font-size: 1rem;
  font-weight: 600;
  color: #000;
  letter-spacing: 0.02em;
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
  z-index: 2;
}



.main-title {
  font-size: 4rem;
  font-weight: 700;
  color: #1a1a2e;
  line-height: 1.2;
  letter-spacing: -0.02em;
}

.light-text {
  font-weight: 300;
}

.subtitle {
  font-size: 1.5rem;
  font-weight: 400;
  margin-top: 0.5rem;
  background: linear-gradient(to right, #d3d3d3, #555);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  color: transparent;
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

.who-i-am-section .section-content {
  flex-direction: column;
  align-items: flex-start;
  text-align: left;
}

/* Content Sections */
.content-section {
  background: whitesmoke;
}

.content-section .section-content {
  flex-direction: column;
  align-items: flex-start;
  text-align: left;
}

/* First job Section */
.first-job-section .section-content {
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  gap: 60px;
}

.first-job-image-content {
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 2;
}

.rectangle-28 {
  height: auto;
  max-width: 400px;
  width: auto;
  filter: grayscale(100%);
  opacity: 0.8;
}

/* When I studied Section */
.when-studied-section {
  background: whitesmoke;
  position: relative;
}

.when-studied-section .section-content {
  flex-direction: column;
  align-items: center;
  text-align: center;
  margin-top: 120px;
}

.when-studied-section .text-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  width: 100%;
}

.image-content-top {
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  display: flex;
  align-items: flex-start;
  justify-content: center;
  pointer-events: none;
  z-index: 1;
}

.platonic-image {
  width: auto;
  height: 70vh;
  max-width: none;
  object-fit: contain;
  object-position: top center;
  filter: grayscale(100%);
}

/* Horizontal Steps Styles */
.steps-container {
  position: relative;
  width: 100%;
  max-width: 1000px;
  margin: 4rem auto 0;
  z-index: 2;
}

.steps-line {
  position: absolute;
  top: 30px;
  left: 0;
  right: 0;
  height: 3px;
  background: linear-gradient(to right, #1a1a2e, #d3d3d3);
  z-index: 1;
}

.steps-wrapper {
  position: relative;
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 2rem;
  z-index: 2;
}

.step-item {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
  transition: transform 0.3s ease;
}

.step-item:hover {
  transform: translateY(-5px);
}

.step-marker {
  position: relative;
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background: #1a1a2e;
  border: 4px solid whitesmoke;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4px 12px rgba(26, 26, 46, 0.15);
  transition: all 0.3s ease;
  z-index: 3;
}

.step-item:hover .step-marker {
  background: #000;
  box-shadow: 0 6px 20px rgba(26, 26, 46, 0.25);
  transform: scale(1.1);
}

.step-number {
  font-size: 1.5rem;
  font-weight: 700;
  color: whitesmoke;
  line-height: 1;
}

.step-content {
  margin-top: 1.5rem;
  text-align: center;
  max-width: 200px;
}

.step-title {
  font-size: 1.3rem;
  font-weight: 600;
  color: #1a1a2e;
  margin: 0;
  line-height: 1.4;
  transition: color 0.3s ease;
}

.step-item:hover .step-title {
  color: #000;
}

.content-body {
  margin-top: 2rem;
  font-size: 1.1rem;
  line-height: 1.8;
  color: #333;
  max-width: 800px;
}

.why-studied-list {
  margin-top: 2rem;
  list-style: none;
  padding: 0;
  max-width: 800px;
}

.why-studied-item {
  margin-bottom: 2.5rem;
}

.why-studied-item:last-child {
  margin-bottom: 0;
}

.why-studied-point-title {
  font-size: 1.5rem;
  font-weight: 700;
  color: #1a1a2e;
  margin-bottom: 0.75rem;
  line-height: 1.3;
}

.why-studied-point-text {
  font-size: 1.1rem;
  line-height: 1.8;
  color: #333;
  margin: 0;
}

.minsait-logo-item {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  margin-top: 2rem;
}

.minsait-logo {
  max-width: 200px;
  height: auto;
  filter: grayscale(100%);
}

/* Master vs. Bootcamp Comparison Styles */
.comparison-container {
  margin-top: 3rem;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
  max-width: 1200px;
}

.comparison-column {
  display: flex;
  flex-direction: column;
}

.comparison-title {
  font-size: 2rem;
  font-weight: 700;
  color: #1a1a2e;
  margin-bottom: 2rem;
  line-height: 1.2;
}

.comparison-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.comparison-item {
  margin-bottom: 2.5rem;
}

.comparison-item:last-child {
  margin-bottom: 0;
}

.comparison-point-title {
  font-size: 1.3rem;
  font-weight: 700;
  color: #1a1a2e;
  margin-bottom: 0.75rem;
  line-height: 1.3;
}

.comparison-point-text {
  font-size: 1.1rem;
  line-height: 1.8;
  color: #333;
  margin: 0;
}

/* Where I've worked Section Layout */
#where-worked {
  align-items: center;
  padding-top: 10px;
}

#where-worked .section-content {
  max-width: 80%;
  width: 100%;
}

#where-worked .text-content {
  width: 100%;
  max-width: 100%;
}

.where-worked-header {
  margin-top: -3rem;
  margin-bottom: 10rem;
  max-width: 1400px;
  width: 100%;
}

.where-worked-header .section-title {
  margin-bottom: 0.5rem;
}

.where-worked-header .section-subtitle {
  font-size: 1.2rem;
  font-weight: 400;
  color: #666;
  margin: 0;
}

/* Horizontal Timeline Styles */
.timeline-container-full {
  position: relative;
  z-index: 2;
  width: 100%;
  padding: 80px 0;
  margin-left: 0;
  margin-right: 0;
}

.horizontal-timeline {
  position: relative;
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 40px;
}

.timeline-line {
  display: none;
}

.timeline-item {
  position: relative;
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  z-index: 2;
}

.timeline-item-top {
  align-items: center;
}

.timeline-item-bottom {
  align-items: center;
}

.timeline-marker-wrapper {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
}

.timeline-marker {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #1a1a2e;
  border: 4px solid whitesmoke;
  z-index: 3;
  box-shadow: 0 0 0 2px #1a1a2e;
  position: relative;
}

.marker-top {
  transform: translateY(-12px);
}

.marker-bottom {
  transform: translateY(12px);
}

.timeline-year {
  font-size: 0.9rem;
  font-weight: 600;
  color: #1a1a2e;
  white-space: nowrap;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  z-index: 4;
}

.timeline-item-top .timeline-year {
  top: calc(50% + 24px);
}

.timeline-item-bottom .timeline-year {
  bottom: calc(50% + 24px);
}

.timeline-connector {
  position: absolute;
  width: 2px;
  background: #d3d3d3;
  z-index: 1;
  left: 50%;
  transform: translateX(-50%);
}

.timeline-item-top .timeline-connector {
  bottom: 100%;
  height: 100px;
  margin-bottom: 10px;
}

.timeline-item-bottom .timeline-connector {
  top: 100%;
  height: 100px;
  margin-top: 10px;
}

.timeline-content {
  position: absolute;
  width: 240px;
  background: rgba(255, 255, 255, 0.9);
  padding: 1.25rem;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
  transition: all 0.3s ease;
}

.timeline-item-top .timeline-content {
  bottom: 100%;
  margin-bottom: 100px;
  transform: translateX(-50%);
  left: 50%;
}

.timeline-item-bottom .timeline-content {
  top: 100%;
  margin-top: 100px;
  transform: translateX(-50%);
  left: 50%;
}

.timeline-item:hover .timeline-content {
  background: rgba(255, 255, 255, 1);
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.12);
  transform: translateX(-50%) translateY(-4px);
}

.job-position {
  font-size: 1rem;
  font-weight: 700;
  color: #1a1a2e;
  margin-bottom: 0.5rem;
  line-height: 1.3;
}

.job-company {
  font-size: 0.9rem;
  font-weight: 400;
  color: #666;
  margin-bottom: 0.4rem;
}

.job-location {
  font-size: 0.85rem;
  font-weight: 400;
  color: #666;
  margin: 0;
  line-height: 1.4;
}


.job-header {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  margin-bottom: 0.75rem;
}

.job-title {
  font-size: 1.3rem;
  font-weight: 700;
  color: #1a1a2e;
  margin: 0;
  line-height: 1.3;
}

.job-company {
  font-size: 1rem;
  font-weight: 500;
  color: #666;
}

.job-details {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-bottom: 0.5rem;
  font-size: 0.9rem;
  color: #666;
}

.job-period {
  font-weight: 500;
}

.job-duration {
  color: #999;
}

.job-location {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-bottom: 0.75rem;
  font-size: 0.85rem;
  color: #666;
}

.job-type {
  padding: 2px 8px;
  background: rgba(0, 0, 0, 0.05);
  border-radius: 4px;
  font-size: 0.85rem;
  color: #666;
}

.job-description {
  font-size: 0.95rem;
  line-height: 1.6;
  color: #444;
  margin: 0.75rem 0;
}

.job-skills {
  margin-top: 0.75rem;
  font-size: 0.85rem;
  line-height: 1.6;
  color: #666;
}

.skills-label {
  font-weight: 600;
  color: #555;
  margin-right: 8px;
}

.skills-list {
  color: #666;
}

.ellipse-bg {
  position: absolute;
  top: 0;
  right: 0;
  transform: none;
  pointer-events: none;
  z-index: 0;
}

.ellipse-img {
  height: 70vh;
  width: auto;
  opacity: 0.22;
  filter: grayscale(100%);
}

.resizes-logo-wrapper {
  position: relative;
  display: inline-block;
}

.resizes-logo-link {
  display: inline-block;
  background: none;
  border: none;
  padding: 0;
  cursor: pointer;
  text-decoration: none;
  transition: transform 0.3s ease, opacity 0.3s ease;
  position: relative;
  z-index: 3;
}

.resizes-logo-wrapper.dropdown-active .resizes-logo-link {
  transform: translateY(-2px) rotate(2deg) scale(1.05);
  opacity: 0.9;
}

.resizes-logo-link:hover {
  transform: translateY(-2px) rotate(2deg) scale(1.05);
  opacity: 0.9;
}

.resizes-logo {
  max-width: 300px;
  height: auto;
  filter: grayscale(100%);
  transition: filter 0.3s ease, transform 0.3s ease;
  display: block;
}

.resizes-logo-wrapper.dropdown-active .resizes-logo,
.resizes-logo-link:hover .resizes-logo {
  filter: grayscale(80%);
}

.resizes-dropdown {
  position: absolute;
  top: 50%;
  left: calc(100% + 20px);
  transform: translateY(-50%) translateX(-20px);
  display: flex;
  flex-direction: column;
  gap: 12px;
  opacity: 0;
  visibility: hidden;
  pointer-events: none;
  transition: opacity 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55),
              visibility 0.4s,
              transform 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55);
  z-index: 2;
  min-width: 160px;
}

.resizes-logo-wrapper.dropdown-active .resizes-dropdown {
  opacity: 1;
  visibility: visible;
  pointer-events: all;
  transform: translateY(-50%) translateX(0);
}

.dropdown-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px 20px;
  background: rgba(255, 255, 255, 0.95);
  border: 2px solid #1a1a2e;
  border-radius: 12px;
  text-decoration: none;
  color: #1a1a2e;
  font-weight: 600;
  font-size: 0.95rem;
  transition: all 0.3s cubic-bezier(0.68, -0.55, 0.265, 1.55);
  transform: translateX(-30px) rotate(-5deg);
  opacity: 0;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  position: relative;
  overflow: hidden;
}

.dropdown-item::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(26, 26, 46, 0.1), transparent);
  transition: left 0.5s ease;
}

.dropdown-item:hover::before {
  left: 100%;
}

.resizes-logo-wrapper.dropdown-active .dropdown-item-1 {
  opacity: 1;
  transform: translateX(0) rotate(2deg) scale(1);
  transition-delay: 0.1s;
}

.resizes-logo-wrapper.dropdown-active .dropdown-item-2 {
  opacity: 1;
  transform: translateX(0) rotate(-2deg) scale(1);
  transition-delay: 0.2s;
}

.resizes-logo-wrapper.dropdown-active .dropdown-item-3 {
  opacity: 1;
  transform: translateX(0) rotate(1deg) scale(1);
  transition-delay: 0.3s;
}

.dropdown-item:hover {
  transform: translateX(10px) rotate(3deg) scale(1.08) !important;
  background: #1a1a2e;
  color: whitesmoke;
  box-shadow: 0 6px 20px rgba(26, 26, 46, 0.3);
  border-color: #1a1a2e;
}

.dropdown-item:hover .dropdown-icon {
  transform: rotate(360deg) scale(1.2);
  animation: bounce 0.6s ease;
}

.dropdown-icon {
  font-size: 1.4rem;
  display: inline-block;
  transition: transform 0.6s cubic-bezier(0.68, -0.55, 0.265, 1.55);
  transform-origin: center;
}

.dropdown-text {
  flex: 1;
  transition: transform 0.3s ease;
}

.dropdown-item:hover .dropdown-text {
  transform: translateX(4px);
}

@keyframes bounce {
  0%, 100% {
    transform: translateY(0) rotate(360deg) scale(1.2);
  }
  50% {
    transform: translateY(-8px) rotate(360deg) scale(1.2);
  }
}

.dash-container {
  position: absolute;
  bottom: 80px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 2;
  width: 100%;
  max-width: 1400px;
  padding: 0 40px;
}

.dash-logo-link {
  display: inline-block;
  text-decoration: none;
  transition: transform 0.3s ease, opacity 0.3s ease;
}

.dash-logo-link:hover {
  transform: translateY(-2px);
  opacity: 0.9;
}

.dash-logo {
  max-width: 180px;
  height: auto;
  filter: grayscale(100%);
  transition: filter 0.3s ease;
}

.dash-logo-link:hover .dash-logo {
  filter: grayscale(80%);
}

/* Resizes Section */
.resizes-section {
  background: whitesmoke;
  position: relative;
}

.resizes-section .section-content {
  align-items: center;
  justify-content: center;
}

.resizes-center-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  z-index: 2;
}

.resizes-section .notification-content {
  position: absolute;
  top: 16px;
  right: 16px;
  pointer-events: auto;
  z-index: 2;
  text-decoration: none;
  display: inline-block;
  transition: transform 0.3s ease, opacity 0.3s ease;
}

.resizes-section .notification-content:hover {
  transform: translateY(-2px);
  opacity: 1;
}

.notification-image {
  width: auto;
  height: 55px;
  filter: grayscale(100%);
  opacity: 0.95;
  transition: filter 0.3s ease;
}

.resizes-section .notification-content:hover .notification-image {
  filter: grayscale(80%);
}

.toast-slide-enter-active,
.toast-slide-leave-active {
  transition: transform 280ms ease, opacity 280ms ease;
}

.toast-slide-enter-from,
.toast-slide-leave-to {
  transform: translateX(24px);
  opacity: 0;
}

.toast-slide-enter-to,
.toast-slide-leave-from {
  transform: translateX(0);
  opacity: 1;
}

.resizes-section .image-content {
  position: absolute;
  bottom: 0;
  width: auto;
  display: flex;
  align-items: flex-end;
  justify-content: flex-end;
  pointer-events: none;
  z-index: 1;
}

.resizes-tube-image {
  left: 0;
}

.resizes-tube-image .character-image {
  transform: scaleX(-1);
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
  margin-bottom: 3rem;
  background: linear-gradient(to right, #d3d3d3, #555);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  color: transparent;
}

.profile-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  width: 100%;
}

.profile-image-wrapper {
  width: 280px;
  height: 280px;
  border-radius: 50%;
  overflow: visible;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
  position: relative;
  cursor: pointer;
}

.profile-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  filter: grayscale(100%);
  border-radius: 50%;
}

.profile-hover-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.3s ease;
  z-index: 10;
  overflow: visible;
}

.profile-image-wrapper:hover .profile-hover-overlay {
  opacity: 1;
  pointer-events: none;
}

.hover-item {
  position: absolute;
  display: flex;
  align-items: center;
  gap: 20px;
  white-space: nowrap;
}

.item-1 {
  top: 12%;
  left: 100%;
  margin-left: 20px;
}

.item-2 {
  top: 30%;
  left: 100%;
  margin-left: 20px;
}

.item-3 {
  top: 50%;
  left: 100%;
  margin-left: 20px;
}

.item-4 {
  top: 70%;
  left: 100%;
  margin-left: 20px;
}

.arrow-line {
  width: 120px;
  height: 40px;
  overflow: visible;
}

.arrow-line path {
  stroke-linecap: round;
  stroke-linejoin: round;
  stroke-width: 2.5;
  opacity: 0.85;
}

.hover-label {
  display: flex;
  flex-direction: column;
  font-size: 0.9rem;
  color: #1a1a2e;
  font-weight: 500;
}

.label-line-1 {
  font-weight: 600;
  margin-bottom: 2px;
}

.label-line-2 {
  font-weight: 400;
  opacity: 0.8;
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
  transition: all 0.3s ease;
  padding: 8px 12px;
  border-radius: 6px;
}

.social-link:hover {
  background-color: rgba(0, 0, 0, 0.05);
  transform: translateY(-2px);
  color: #0077b5;
}

.social-link:hover .linkedin-icon {
  fill: #0077b5;
  transform: scale(1.1);
}

.social-handle {
  font-weight: 500;
}

.linkedin-icon {
  width: 20px;
  height: 20px;
  fill: #0077b5;
  transition: all 0.3s ease;
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
  opacity: 1;
  transform: translateX(0);
  transition: all 0.3s ease;
  white-space: nowrap;
  color: #d3d3d3;
}

.nav-item:hover .nav-link,
.nav-item.active .nav-link,
.nav-item:focus-within .nav-link {
  color: #000;
}

.nav-item:hover .nav-label,
.nav-item.active .nav-label,
.nav-item:focus-within .nav-label {
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

/* When one item is active, keep others grey */
.nav-item:not(.active):not(:hover):not(:focus-within) .nav-link {
  color: #d3d3d3;
}

.nav-item:not(.active):not(:hover):not(:focus-within) .nav-label {
  color: #d3d3d3;
}

.nav-item:not(.active):not(:hover):not(:focus-within) .nav-dot {
  background: #d3d3d3;
  border-color: whitesmoke;
  transform: scale(1);
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

  .language-toggle {
    top: 15px;
    right: 15px;
    padding: 6px 12px;
    font-size: 0.85rem;
  }

  .where-worked-header {
    flex-direction: column;
    gap: 2.5rem;
  }

  #where-worked .section-content {
    max-width: 100%;
    padding: 0 20px;
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

  .language-toggle {
    top: 10px;
    right: 10px;
    padding: 6px 10px;
    font-size: 0.8rem;
    min-width: 45px;
  }

  .nav-label {
    display: none;
  }

  .ellipse-img {
    height: 45vh;
    opacity: 0.18;
  }

  .notification-image {
    height: 45px;
  }

  .comparison-container {
    grid-template-columns: 1fr;
    gap: 3rem;
  }

  .comparison-title {
    font-size: 1.75rem;
  }

  .where-worked-header {
    flex-direction: column;
    gap: 2rem;
  }

  #where-worked .section-content {
    max-width: 100%;
    padding: 0;
  }

  .timeline-container-full {
    padding: 40px 0;
  }

  .horizontal-timeline {
    flex-wrap: wrap;
    gap: 40px;
    padding: 0 10px;
  }

  .steps-container {
    margin-top: 3rem;
  }

  .steps-wrapper {
    flex-direction: column;
    gap: 2.5rem;
    align-items: center;
  }

  .steps-line {
    display: none;
  }

  .step-item {
    flex-direction: row;
    align-items: center;
    justify-content: center;
    width: 100%;
    gap: 1.5rem;
  }

  .step-marker {
    width: 50px;
    height: 50px;
    flex-shrink: 0;
  }

  .step-number {
    font-size: 1.2rem;
  }

  .step-content {
    margin-top: 0;
    text-align: center;
    max-width: none;
  }

  .step-title {
    font-size: 1.1rem;
  }

  .timeline-line {
    display: none;
  }

  .timeline-item {
    flex: 0 0 calc(50% - 20px);
    min-width: 200px;
  }

  .timeline-connector {
    display: none;
  }

  .timeline-content {
    position: relative;
    width: 100%;
    margin: 20px 0 0 0;
    transform: none;
    left: auto;
  }

  .timeline-item-top .timeline-content,
  .timeline-item-bottom .timeline-content {
    position: relative;
    margin: 20px 0 0 0;
    transform: none;
    left: auto;
  }

  .timeline-marker {
    width: 16px;
    height: 16px;
  }

  .timeline-year {
    font-size: 0.85rem;
  }

  .job-position {
    font-size: 0.95rem;
  }

  .timeline-content {
    padding: 1rem;
  }
}

@media (max-width: 480px) {
  .main-title {
    font-size: 2rem;
  }

  .section-title {
    font-size: 1.75rem;
  }

  .where-worked-header {
    flex-direction: column;
    gap: 1.5rem;
  }

  #where-worked .section-content {
    max-width: 100%;
    padding: 0;
  }

  .timeline-container-full {
    padding: 30px 0;
  }

  .horizontal-timeline {
    gap: 30px;
    padding: 0 5px;
  }

  .steps-container {
    margin-top: 2rem;
  }

  .step-marker {
    width: 45px;
    height: 45px;
  }

  .step-number {
    font-size: 1rem;
  }

  .step-title {
    font-size: 1rem;
  }

  .timeline-item {
    flex: 0 0 100%;
    min-width: 100%;
  }

  .timeline-content {
    width: 100%;
    padding: 0.9rem;
  }

  .first-job-section .section-content {
    flex-direction: column;
    gap: 2rem;
  }

  .rectangle-28 {
    max-width: 100%;
  }

  .timeline-marker {
    width: 14px;
    height: 14px;
  }

  .timeline-year {
    font-size: 0.8rem;
  }

  .job-position {
    font-size: 0.9rem;
  }

  .job-company {
    font-size: 0.85rem;
  }

  .job-location {
    font-size: 0.8rem;
  }

  .first-job-section .section-content {
    flex-direction: column;
    gap: 2rem;
  }

  .rectangle-28 {
    max-width: 100%;
  }
}
</style>
