<template>
  <div class="landing-page">
    <!-- Generative Background Canvas -->
    <canvas
      ref="noiseCanvas"
      class="generative-bg"
      aria-hidden="true"
    ></canvas>

    <!-- Language Toggle Button -->
    <Transition name="fade">
      <button 
        v-if="activeSection === 'masterclass'"
        class="language-toggle" 
        @click="toggleLanguage"
        :aria-label="currentLanguage === 'en' ? 'Cambiar a español' : 'Switch to English'"
        :title="t.aria.toggleLanguage"
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
          :alt="t.alt.character"
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
          <h2 class="section-title scroll-reveal">{{ t.whoIAm.title }}</h2>
          <p class="section-subtitle scroll-reveal">{{ t.whoIAm.subtitle }}</p>
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
                  <div class="label-line-1">{{ t.whoIAm.profileHover.industrialDesign }}</div>
                  <div class="label-line-2">ESAPA</div>
                </div>
              </div>
              <div class="hover-item item-2">
                <svg class="arrow-line" viewBox="0 0 200 50" preserveAspectRatio="none">
                  <path d="M 0,25 Q 18,23 35,25 Q 52,27 70,25 Q 88,23 105,24 Q 122,25 140,26 Q 158,27 175,25 Q 192,23 200,25" 
                        stroke="#1a1a2e" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" marker-end="url(#arrowhead-profile)" opacity="0.9"/>
                </svg>
                <div class="hover-label">
                  <div class="label-line-1">{{ t.whoIAm.profileHover.masterFullStack }}</div>
                  <div class="label-line-2">UNIR</div>
                </div>
              </div>
              <div class="hover-item item-3">
                <svg class="arrow-line" viewBox="0 0 200 50" preserveAspectRatio="none">
                  <path d="M 0,27 Q 20,24 40,26 Q 60,28 80,27 Q 100,26 120,25 Q 140,24 160,26 Q 180,28 200,25" 
                        stroke="#1a1a2e" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" marker-end="url(#arrowhead-profile)" opacity="0.9"/>
                </svg>
                <div class="hover-label">
                  <div class="label-line-1">{{ t.whoIAm.profileHover.bootcampFullStack }}</div>
                  <div class="label-line-2">Factoría F5</div>
                </div>
              </div>
              <div class="hover-item item-4">
                <svg class="arrow-line" viewBox="0 0 200 50" preserveAspectRatio="none">
                  <path d="M 0,24 Q 16,26 32,24 Q 48,22 64,23 Q 80,24 96,25 Q 112,26 128,25 Q 144,24 160,23 Q 176,22 192,24 Q 196,24 200,25" 
                        stroke="#1a1a2e" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" marker-end="url(#arrowhead-profile)" opacity="0.9"/>
                </svg>
                <div class="hover-label">
                  <div class="label-line-1">{{ t.whoIAm.profileHover.trialAndError }}</div>
                  <div class="label-line-2">{{ t.whoIAm.profileHover.life }}</div>
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
            <h2 class="section-title scroll-reveal">{{ t.whereWorked.title }}</h2>
            <p class="section-subtitle scroll-reveal">{{ t.whereWorked.subtitle }}</p>
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
          <h2 class="section-title scroll-reveal">{{ t.whyStudied.title }}</h2>
          <p class="section-subtitle scroll-reveal">{{ t.whyStudied.subtitle }}</p>
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
          :alt="t.alt.platonic"
          class="platonic-image"
        />
      </div>
      <div class="section-content">
        <div class="text-content">
          <h2 class="section-title scroll-reveal">{{ t.whenStudied.title }}</h2>
          <p class="section-subtitle scroll-reveal">{{ t.whenStudied.subtitle }}</p>
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
          <h2 class="section-title scroll-reveal">{{ t.masterVsBootcamp.title }}</h2>
          <p class="section-subtitle scroll-reveal">{{ t.masterVsBootcamp.subtitle }}</p>
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
          <h2 class="section-title scroll-reveal">{{ t.firstJob.title }}</h2>
          <p class="section-subtitle scroll-reveal">{{ t.firstJob.subtitle }}</p>
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
          <h2 class="section-title scroll-reveal">{{ t.softSkills.title }}</h2>
          <p class="section-subtitle scroll-reveal">{{ t.softSkills.subtitle }}</p>
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
      <div class="section-content resizes-redesigned">
        <div class="resizes-main-content">
          <div class="resizes-left-column">
            <div class="resizes-logo-wrapper" :class="{ 'dropdown-active': resizesDropdownOpen }" ref="resizesDropdownWrapper">
              <button
                @click="toggleResizesDropdown"
                class="resizes-logo-link"
                type="button"
                :aria-expanded="resizesDropdownOpen"
                :aria-label="t.aria.toggleResizesMenu"
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
          <div class="chat-container chat-container-lg" ref="chatContainer">
            <div
              v-for="(msg, index) in chatMessages"
              :key="index"
              :class="['chat-bubble', msg.type, { 'chat-visible': visibleMessages > index }]"
            >
              <p class="chat-text">{{ msg.text }}</p>
            </div>
            <div v-if="chatTyping" class="chat-bubble question typing-indicator">
              <span class="typing-dot"></span>
              <span class="typing-dot"></span>
              <span class="typing-dot"></span>
            </div>
          </div>
        </div>
      </div>
      <Transition name="toast-slide">
        <a
          v-if="resizesNotificationVisible"
          href="mailto:hello@resiz.es"
          class="notification-content"
          :aria-label="t.aria.contactResizes"
        >
          <img
            :src="notificationImage"
            :alt="t.aria.contactResizes"
            class="notification-image"
          />
        </a>
      </Transition>
      <div class="image-content resizes-tube-image">
        <img
          :src="tubeImage"
          :alt="t.alt.character"
          class="character-image"
        />
      </div>
      <div class="resizes-bottom-tagline">
        <p class="resizes-tagline-text">{{ t.resizes.description }}</p>
      </div>
      <button
        v-if="hasNextSection('resizes')"
        class="scroll-arrow-btn"
        @click="scrollToNextSection"
        :aria-label="t.aria.scrollNext"
      >
        <svg class="arrow-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M6 9l6 6 6-6"/>
        </svg>
      </button>
    </section>

    <!-- My Real Stack Section -->
    <section id="my-stack" class="section content-section">
      <div class="ellipse-bg" aria-hidden="true">
        <img :src="ellipseImage" alt="" class="ellipse-img" />
      </div>
      <div class="section-content">
        <div class="text-content">
          <h2 class="section-title scroll-reveal">{{ t.myStack.title }}</h2>
          <p class="section-subtitle scroll-reveal">{{ t.myStack.subtitle }}</p>
          <div class="radial-hero-wrapper scroll-reveal">
            <svg :viewBox="radialViewBox" class="radial-hero" xmlns="http://www.w3.org/2000/svg">
              <!-- Background ring of ticks -->
              <line
                v-for="(tick, ti) in radialTicks" :key="'tick-' + ti"
                :x1="tick.x1" :y1="tick.y1" :x2="tick.x2" :y2="tick.y2"
                class="ring-tick"
              />
              <!-- Category arcs + labels -->
              <g
                v-for="cat in radialCats" :key="'cat-' + cat.catIndex"
                @mouseenter="radialHovered = cat.catIndex"
                @mouseleave="radialHovered = null"
                style="cursor: pointer;"
              >
                <path
                  :d="cat.arcPath"
                  class="ring-cat-arc"
                  :class="{ active: radialHovered === cat.catIndex, dimmed: radialHovered !== null && radialHovered !== cat.catIndex }"
                />
                <text
                  :x="cat.labelX" :y="cat.labelY"
                  text-anchor="middle"
                  class="ring-cat-label"
                  :class="{ active: radialHovered === cat.catIndex, dimmed: radialHovered !== null && radialHovered !== cat.catIndex }"
                >{{ cat.name }}</text>
              </g>
              <!-- Stack items -->
              <g
                v-for="(node, ni) in radialNodes" :key="'node-' + ni"
                class="ring-item"
                :class="{ dimmed: radialHovered !== null && radialHovered !== node.catIndex }"
                @mouseenter="radialHovered = node.catIndex"
                @mouseleave="radialHovered = null"
                style="cursor: pointer;"
              >
                <!-- Highlighted tick on the ring -->
                <line
                  :x1="node.x1" :y1="node.y1"
                  :x2="node.x2" :y2="node.y2"
                  class="ring-item-tick"
                  :class="{ active: radialHovered === node.catIndex }"
                />
                <!-- Label card -->
                <g :transform="`translate(${node.cardX}, ${node.cardY}) rotate(${node.tilt})`">
                  <rect
                    :x="-node.cardW / 2" :y="-node.cardH / 2"
                    :width="node.cardW" :height="node.cardH"
                    rx="10"
                    class="ring-card"
                    :class="{ active: radialHovered === node.catIndex }"
                  />
                  <text
                    text-anchor="middle"
                    dominant-baseline="central"
                    y="1"
                    class="ring-card-text"
                  >{{ node.name }}</text>
                </g>
              </g>
            </svg>
          </div>
        </div>
      </div>
      <button
        v-if="hasNextSection('my-stack')"
        class="scroll-arrow-btn"
        @click="scrollToNextSection"
        :aria-label="t.aria.scrollNext"
      >
        <svg class="arrow-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M6 9l6 6 6-6"/>
        </svg>
      </button>
    </section>

    <!-- A Day as Full Stack Section -->
    <section id="my-day" class="section content-section">
      <div class="ellipse-bg" aria-hidden="true">
        <img :src="ellipseImage" alt="" class="ellipse-img" />
      </div>
      <div class="section-content">
        <div class="text-content">
          <h2 class="section-title scroll-reveal">{{ t.myDay.title }}</h2>
          <p class="section-subtitle scroll-reveal">{{ t.myDay.subtitle }}</p>
          <div class="day-timeline">
            <div class="day-timeline-line"></div>
            <div
              v-for="(item, index) in t.myDay.items"
              :key="index"
              class="day-item scroll-reveal"
            >
              <div class="day-time">{{ item.time }}</div>
              <div class="day-marker"></div>
              <div class="day-content">
                <h3 class="day-title">{{ item.title }}</h3>
                <p class="day-desc">{{ item.desc }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
      <button
        v-if="hasNextSection('my-day')"
        class="scroll-arrow-btn"
        @click="scrollToNextSection"
        :aria-label="t.aria.scrollNext"
      >
        <svg class="arrow-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M6 9l6 6 6-6"/>
        </svg>
      </button>
    </section>

    <!-- Closing Section -->
    <section id="closing" class="section content-section closing-section">
      <div class="ellipse-bg" aria-hidden="true">
        <img :src="ellipseImage" alt="" class="ellipse-img" />
      </div>
      <div class="section-content">
        <div class="text-content">
          <h2 class="section-title scroll-reveal">{{ t.closing.title }}</h2>
          <p class="section-subtitle scroll-reveal">{{ t.closing.subtitle }}</p>
          <p class="closing-message scroll-reveal">{{ t.closing.message }}</p>
          <div class="closing-links scroll-reveal">
            <a
              href="https://www.linkedin.com/in/albegosu/"
              target="_blank"
              rel="noopener noreferrer"
              class="closing-link"
            >
              <span class="closing-link-icon">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-4 0v7h-4v-7a6 6 0 0 1 6-6z"/><rect x="2" y="9" width="4" height="12"/><circle cx="4" cy="4" r="2"/></svg>
              </span>
              {{ t.closing.linkedin }}
            </a>
            <a
              href="mailto:alberto@resiz.es"
              class="closing-link"
            >
              <span class="closing-link-icon">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><polyline points="22,6 12,13 2,6"/></svg>
              </span>
              {{ t.closing.email }}
            </a>
          </div>
        </div>
      </div>
      <div class="closing-footer">
        <span class="closing-name">Alberto González</span>
        <span class="closing-separator">/</span>
        <span class="closing-role">{{ t.whoIAm.profileTitle }}</span>
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
      subtitle: 'Survival Kit'
    },
    whoIAm: {
      title: 'Who I am',
      subtitle: 'From Industrial Design to Web Development',
      profileTitle: 'Full Stack Engineer',
      profileHover: {
        industrialDesign: 'Industrial Design',
        masterFullStack: 'Master Full Stack',
        bootcampFullStack: 'Bootcamp Full Stack',
        trialAndError: 'Trial and Error',
        life: 'Life'
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
      subtitle: 'Key points of my experience',
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
    myStack: {
      title: 'My Real Stack',
      subtitle: 'What I actually use every day',
      frontend: 'Frontend',
      backend: 'Backend',
      tools: 'Tools',
      ai: 'AI & Productivity'
    },
    myDay: {
      title: 'A Day as Full Stack',
      subtitle: 'What a typical workday looks like',
      items: [
        { time: '9:00', title: 'Stand-up & Planning', desc: 'Quick sync with the team: what we did, what we\'ll do, and blockers.' },
        { time: '10:00', title: 'Build & Code', desc: 'Deep work time. Frontend features, API endpoints, database queries — the core of the day.' },
        { time: '13:00', title: 'Code Review', desc: 'Reviewing PRs, discussing approaches, learning from teammates\' code.' },
        { time: '14:00', title: 'Lunch & Learning', desc: 'Recharge and explore — articles, videos, side experiments.' },
        { time: '15:30', title: 'Testing & Debugging', desc: 'Write tests, fix bugs, and ensure everything works before shipping.' },
        { time: '17:00', title: 'Deploy & Docs', desc: 'Ship to production, update documentation, and plan tomorrow.' }
      ]
    },
    resizes: {
      subtitle: 'My place',
      description: 'A tech company building tools that help developers and teams work smarter.',
      chat: [
        { type: 'question', text: 'What do you do at Resizes?' },
        { type: 'answer', text: 'I build Dash — a platform that helps dev teams streamline their workflows. I work across the full stack, from Vue 3 frontends to Node.js APIs.' },
        { type: 'question', text: 'How\'s the team?' },
        { type: 'answer', text: 'Small, focused, and very autonomous. We move fast, ship often, and everyone has real ownership over what they build.' },
        { type: 'question', text: 'What tech do you use?' },
        { type: 'answer', text: 'Vue 3 + Nuxt on the frontend, Node.js + PostgreSQL on the backend. We use Claude and Cursor daily — AI is part of the workflow, not a gimmick.' }
      ]
    },
    nav: {
      masterclass: 'Masterclass',
      whoIAm: 'Who I am',
      whereWorked: 'Where I\'ve worked',
      whyStudied: 'Why?',
      whenStudied: 'When I studied',
      masterVsBootcamp: 'Master vs. Bootcamp',
      firstJob: 'New Job',
      softSkills: 'Soft Skills',
      myStack: 'My Stack',
      myDay: 'My Day',
      resizes: 'Resizes',
      closing: 'Thank you'
    },
    closing: {
      title: 'Thank you',
      subtitle: 'Let\'s keep in touch',
      message: 'If you have any questions or just want to connect, don\'t hesitate to reach out.',
      linkedin: 'Connect on LinkedIn',
      email: 'Send me an email'
    },
    aria: {
      scrollNext: 'Scroll to next section',
      toggleLanguage: 'Toggle language',
      toggleResizesMenu: 'Toggle Resizes menu',
      contactResizes: 'Contact Resizes'
    },
    alt: {
      character: '3D Character',
      platonic: 'Platonic Dodecahedron'
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
        trialAndError: 'Prueba y error',
        life: 'La vida'
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
      subtitle: 'Los puntos más importantes para mí',
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
          title: 'Más trabajo colaborativo',
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
    myStack: {
      title: 'Mi Stack Real',
      subtitle: 'Lo que uso de verdad cada día',
      frontend: 'Frontend',
      backend: 'Backend',
      tools: 'Herramientas',
      ai: 'IA y Productividad'
    },
    myDay: {
      title: 'Un día como Full Stack',
      subtitle: 'Cómo es un día típico de trabajo',
      items: [
        { time: '9:00', title: 'Stand-up y Planificación', desc: 'Sincronización rápida con el equipo: qué hicimos, qué haremos y bloqueos.' },
        { time: '10:00', title: 'Construir y Programar', desc: 'Tiempo de trabajo profundo. Features de frontend, endpoints de API, consultas a base de datos — el núcleo del día.' },
        { time: '13:00', title: 'Code Review', desc: 'Revisando PRs, discutiendo enfoques, aprendiendo del código de los compañeros.' },
        { time: '14:00', title: 'Comida y Aprendizaje', desc: 'Recargar y explorar — artículos, vídeos, experimentos propios.' },
        { time: '15:30', title: 'Testing y Debugging', desc: 'Escribir tests, arreglar bugs y asegurar que todo funciona antes de desplegar.' },
        { time: '17:00', title: 'Deploy y Documentación', desc: 'Desplegar a producción, actualizar documentación y planificar el día siguiente.' }
      ]
    },
    resizes: {
      subtitle: 'Mi lugar',
      description: 'Una empresa tech que construye herramientas para que desarrolladores y equipos trabajen mejor.',
      chat: [
        { type: 'question', text: '¿Qué haces en Resizes?' },
        { type: 'answer', text: 'Construyo Dash — una plataforma que ayuda a equipos de desarrollo a optimizar sus flujos de trabajo. Trabajo en todo el stack, desde frontends con Vue 3 hasta APIs con Node.js.' },
        { type: 'question', text: '¿Cómo es el equipo?' },
        { type: 'answer', text: 'Pequeño, enfocado y muy autónomo. Nos movemos rápido, desplegamos a menudo y cada uno tiene responsabilidad real sobre lo que construye.' },
        { type: 'question', text: '¿Qué tecnología usáis?' },
        { type: 'answer', text: 'Vue 3 + Nuxt en el frontend, Node.js + PostgreSQL en el backend. Usamos Claude y Cursor a diario — la IA es parte del flujo de trabajo, no un adorno.' }
      ]
    },
    nav: {
      masterclass: 'Masterclass',
      whoIAm: 'Quién soy',
      whereWorked: 'Dónde he trabajado',
      whyStudied: '¿Por qué?',
      whenStudied: 'Cuándo estudié',
      masterVsBootcamp: 'Máster vs. Bootcamp',
      firstJob: 'Nuevo trabajo',
      softSkills: 'Habilidades blandas',
      myStack: 'Mi Stack',
      myDay: 'Mi día',
      resizes: 'Resizes',
      closing: 'Gracias'
    },
    closing: {
      title: 'Gracias',
      subtitle: 'Mantengamos el contacto',
      message: 'Si tienes alguna pregunta o simplemente quieres conectar, no dudes en escribirme.',
      linkedin: 'Conecta en LinkedIn',
      email: 'Envíame un email'
    },
    aria: {
      scrollNext: 'Desplazarse a la siguiente sección',
      toggleLanguage: 'Cambiar idioma',
      toggleResizesMenu: 'Abrir el menú de Resizes',
      contactResizes: 'Contactar con Resizes'
    },
    alt: {
      character: 'Personaje 3D',
      platonic: 'Dodecaedro platónico'
    }
  }
}

// Computed translation helper
const t = computed(() => translations[currentLanguage.value])

// Toggle language function
const toggleLanguage = () => {
  currentLanguage.value = currentLanguage.value === 'en' ? 'es' : 'en'
}

// Keep <html lang> in sync with the selected language
watch(currentLanguage, (lang) => {
  document.documentElement.lang = lang
}, { immediate: true })

// Computed work experience with translations
const workExperience = computed(() => [
  {
    position: currentLanguage.value === 'en' ? 'Sales Advisor' : 'Asesor de ventas',
    company: 'H&M, PULL&BEAR, Stradivarius, Decathlon',
    location: currentLanguage.value === 'en' ? 'Madrid, Gijón, Avilés, Spain' : 'Madrid, Gijón, Avilés, España',
    year: '2012 - 2017'
  },
  {
    position: currentLanguage.value === 'en' ? 'Industrial Designer' : 'Diseñador Industrial',
    company: 'enPieza eStudio',
    location: currentLanguage.value === 'en' ? 'Madrid, Spain' : 'Madrid, España',
    year: '2016 - 2017'
  },
  {
    position: currentLanguage.value === 'en' ? 'Senior CAD Specialist' : 'Especialista CAD Senior',
    company: 'Align Technology',
    location: currentLanguage.value === 'en' ? 'Madrid, Spain' : 'Madrid, España',
    year: '2018 - 2022'
  },
  {
    position: currentLanguage.value === 'en' ? 'Production Manager' : 'Responsable de producción',
    company: 'MYL ideas',
    location: currentLanguage.value === 'en' ? 'Oviedo, Asturias, Spain' : 'Oviedo, Asturias, España',
    year: '2022 - 2023'
  },
  {
    position: currentLanguage.value === 'en' ? 'Consultant and Analyst' : 'Consultor y Analista',
    company: 'Minsait',
    location: currentLanguage.value === 'en' ? 'Gijón, Asturias, Spain' : 'Gijón, Asturias, España',
    year: '2024 - 2025'
  },
  {
    position: currentLanguage.value === 'en' ? 'Full Stack Engineer' : 'Ingeniero Full Stack',
    company: 'Resizes',
    location: currentLanguage.value === 'en' ? 'Gijón, Asturias, Spain' : 'Gijón, Asturias, España',
    year: '2025'
  },
  {
    position: currentLanguage.value === 'en' ? 'Product Engineer' : 'Product Engineer',
    company: 'Resizes',
    location: currentLanguage.value === 'en' ? 'Gijón, Asturias, Spain' : 'Gijón, Asturias, España',
    year: '2026 - Present'
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
  { id: 'resizes', label: t.value.nav.resizes },
  { id: 'my-stack', label: t.value.nav.myStack },
  { id: 'my-day', label: t.value.nav.myDay },
  { id: 'closing', label: t.value.nav.closing }
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

// ── Radial stack timeline state ──
const radialHovered = ref(null)

const radialData = computed(() => {
  const cx = 350, cy = 350
  const R = 250
  const categories = [
    { name: t.value.myStack.ai, items: ['Claude', 'Copilot', 'Claude Code', 'Cursor'] },
    { name: t.value.myStack.frontend, items: ['Vue 3', 'Nuxt', 'React', 'Vite', 'Tailwind'] },
    { name: t.value.myStack.backend, items: ['Node.js', 'Express', 'Python', 'PostgreSQL', 'TypeScript'] },
    { name: t.value.myStack.tools, items: ['Git', 'GitHub Actions', 'Docker', 'VS Code'] }
  ]

  const toXY = (angleDeg, radius) => {
    const a = (angleDeg * Math.PI) / 180
    return { x: cx + radius * Math.cos(a), y: cy + radius * Math.sin(a) }
  }

  const ticks = []
  const TICK_COUNT = 96
  for (let i = 0; i < TICK_COUNT; i++) {
    const deg = (i / TICK_COUNT) * 360
    const p1 = toXY(deg, R)
    const p2 = toXY(deg, R + 16)
    ticks.push({ x1: p1.x, y1: p1.y, x2: p2.x, y2: p2.y })
  }

  // Each category owns a contiguous arc, sized by item count, with gaps between groups
  const gapDeg = 16
  const totalItems = categories.reduce((sum, c) => sum + c.items.length, 0)
  const slotDeg = (360 - gapDeg * categories.length) / totalItems

  const nodes = []
  const cats = []
  let cursor = -90 + gapDeg / 2

  categories.forEach((cat, ci) => {
    const arcStart = cursor
    const arcEnd = cursor + slotDeg * cat.items.length
    const midDeg = (arcStart + arcEnd) / 2

    const arcR = R - 26
    const a1 = toXY(arcStart + 2, arcR)
    const a2 = toXY(arcEnd - 2, arcR)
    const labelPos = toXY(midDeg, R - 66)
    cats.push({
      name: cat.name,
      catIndex: ci,
      arcPath: `M ${a1.x} ${a1.y} A ${arcR} ${arcR} 0 0 1 ${a2.x} ${a2.y}`,
      labelX: labelPos.x,
      labelY: labelPos.y
    })

    cat.items.forEach((name, ii) => {
      const deg = arcStart + slotDeg * (ii + 0.5)
      const idx = nodes.length
      const p1 = toXY(deg, R - 4)
      const p2 = toXY(deg, R + 20)
      const cardPos = toXY(deg, R + 62 + (idx % 2) * 32)
      nodes.push({
        name,
        catIndex: ci,
        x1: p1.x, y1: p1.y, x2: p2.x, y2: p2.y,
        cardX: cardPos.x, cardY: cardPos.y,
        cardW: Math.max(name.length * 8 + 26, 64),
        cardH: 32,
        tilt: ((idx * 47) % 13) - 6
      })
    })

    cursor = arcEnd + gapDeg
  })

  return { ticks, nodes, cats }
})

const radialTicks = computed(() => radialData.value.ticks)
const radialNodes = computed(() => radialData.value.nodes)
const radialCats = computed(() => radialData.value.cats)
const radialViewBox = computed(() => {
  const nodes = radialData.value.nodes
  if (!nodes.length) return '0 0 700 700'
  let minX = Infinity, minY = Infinity, maxX = -Infinity, maxY = -Infinity
  nodes.forEach(n => {
    const half = n.cardW / 2 + 14
    minX = Math.min(minX, n.cardX - half)
    minY = Math.min(minY, n.cardY - 32)
    maxX = Math.max(maxX, n.cardX + half)
    maxY = Math.max(maxY, n.cardY + 32)
  })
  minX = Math.min(minX, 50)
  minY = Math.min(minY, 50)
  maxX = Math.max(maxX, 650)
  maxY = Math.max(maxY, 650)
  return `${minX} ${minY} ${maxX - minX} ${maxY - minY}`
})

// ── Chatbox state ──
const chatContainer = ref(null)
const visibleMessages = ref(0)
const chatTyping = ref(false)
let chatTimeouts = []
let chatObserver = null

const chatMessages = computed(() => t.value.resizes.chat || [])

const startChatSequence = () => {
  visibleMessages.value = 0
  chatTyping.value = false
  chatTimeouts.forEach(clearTimeout)
  chatTimeouts = []

  const msgs = chatMessages.value
  msgs.forEach((msg, index) => {
    const typingDelay = index * 1800
    chatTimeouts.push(setTimeout(() => { chatTyping.value = true }, typingDelay))
    chatTimeouts.push(setTimeout(() => {
      chatTyping.value = false
      visibleMessages.value = index + 1
    }, typingDelay + 800))
  })
}

// ── Generative canvas (Simplex noise particle mesh) ──
// Fixed to the viewport so the mesh lives behind every section; scroll
// shifts the noise field and adds parallax so it feels continuous.
const noiseCanvas = ref(null)
let animationFrameId = null
let canvasCtx = null
let particles = []
const PARTICLE_COUNT = 110
const CONNECTION_DIST = 160
const NOISE_SCALE = 0.003
const NOISE_SPEED = 0.0005
const WRAP_MARGIN = 30
const POINTER_RADIUS = 140
let noiseTime = 0
let viewW = 0
let viewH = 0
let lastScrollY = 0
const pointer = { x: -9999, y: -9999, active: false }
const prefersReducedMotion =
  typeof window !== 'undefined' &&
  window.matchMedia('(prefers-reduced-motion: reduce)').matches

const simplexNoise2D = (() => {
  const perm = new Uint8Array(512)
  const grad = [[1,1],[-1,1],[1,-1],[-1,-1],[1,0],[-1,0],[0,1],[0,-1]]
  for (let i = 0; i < 256; i++) perm[i] = i
  for (let i = 255; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [perm[i], perm[j]] = [perm[j], perm[i]]
  }
  for (let i = 0; i < 256; i++) perm[i + 256] = perm[i]
  const F2 = 0.5 * (Math.sqrt(3) - 1)
  const G2 = (3 - Math.sqrt(3)) / 6

  return (xin, yin) => {
    const s = (xin + yin) * F2
    const i = Math.floor(xin + s), j = Math.floor(yin + s)
    const tt = (i + j) * G2
    const X0 = i - tt, Y0 = j - tt
    const x0 = xin - X0, y0 = yin - Y0
    const i1 = x0 > y0 ? 1 : 0, j1 = x0 > y0 ? 0 : 1
    const x1 = x0 - i1 + G2, y1 = y0 - j1 + G2
    const x2 = x0 - 1 + 2 * G2, y2 = y0 - 1 + 2 * G2
    const ii = i & 255, jj = j & 255

    const dot = (gi, x, y) => {
      const g = grad[gi % 8]
      return g[0] * x + g[1] * y
    }

    let n0 = 0, n1 = 0, n2 = 0
    let t0 = 0.5 - x0 * x0 - y0 * y0
    if (t0 > 0) { t0 *= t0; n0 = t0 * t0 * dot(perm[ii + perm[jj]], x0, y0) }
    let t1 = 0.5 - x1 * x1 - y1 * y1
    if (t1 > 0) { t1 *= t1; n1 = t1 * t1 * dot(perm[ii + i1 + perm[jj + j1]], x1, y1) }
    let t2 = 0.5 - x2 * x2 - y2 * y2
    if (t2 > 0) { t2 *= t2; n2 = t2 * t2 * dot(perm[ii + 1 + perm[jj + 1]], x2, y2) }
    return 70 * (n0 + n1 + n2)
  }
})()

const resizeCanvas = () => {
  const canvas = noiseCanvas.value
  if (!canvas) return
  const dpr = Math.min(window.devicePixelRatio || 1, 2)
  viewW = window.innerWidth
  viewH = window.innerHeight
  canvas.width = viewW * dpr
  canvas.height = viewH * dpr
  if (canvasCtx) canvasCtx.setTransform(dpr, 0, 0, dpr, 0, 0)
}

const seedParticles = () => {
  particles = Array.from({ length: PARTICLE_COUNT }, () => ({
    x: Math.random() * viewW,
    y: Math.random() * viewH,
    r: 1.2 + Math.random() * 1.6,
    depth: 0.4 + Math.random() * 0.6
  }))
}

const initCanvas = () => {
  const canvas = noiseCanvas.value
  if (!canvas || window.innerWidth <= 768) {
    if (canvas) canvas.style.display = 'none'
    return
  }
  canvasCtx = canvas.getContext('2d')
  resizeCanvas()
  seedParticles()
  lastScrollY = window.scrollY
  if (prefersReducedMotion) {
    drawCanvasFrame()
  } else {
    animateCanvas()
  }
}

const drawCanvasFrame = () => {
  const ctx = canvasCtx
  if (!ctx) return

  const scrollY = window.scrollY
  const scrollDelta = scrollY - lastScrollY
  lastScrollY = scrollY

  ctx.clearRect(0, 0, viewW, viewH)
  noiseTime += NOISE_SPEED
  const scrollFactor = scrollY * 0.0004

  particles.forEach(p => {
    const angle = simplexNoise2D(p.x * NOISE_SCALE + scrollFactor, p.y * NOISE_SCALE + noiseTime) * Math.PI * 2
    p.x += Math.cos(angle) * 0.5 * p.depth
    p.y += Math.sin(angle) * 0.5 * p.depth
    p.y -= scrollDelta * 0.12 * p.depth

    if (pointer.active) {
      const pdx = p.x - pointer.x
      const pdy = p.y - pointer.y
      const d2 = pdx * pdx + pdy * pdy
      if (d2 < POINTER_RADIUS * POINTER_RADIUS && d2 > 0.01) {
        const d = Math.sqrt(d2)
        const force = (1 - d / POINTER_RADIUS) * 1.2
        p.x += (pdx / d) * force
        p.y += (pdy / d) * force
      }
    }

    if (p.x < -WRAP_MARGIN) p.x = viewW + WRAP_MARGIN
    if (p.x > viewW + WRAP_MARGIN) p.x = -WRAP_MARGIN
    if (p.y < -WRAP_MARGIN) p.y = viewH + WRAP_MARGIN
    if (p.y > viewH + WRAP_MARGIN) p.y = -WRAP_MARGIN
  })

  const neighbors = particles.map(() => [])
  for (let i = 0; i < particles.length; i++) {
    for (let j = i + 1; j < particles.length; j++) {
      const dx = particles[i].x - particles[j].x
      const dy = particles[i].y - particles[j].y
      const dist = Math.sqrt(dx * dx + dy * dy)
      if (dist < CONNECTION_DIST) {
        neighbors[i].push(j)
        // Gentle separation so the flow field doesn't collapse into dense clumps
        if (dist > 0.01 && dist < 32) {
          const push = (1 - dist / 32) * 0.35
          const ux = dx / dist, uy = dy / dist
          particles[i].x += ux * push
          particles[i].y += uy * push
          particles[j].x -= ux * push
          particles[j].y -= uy * push
        }
        const alpha = (1 - dist / CONNECTION_DIST) * 0.16
        ctx.strokeStyle = `rgba(26, 26, 46, ${alpha})`
        ctx.lineWidth = 1
        ctx.beginPath()
        ctx.moveTo(particles[i].x, particles[i].y)
        ctx.lineTo(particles[j].x, particles[j].y)
        ctx.stroke()
      }
    }
  }

  // Fill triangles between mutually-close particles for a subtle geometric wash.
  // Cap neighbors per particle so dense clusters don't stack fills into dark blobs.
  ctx.fillStyle = 'rgba(26, 26, 46, 0.012)'
  for (let i = 0; i < particles.length; i++) {
    const ns = neighbors[i]
    const limit = Math.min(ns.length, 4)
    for (let a = 0; a < limit; a++) {
      for (let b = a + 1; b < limit; b++) {
        const j = ns[a], k = ns[b]
        const dx = particles[j].x - particles[k].x
        const dy = particles[j].y - particles[k].y
        if (dx * dx + dy * dy < CONNECTION_DIST * CONNECTION_DIST) {
          ctx.beginPath()
          ctx.moveTo(particles[i].x, particles[i].y)
          ctx.lineTo(particles[j].x, particles[j].y)
          ctx.lineTo(particles[k].x, particles[k].y)
          ctx.closePath()
          ctx.fill()
        }
      }
    }
  }

  particles.forEach(p => {
    ctx.fillStyle = `rgba(26, 26, 46, ${0.15 + p.depth * 0.15})`
    ctx.beginPath()
    ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2)
    ctx.fill()
  })
}

const animateCanvas = () => {
  drawCanvasFrame()
  animationFrameId = requestAnimationFrame(animateCanvas)
}

const handlePointerMove = (e) => {
  pointer.x = e.clientX
  pointer.y = e.clientY
  pointer.active = true
}

const handlePointerLeave = () => {
  pointer.active = false
}

let canvasResizeTimeout = null
const handleCanvasResize = () => {
  clearTimeout(canvasResizeTimeout)
  canvasResizeTimeout = setTimeout(() => {
    if (window.innerWidth <= 768) {
      if (noiseCanvas.value) noiseCanvas.value.style.display = 'none'
      if (animationFrameId) cancelAnimationFrame(animationFrameId)
      animationFrameId = null
      return
    }
    if (noiseCanvas.value) noiseCanvas.value.style.display = 'block'
    if (!canvasCtx && noiseCanvas.value) canvasCtx = noiseCanvas.value.getContext('2d')
    resizeCanvas()
    seedParticles()
    if (prefersReducedMotion) {
      drawCanvasFrame()
    } else if (!animationFrameId) {
      animateCanvas()
    }
  }, 200)
}

let scrollRevealObserver = null

const initScrollReveal = () => {
  scrollRevealObserver = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        entry.target.classList.add('revealed')
        scrollRevealObserver.unobserve(entry.target)
      }
    })
  }, { threshold: 0.15, rootMargin: '0px 0px -50px 0px' })

  document.querySelectorAll('.scroll-reveal').forEach((el) => {
    scrollRevealObserver.observe(el)
  })
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
  handleScroll()
  document.addEventListener('click', handleClickOutside)
  setTimeout(initScrollReveal, 100)

  initCanvas()
  window.addEventListener('resize', handleCanvasResize)
  window.addEventListener('pointermove', handlePointerMove, { passive: true })
  document.documentElement.addEventListener('pointerleave', handlePointerLeave)

  setTimeout(() => {
    if (chatContainer.value) {
      chatObserver = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
          if (entry.isIntersecting) {
            startChatSequence()
            chatObserver.unobserve(entry.target)
          }
        })
      }, { threshold: 0.3 })
      chatObserver.observe(chatContainer.value)
    }
  }, 200)
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
  window.removeEventListener('resize', handleCanvasResize)
  window.removeEventListener('pointermove', handlePointerMove)
  document.documentElement.removeEventListener('pointerleave', handlePointerLeave)
  if (chronometerInterval) clearInterval(chronometerInterval)
  if (resizesNotificationTimeout) clearTimeout(resizesNotificationTimeout)
  if (scrollRevealObserver) scrollRevealObserver.disconnect()
  if (animationFrameId) cancelAnimationFrame(animationFrameId)
  if (chatObserver) chatObserver.disconnect()
  chatTimeouts.forEach(clearTimeout)
})
</script>

