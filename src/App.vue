<script setup>
import { computed, onMounted, onUnmounted, ref } from 'vue'

const pointer = ref({ x: 50, y: 35 })
const activeProcess = ref(0)
const visibleSections = ref(new Set())

const projects = [
  {
    number: '01',
    title: 'Marketio',
    category: 'Web application',
    status: 'Built project',
    description: 'An online e-commerce platform built independently from front end to back end.',
    link: 'https://github.com/SoufianeAbk/Marketio',
    accent: 'lime'
  },
  {
    number: '02',
    title: 'Boekenwinkel',
    category: 'Web application',
    status: 'Built project',
    description: 'A bookstore where customers can discover, order and review books.',
    link: 'https://github.com/SoufianeAbk/boekenwinkel',
    accent: 'blue'
  }
]

const processSteps = [
  { label: 'Understand', text: 'I start by clarifying the goal, the user and the problem worth solving.' },
  { label: 'Plan', text: 'I break the challenge into practical steps, choosing a clear technical direction.' },
  { label: 'Build', text: 'I turn the plan into a working solution, from interface to implementation.' },
  { label: 'Improve', text: 'I review the result, learn from it and look for the next improvement.' }
]

const heroStyle = computed(() => ({
  '--pointer-x': `${pointer.value.x}%`,
  '--pointer-y': `${pointer.value.y}%`
}))

function updatePointer(event) {
  pointer.value = {
    x: (event.clientX / window.innerWidth) * 100,
    y: (event.clientY / window.innerHeight) * 100
  }
}

function setActiveProcess(index) {
  activeProcess.value = index
}

function observeSections() {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) visibleSections.value = new Set([...visibleSections.value, entry.target.id])
      })
    },
    { threshold: 0.16 }
  )
  document.querySelectorAll('[data-reveal]').forEach((section) => observer.observe(section))
  return observer
}

let sectionObserver
onMounted(() => {
  window.addEventListener('pointermove', updatePointer)
  sectionObserver = observeSections()
})
onUnmounted(() => {
  window.removeEventListener('pointermove', updatePointer)
  sectionObserver?.disconnect()
})
</script>