<style scoped>
.landing-page {
  min-height: 100vh;
  min-height: 100dvh; /* Dynamic viewport height for mobile */
  background: #ebebeb;
  position: relative;
  font-family: 'Switzer', sans-serif;
  overflow-x: hidden;
  width: 100%;
}

/* Safe area insets for modern mobile devices */
@supports (padding: max(0px)) {
  .landing-page {
    padding-left: max(0px, env(safe-area-inset-left));
    padding-right: max(0px, env(safe-area-inset-right));
  }
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
  transform: scale(0.8);
  transform-origin: top right;
}

/* Safe area for language toggle on mobile */
@supports (top: max(0px)) {
  @media (max-width: 768px) {
    .language-toggle {
      top: max(10px, env(safe-area-inset-top));
      right: max(10px, env(safe-area-inset-right));
    }
  }
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
  transform: scale(0.8);
  transform-origin: bottom left;
}

/* Safe area for chronometer on mobile */
@supports (bottom: max(0px)) {
  @media (max-width: 768px) {
    .chronometer {
      bottom: max(15px, env(safe-area-inset-bottom));
      left: max(15px, env(safe-area-inset-left));
    }
  }
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
  min-height: 100dvh; /* Dynamic viewport height for mobile */
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  width: 100%;
  overflow-x: hidden;
  box-sizing: border-box;
}


.section-content {
  max-width: 1400px;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 60px;
  transform: scale(0.8);
  transform-origin: center center;
  position: relative;
  box-sizing: border-box;
}

/* Masterclass Section */
.masterclass-section {
  background: #ebebeb;
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
  transform: translateX(-50%) scale(1.25);
  transform-origin: center bottom;
  width: 100%;
  max-width: 100vw;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  pointer-events: none;
  z-index: 1;
  overflow: hidden;
}

.character-image {
  width: auto;
  height: 90vh;
  max-height: 800px;
  max-width: none;
  object-fit: contain;
  object-position: bottom center;
  filter: grayscale(100%);
}

.scroll-arrow-btn {
  position: absolute;
  bottom: 30px;
  left: 50%;
  transform: translateX(-50%) scale(0.8);
  transform-origin: center bottom;
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
  margin: 0;
}

/* Ensure consistent button position in all sections - especially Soft Skills */
#soft-skills .scroll-arrow-btn {
  bottom: 30px !important;
  position: absolute !important;
  left: 50% !important;
  transform: translateX(-50%) scale(0.8) !important;
  transform-origin: center bottom !important;
  margin-bottom: 0 !important;
}

/* Safe area for scroll button on mobile */
@supports (padding: max(0px)) {
  @media (max-width: 768px) {
    .scroll-arrow-btn {
      bottom: max(20px, env(safe-area-inset-bottom));
    }
  }
  
  @media (max-width: 480px) {
    .scroll-arrow-btn {
      bottom: max(15px, env(safe-area-inset-bottom));
    }
  }
}