<template>
  <div id="top" class="site-shell">
    <a class="skip-link" href="#main-content">Skip to content</a>
    <header class="site-header">
      <a class="brand" href="#top" aria-label="Back to top">
        <span class="brand-mark">AS</span>
        <span>Abakkiou Soufiane</span>
      </a>
      <nav class="main-nav" aria-label="Main navigation">
        <a href="#about">About</a>
        <a href="#process">Process</a>
        <a href="#projects">Projects</a>
        <a href="#contact">Contact</a>
      </nav>
      <a class="header-link" href="https://www.linkedin.com/in/soufiane-abakkiou-b2b9772a3" target="_blank" rel="noreferrer">
        LinkedIn <span aria-hidden="true">↗</span>
      </a>
    </header>

    <main id="main-content">
      <section class="hero" :style="heroStyle" aria-labelledby="hero-title">
        <div class="hero-grid" aria-hidden="true"></div>
        <div class="hero-glow" aria-hidden="true"></div>
        <div class="hero-content">
          <p class="eyebrow"><span class="pulse-dot"></span> Portfolio / 2025</p>
          <h1 id="hero-title">Building useful<br /><em>things, thoughtfully.</em></h1>
          <p class="hero-intro">I’m Abakkiou Soufiane, a junior IT professional focused on turning challenges into practical digital solutions.</p>
          <div class="hero-actions">
            <a class="button button-primary" href="#projects">Explore my work <span>↓</span></a>
            <a class="text-link" href="#process">How I work <span>↘</span></a>
          </div>
        </div>
        <div class="hero-orbit orbit-one" aria-hidden="true"><span></span></div>
        <div class="hero-orbit orbit-two" aria-hidden="true"><span></span></div>
        <div class="hero-meta">
          <span>01 — 04</span>
          <span>Scroll to explore <strong>↓</strong></span>
        </div>
      </section>

      <section id="about" class="intro-section section-pad" data-reveal :class="{ 'is-visible': visibleSections.has('about') }">
        <div class="section-label"><span>01</span><span>About me</span></div>
        <div class="intro-copy">
          <p class="section-kicker">Curious by nature. Precise by practice.</p>
          <h2>A motivated junior professional with a <span>builder’s mindset.</span></h2>
          <p class="body-copy">I have a passion for software development, cybersecurity and new technologies. With knowledge of multiple programming languages, I enjoy creating innovative, practical solutions. I’m eager to keep developing my technical skills within a professional IT environment.</p>
        </div>
      </section>

      <section id="process" class="process-section section-pad" data-reveal :class="{ 'is-visible': visibleSections.has('process') }">
        <div class="section-label"><span>02</span><span>My process</span></div>
        <div class="process-heading">
          <h2>The work behind<br /><span>the work.</span></h2>
          <p>Recruiters should see more than the final screen. This is how I approach a new challenge.</p>
        </div>
        <div class="process-list">
          <button v-for="(step, index) in processSteps" :key="step.label" type="button" class="process-item" :class="{ active: activeProcess === index }" :aria-pressed="activeProcess === index" @mouseenter="setActiveProcess(index)" @focus="setActiveProcess(index)" @click="setActiveProcess(index)">
            <span class="process-number">0{{ index + 1 }}</span>
            <span class="process-name">{{ step.label }}</span>
            <span class="process-description">{{ step.text }}</span>
            <span class="process-arrow" aria-hidden="true">↗</span>
          </button>
        </div>
      </section>

      <section id="projects" class="projects-section section-pad" data-reveal :class="{ 'is-visible': visibleSections.has('projects') }">
        <div class="section-label"><span>03</span><span>Selected work</span></div>
        <div class="projects-heading">
          <h2>Projects with a<br /><span>purpose.</span></h2>
          <p>Real builds, honest descriptions, and a focus on the decisions that make a solution useful.</p>
        </div>
        <div class="project-grid">
          <article v-for="project in projects" :key="project.title" class="project-card" :class="project.accent">
            <div class="project-visual">
              <div class="visual-window">
                <div class="window-bar"><span></span><span></span><span></span><b>{{ project.number }}</b></div>
                <div class="visual-lines"><i></i><i></i><i></i><i></i></div>
                <div class="visual-block"></div>
              </div>
              <span class="project-number">{{ project.number }}</span>
            </div>
            <div class="project-info">
              <div class="project-topline"><span>{{ project.category }}</span><span>{{ project.status }}</span></div>
              <h3>{{ project.title }}</h3>
              <p>{{ project.description }}</p>
              <a class="project-link" :href="project.link" target="_blank" rel="noreferrer">View on GitHub <span>↗</span></a>
            </div>
          </article>
        </div>
      </section>

      <section class="skills-section section-pad" data-reveal :class="{ 'is-visible': visibleSections.has('skills') }" id="skills">
        <div class="section-label"><span>04</span><span>Toolkit</span></div>
        <div class="skills-content">
          <h2>Always learning.<br /><span>Always moving.</span></h2>
          <div class="skill-groups">
            <div><span class="skill-label">Technical</span><p>Python <b>·</b> .NET <b>·</b> Java</p></div>
            <div><span class="skill-label">Human</span><p>Teamwork <b>·</b> Communication <b>·</b> Problem-solving</p></div>
          </div>
        </div>
      </section>

      <section id="contact" class="contact-section section-pad" data-reveal :class="{ 'is-visible': visibleSections.has('contact') }">
        <p class="eyebrow"><span class="pulse-dot"></span> Let’s connect</p>
        <h2>Have a challenge<br /><em>in mind?</em></h2>
        <a class="contact-link" href="https://www.linkedin.com/in/soufiane-abakkiou-b2b9772a3" target="_blank" rel="noreferrer">Say hello on LinkedIn <span>↗</span></a>
      </section>
    </main>
    <footer><span>© 2026 Abakkiou Soufiane</span><span>Professional portfolio</span><a href="#top">Back to top ↑</a></footer>
  </div>
</template>