.scroll-arrow-btn:hover {
  background: #000;
  transform: translateX(-50%) translateY(-5px) scale(0.8);
  transform-origin: center bottom;
}

#soft-skills .scroll-arrow-btn:hover {
  transform: translateX(-50%) translateY(-5px) scale(0.8) !important;
  transform-origin: center bottom !important;
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
  background: #ebebeb;
}

.who-i-am-section .section-content {
  flex-direction: column;
  align-items: flex-start;
  text-align: left;
}

/* Content Sections */
.content-section {
  background: #ebebeb;
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
  transform: scale(1.25);
  transform-origin: center center;
  max-width: 100%;
  overflow: hidden;
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
  background: #ebebeb;
  position: relative;
}

.when-studied-section .section-content {
  flex-direction: column;
  align-items: center;
  text-align: center;
  margin-top: 120px;
}

@media (max-width: 768px) {
  .when-studied-section .section-content {
    margin-top: 80px;
  }
}

@media (max-width: 480px) {
  .when-studied-section .section-content {
    margin-top: 40px;
  }
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
  transform: translateX(-50%) scale(1.25);
  transform-origin: center top;
  width: 100%;
  max-width: 100vw;
  display: flex;
  align-items: flex-start;
  justify-content: center;
  pointer-events: none;
  z-index: 1;
  overflow: hidden;
}

.platonic-image {
  width: auto;
  height: 70vh;
  max-height: 600px;
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

@media (max-width: 768px) {
  .where-worked-header {
    margin-top: -1.5rem;
    margin-bottom: 4rem;
  }
}

@media (max-width: 480px) {
  .where-worked-header {
    margin-top: -1rem;
    margin-bottom: 3rem;
  }
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

@media (max-width: 768px) {
  .timeline-container-full {
    padding: 40px 0;
  }
}

@media (max-width: 480px) {
  .timeline-container-full {
    padding: 30px 0;
  }
}

.horizontal-timeline {
  position: relative;
  width: 100%;
  max-width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 40px;
  box-sizing: border-box;
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
  transform: scale(1.25);
  transform-origin: top right;
  pointer-events: none;
  z-index: 0;
  overflow: hidden;
  max-width: 100vw;
}

.ellipse-img {
  height: 70vh;
  max-height: 600px;
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
  transform: translateX(-50%) scale(0.8);
  transform-origin: center center;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 2;
  width: 100%;
  max-width: min(1400px, 100vw);
  padding: 0 40px;
  box-sizing: border-box;
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
  transform: scale(0.8);
  transform-origin: top right;
}

.resizes-section .notification-content:hover {
  transform: translateY(-2px) scale(0.8);
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
  max-width: 100vw;
  display: flex;
  align-items: flex-end;
  justify-content: flex-end;
  pointer-events: none;
  z-index: 1;
  overflow: hidden;
  transform: scale(1.25);
  transform-origin: bottom left;
}

.resizes-tube-image {
  left: 0;
}

.resizes-tube-image .character-image {
  transform: scaleX(-1);
  height: 50vh;
  max-height: 400px;
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

@media (max-width: 768px) {
  .profile-content {
    gap: 1.5rem;
  }
}

@media (max-width: 480px) {
  .profile-content {
    gap: 1.25rem;
  }
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
  transform: translateY(-50%) scale(0.8);
  transform-origin: center right;
  z-index: 1000;
}

/* Safe area for side nav on mobile */
@supports (right: max(0px)) {
  @media (max-width: 768px) {
    .side-nav {
      right: max(15px, env(safe-area-inset-right));
    }
  }
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
  .section {
    padding: 70px 30px;
  }

  .section-content {
    flex-direction: column;
    text-align: center;
  }

  .main-title {
    font-size: 3rem;
  }

  .character-image {
    height: 70vh;
    max-height: 650px;
  }

  .platonic-image {
    height: 60vh;
    max-height: 500px;
  }

  .ellipse-img {
    height: 60vh;
    max-height: 500px;
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
    margin-bottom: 6rem;
  }

  #where-worked .section-content {
    max-width: 100%;
    padding: 0 20px;
  }

  .timeline-container-full {
    padding: 60px 0;
  }

  .when-studied-section .section-content {
    margin-top: 100px;
  }
}

@media (max-width: 768px) {
  .section {
    padding: 60px 20px;
    min-height: auto; /* Allow sections to size naturally on mobile */
    min-height: auto;
  }

  .masterclass-section,
  .who-i-am-section,
  .when-studied-section,
  .resizes-section {
    min-height: 100vh;
    min-height: 100dvh;
  }

  .main-title {
    font-size: 2.5rem;
  }

  .section-title {
    font-size: 2rem;
  }

  .section-subtitle {
    margin-bottom: 2rem;
  }

  .subtitle {
    font-size: 1.2rem;
  }

  .character-image {
    height: 50vh;
    max-height: 400px;
  }

  .platonic-image {
    height: 40vh;
    max-height: 300px;
  }

  .profile-image-wrapper {
    width: 220px;
    height: 220px;
  }

  .profile-name {
    font-size: 1.5rem;
  }

  .profile-title {
    font-size: 1.1rem;
    margin-bottom: 1rem;
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
    height: 35vh;
    max-height: 250px;
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
    margin-bottom: 1.5rem;
  }

  .where-worked-header {
    flex-direction: column;
    gap: 2rem;
  }

  #where-worked .section-content {
    max-width: 100%;
    padding: 0;
  }

  #where-worked {
    padding-top: 40px;
    padding-bottom: 40px;
    min-height: auto;
    min-height: auto;
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
    margin-top: 2rem;
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

  .why-studied-list {
    margin-top: 1.5rem;
  }

  .why-studied-item {
    margin-bottom: 2rem;
  }

  .why-studied-point-title {
    font-size: 1.3rem;
  }

  .why-studied-point-text {
    font-size: 1rem;
  }

  .content-section {
    min-height: auto;
    min-height: auto;
    padding-top: 60px;
    padding-bottom: 60px;
  }

  .first-job-section .section-content {
    flex-direction: column;
    gap: 2rem;
  }

  .rectangle-28 {
    max-width: 100%;
    max-height: 300px;
  }

  .resizes-section {
    padding-bottom: 100px;
  }

  .resizes-tube-image .character-image {
    height: 40vh;
    max-height: 300px;
  }

  .dash-container {
    bottom: 40px;
    padding: 0 20px;
  }

  .dash-logo {
    max-width: 140px;
  }

  .resizes-logo {
    max-width: 250px;
  }

  .scroll-arrow-btn {
    bottom: 20px;
    width: 44px;
    height: 44px;
  }

  .arrow-icon {
    width: 20px;
    height: 20px;
  }
}

@media (max-width: 480px) {
  .section {
    padding: 40px 15px;
    min-height: auto;
    min-height: auto;
  }

  .masterclass-section,
  .who-i-am-section,
  .when-studied-section,
  .resizes-section {
    min-height: 100vh;
    min-height: 100dvh;
  }

  .main-title {
    font-size: 2rem;
    line-height: 1.1;
  }

  .section-title {
    font-size: 1.75rem;
    line-height: 1.2;
    margin-bottom: 0.75rem;
  }

  .section-subtitle {
    font-size: 1rem;
    margin-bottom: 1.5rem;
  }

  .subtitle {
    font-size: 1.1rem;
    margin-top: 0.75rem;
  }

  .character-image {
    height: 45vh;
    max-height: 350px;
  }

  .platonic-image {
    height: 35vh;
    max-height: 250px;
  }

  .where-worked-header {
    flex-direction: column;
    gap: 1.5rem;
  }

  #where-worked .section-content {
    max-width: 100%;
    padding: 0;
  }

  #where-worked {
    padding-top: 30px;
    padding-bottom: 30px;
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

  .steps-wrapper {
    gap: 2rem;
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
    line-height: 1.3;
  }

  .timeline-item {
    flex: 0 0 100%;
    min-width: 100%;
  }

  .timeline-content {
    width: 100%;
    padding: 0.9rem;
    margin-top: 15px;
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
    margin-bottom: 0.4rem;
  }

  .job-company {
    font-size: 0.85rem;
    margin-bottom: 0.3rem;
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
    max-height: 250px;
  }

  .profile-image-wrapper {
    width: 180px;
    height: 180px;
  }

  .profile-name {
    font-size: 1.3rem;
  }

  .profile-title {
    font-size: 1rem;
  }

  .why-studied-list {
    margin-top: 1.5rem;
  }

  .why-studied-item {
    margin-bottom: 1.5rem;
  }

  .why-studied-point-title {
    font-size: 1.2rem;
    margin-bottom: 0.5rem;
  }

  .why-studied-point-text {
    font-size: 0.95rem;
    line-height: 1.6;
  }

  .comparison-container {
    gap: 2rem;
    margin-top: 2rem;
  }

  .comparison-title {
    font-size: 1.5rem;
    margin-bottom: 1.25rem;
  }

  .comparison-item {
    margin-bottom: 2rem;
  }

  .comparison-point-title {
    font-size: 1.15rem;
  }

  .comparison-point-text {
    font-size: 0.95rem;
    line-height: 1.7;
  }

  .ellipse-img {
    height: 30vh;
    max-height: 200px;
  }

  .content-section {
    padding-top: 40px;
    padding-bottom: 40px;
  }

  .resizes-section {
    padding-bottom: 80px;
  }

  .resizes-tube-image .character-image {
    height: 35vh;
    max-height: 250px;
  }

  .resizes-logo {
    max-width: 200px;
  }

  .dash-logo {
    max-width: 120px;
  }

  .dash-container {
    bottom: 30px;
    padding: 0 15px;
  }

  .notification-image {
    height: 40px;
  }

  .minsait-logo {
    max-width: 150px;
  }

  .scroll-arrow-btn {
    bottom: 15px;
    width: 40px;
    height: 40px;
  }

  .arrow-icon {
    width: 18px;
    height: 18px;
  }

  .chronometer {
    bottom: 15px;
    left: 15px;
    padding: 6px 10px;
  }

  .chronometer-display {
    font-size: 0.9rem;
  }
}

/* Scroll Reveal Animations */
.scroll-reveal {
  opacity: 0;
  transform: translateY(24px);
  transition: opacity 0.6s cubic-bezier(0.16, 1, 0.3, 1),
              transform 0.6s cubic-bezier(0.16, 1, 0.3, 1);
}

.scroll-reveal.revealed {
  opacity: 1;
  transform: translateY(0);
}

.scroll-reveal:nth-child(2) { transition-delay: 0.08s; }
.scroll-reveal:nth-child(3) { transition-delay: 0.16s; }
.scroll-reveal:nth-child(4) { transition-delay: 0.24s; }
.scroll-reveal:nth-child(5) { transition-delay: 0.32s; }
.scroll-reveal:nth-child(6) { transition-delay: 0.40s; }

/* My Stack Section */
.stack-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2.5rem;
  margin-top: 2rem;
  max-width: 900px;
}

.stack-category {
  background: rgba(255, 255, 255, 0.8);
  border-radius: 12px;
  padding: 1.75rem;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
  transition: box-shadow 0.3s ease, transform 0.3s ease;
}

.stack-category:hover {
  box-shadow: 0 6px 24px rgba(0, 0, 0, 0.1);
  transform: translateY(-2px);
}

.stack-category-title {
  font-size: 1.1rem;
  font-weight: 700;
  color: #1a1a2e;
  margin-bottom: 1.25rem;
  letter-spacing: 0.02em;
  text-transform: uppercase;
  font-size: 0.85rem;
}

.stack-items {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}

.stack-item {
  display: flex;
  align-items: center;
  gap: 0.6rem;
  font-size: 0.95rem;
  font-weight: 500;
  color: #333;
  padding: 0.5rem;
  border-radius: 8px;
  transition: background 0.2s ease;
}

.stack-item:hover {
  background: rgba(26, 26, 46, 0.04);
}

.stack-icon {
  width: 28px;
  height: 28px;
  flex-shrink: 0;
  color: #1a1a2e;
  opacity: 0.7;
}

.stack-icon svg {
  width: 100%;
  height: 100%;
}

/* A Day as Full Stack Section */
.day-timeline {
  position: relative;
  margin-top: 2rem;
  max-width: 700px;
  padding-left: 1rem;
}

.day-timeline-line {
  position: absolute;
  left: 85px;
  top: 0;
  bottom: 0;
  width: 2px;
  background: linear-gradient(to bottom, #1a1a2e, #d3d3d3);
}

.day-item {
  display: flex;
  align-items: flex-start;
  gap: 1.5rem;
  margin-bottom: 2rem;
  position: relative;
}

.day-item:last-child {
  margin-bottom: 0;
}

.day-time {
  font-size: 1rem;
  font-weight: 700;
  color: #1a1a2e;
  min-width: 60px;
  text-align: right;
  padding-top: 0.1rem;
  font-variant-numeric: tabular-nums;
}

.day-marker {
  width: 14px;
  height: 14px;
  border-radius: 50%;
  background: #1a1a2e;
  border: 3px solid whitesmoke;
  box-shadow: 0 0 0 2px #1a1a2e;
  flex-shrink: 0;
  margin-top: 0.2rem;
  z-index: 2;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.day-item:hover .day-marker {
  transform: scale(1.3);
  box-shadow: 0 0 0 2px #1a1a2e, 0 0 12px rgba(26, 26, 46, 0.2);
}

.day-content {
  flex: 1;
}

.day-title {
  font-size: 1.2rem;
  font-weight: 700;
  color: #1a1a2e;
  margin: 0 0 0.35rem 0;
  line-height: 1.3;
}

.day-desc {
  font-size: 0.95rem;
  line-height: 1.6;
  color: #555;
  margin: 0;
}

/* Resizes About */
.resizes-about {
  margin-top: 1.5rem;
  max-width: 500px;
  text-align: center;
}

.resizes-description {
  font-size: 1rem;
  line-height: 1.7;
  color: #555;
  margin: 0 0 0.75rem 0;
}

.resizes-product {
  font-size: 0.95rem;
  line-height: 1.7;
  color: #777;
  margin: 0;
}

/* Responsive: My Stack */
@media (max-width: 768px) {
  .stack-grid {
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }

  .stack-category {
    padding: 1.25rem;
  }

  .stack-items {
    grid-template-columns: 1fr 1fr;
    gap: 0.75rem;
  }

  .stack-item {
    font-size: 0.9rem;
  }

  .stack-icon {
    width: 24px;
    height: 24px;
  }
}

@media (max-width: 480px) {
  .stack-items {
    grid-template-columns: 1fr;
  }

  .stack-category-title {
    font-size: 0.8rem;
  }
}

/* Responsive: My Day */
@media (max-width: 768px) {
  .day-timeline {
    padding-left: 0;
  }

  .day-timeline-line {
    left: 65px;
  }

  .day-time {
    font-size: 0.9rem;
    min-width: 50px;
  }

  .day-title {
    font-size: 1.05rem;
  }

  .day-desc {
    font-size: 0.9rem;
  }

  .day-item {
    gap: 1rem;
    margin-bottom: 1.5rem;
  }
}

@media (max-width: 480px) {
  .day-timeline-line {
    left: 55px;
  }

  .day-time {
    font-size: 0.85rem;
    min-width: 45px;
  }

  .day-title {
    font-size: 1rem;
  }

  .day-desc {
    font-size: 0.85rem;
  }

  .day-marker {
    width: 12px;
    height: 12px;
  }
}

/* Responsive: Resizes About */
@media (max-width: 768px) {
  .resizes-about {
    max-width: 100%;
  }

  .resizes-description {
    font-size: 0.95rem;
  }

  .resizes-product {
    font-size: 0.9rem;
  }
}

/* ── Generative Canvas Background ── */
.generative-bg {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  pointer-events: none;
  z-index: 0;
}

.section {
  z-index: 1;
  background: transparent;
}

@media (max-width: 768px) {
  .generative-bg {
    display: none;
  }
}

/* ── Radar Chart ── */
.stack-layout {
  display: flex;
  align-items: flex-start;
  gap: 3rem;
  margin-top: 2rem;
  max-width: 1100px;
}

/* Radial Hero */
#my-stack .text-content {
  max-width: 900px;
  width: 100%;
}

.radial-hero-wrapper {
  width: 100%;
  max-width: 900px;
  margin: 0 auto;
  max-height: 60vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.radial-hero {
  width: 100%;
  height: auto;
  max-height: 60vh;
}

.ring-tick {
  stroke: #1a1a2e;
  stroke-opacity: 0.16;
  stroke-width: 1.2;
}

.ring-cat-arc {
  stroke: #1a1a2e;
  stroke-opacity: 0.18;
  stroke-width: 1.5;
  fill: none;
  stroke-linecap: round;
  transition: stroke 0.3s ease, stroke-opacity 0.3s ease, stroke-width 0.3s ease;
}

.ring-cat-arc.active {
  stroke: #e8590c;
  stroke-opacity: 0.9;
  stroke-width: 2.5;
}

.ring-cat-arc.dimmed {
  stroke-opacity: 0.08;
}

.ring-cat-label {
  font-size: 12px;
  fill: #1a1a2e;
  fill-opacity: 0.55;
  font-family: 'Switzer', sans-serif;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 2px;
  transition: fill 0.3s ease, fill-opacity 0.3s ease;
}

.ring-cat-label.active {
  fill: #e8590c;
  fill-opacity: 1;
}

.ring-cat-label.dimmed {
  fill-opacity: 0.2;
}

.ring-item {
  transition: opacity 0.3s ease;
}

.ring-item.dimmed {
  opacity: 0.25;
}

.ring-item-tick {
  stroke: #e8590c;
  stroke-width: 7;
  stroke-linecap: round;
  transition: stroke-width 0.3s cubic-bezier(0.16, 1, 0.3, 1);
}

.ring-item-tick.active {
  stroke-width: 10;
}

.ring-card {
  fill: #fdfaf5;
  stroke: #e8590c;
  stroke-width: 1.25;
  transition: fill 0.3s ease, stroke-width 0.3s ease;
}

.ring-card.active {
  fill: #fdf0e6;
  stroke-width: 2;
}

.ring-card-text {
  font-size: 13px;
  fill: #1a1a2e;
  font-family: Georgia, 'Times New Roman', serif;
  font-style: italic;
}

@media (max-width: 768px) {
  .radial-hero-wrapper {
    max-width: 400px;
  }
}

@media (max-width: 480px) {
  .radial-hero-wrapper {
    max-width: 320px;
  }
}

/* ── Chatbox ── */
.chat-container {
  max-width: 520px;
  margin: 2.5rem auto 0;
  display: flex;
  flex-direction: column;
  gap: 0.6rem;
  z-index: 2;
  position: relative;
}

.chat-bubble {
  max-width: 82%;
  padding: 0.8rem 1.1rem;
  border-radius: 16px;
  font-size: 0.9rem;
  line-height: 1.55;
  opacity: 0;
  transform: translateY(10px);
  transition: opacity 0.4s ease, transform 0.4s ease;
}

.chat-bubble.chat-visible {
  opacity: 1;
  transform: translateY(0);
}

.chat-bubble.question {
  align-self: flex-start;
  background: rgba(26, 26, 46, 0.05);
  color: #666;
  border-bottom-left-radius: 4px;
}

.chat-bubble.answer {
  align-self: flex-end;
  background: rgba(26, 26, 46, 0.1);
  color: #1a1a2e;
  border-bottom-right-radius: 4px;
  font-weight: 500;
}

.chat-text {
  margin: 0;
}

.typing-indicator {
  display: flex;
  align-items: center;
  gap: 5px;
  padding: 0.8rem 1.1rem;
  opacity: 1 !important;
  transform: translateY(0) !important;
}

.typing-dot {
  width: 6px;
  height: 6px;
  background: #999;
  border-radius: 50%;
  display: inline-block;
  animation: typingBounce 1.2s ease-in-out infinite;
}

.typing-dot:nth-child(2) { animation-delay: 0.15s; }
.typing-dot:nth-child(3) { animation-delay: 0.3s; }

@keyframes typingBounce {
  0%, 60%, 100% { transform: translateY(0); opacity: 0.4; }
  30% { transform: translateY(-5px); opacity: 1; }
}

@media (max-width: 768px) {
  .chat-container {
    max-width: 100%;
    margin-top: 2rem;
  }

  .chat-bubble {
    max-width: 88%;
    font-size: 0.85rem;
  }
}

@media (max-width: 480px) {
  .chat-bubble {
    padding: 0.7rem 0.9rem;
    font-size: 0.82rem;
  }
}

/* Resizes Redesigned Layout */
.resizes-redesigned {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100%;
  position: relative;
  padding-bottom: 10vh;
}

.resizes-main-content {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  z-index: 2;
  text-align: center;
  gap: 4rem;
  width: 100%;
  max-width: 1100px;
}

.resizes-left-column {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  flex-shrink: 0;
}

.resizes-left-column .section-subtitle {
  margin-bottom: 0;
}

.chat-container-lg {
  max-width: 520px;
  width: 100%;
  margin-top: 0;
  max-height: 65vh;
  overflow-y: auto;
}

.chat-container-lg .chat-bubble {
  padding: 1rem 1.4rem;
  font-size: 1.05rem;
}

.chat-container-lg .chat-text {
  font-size: 1.05rem;
  line-height: 1.6;
}

.resizes-bottom-tagline {
  position: absolute;
  bottom: 80px;
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  max-width: min(1400px, 100vw);
  padding: 0 40px;
  box-sizing: border-box;
  text-align: center;
  z-index: 2;
}

.resizes-tagline-text {
  font-size: 0.95rem;
  color: #777;
  margin: 0;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

/* Closing Section */
.closing-message {
  font-size: 1.1rem;
  line-height: 1.8;
  color: #333;
  margin: 0 0 2rem 0;
  max-width: 600px;
}

.closing-links {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  max-width: 400px;
}

.closing-link {
  display: inline-flex;
  align-items: center;
  gap: 0.6rem;
  padding: 0.7rem 0;
  color: #1a1a2e;
  text-decoration: none;
  font-size: 0.95rem;
  font-weight: 500;
  border-bottom: 1px solid rgba(26, 26, 46, 0.1);
  transition: border-color 0.3s ease, padding-left 0.3s ease;
}

.closing-link:hover {
  border-color: rgba(26, 26, 46, 0.5);
  padding-left: 0.5rem;
}

.closing-link-icon {
  width: 18px;
  height: 18px;
  display: flex;
  align-items: center;
  opacity: 0.6;
}

.closing-link-icon svg {
  width: 100%;
  height: 100%;
}

.closing-footer {
  position: absolute;
  bottom: 30px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.8rem;
  color: rgba(26, 26, 46, 0.35);
  letter-spacing: 1px;
  text-transform: uppercase;
  z-index: 2;
}

.closing-separator {
  opacity: 0.4;
}

@media (max-width: 768px) {
  .resizes-main-content {
    flex-direction: column;
    gap: 0.5rem;
  }

  .chat-container-lg {
    margin-top: 1.5rem;
    max-height: 50vh;
  }

  .chat-container-lg .chat-bubble {
    padding: 0.85rem 1.1rem;
    font-size: 0.95rem;
  }

  .chat-container-lg .chat-text {
    font-size: 0.95rem;
  }

  .resizes-bottom-tagline {
    bottom: 20px;
    padding: 0 20px;
  }

  .resizes-tagline-text {
    white-space: normal;
    font-size: 0.85rem;
  }

}
</style>
