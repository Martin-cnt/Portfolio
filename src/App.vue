<script setup>
import { onBeforeUnmount, onMounted, ref } from 'vue'
import Projet from './Projet.vue'
import Quixo from './Quixo.vue'
import AutoFest from './AutoFest.vue'
import Ant from './Ant.vue'

function getActiveProjectFromLocation() {
  const project = new URLSearchParams(window.location.search).get('projet')
  return project === 'jean-xtreme' || project === 'quixo' || project === 'autofest' || project === 'ant'
    ? project
    : null
}

const menuOpen = ref(false)
const isSending = ref(false)
const formFeedback = ref('')
const hasFormError = ref(false)
const activeProject = ref(getActiveProjectFromLocation())
const cvUrl = '/Portfolio/CUINET_Martin_CV.pdf'
const photoUrl = `${import.meta.env.BASE_URL}Photo_CV.jpg`
const projectImageUrl = `${import.meta.env.BASE_URL}JNX/jnx.jpg`
const quixoImageUrl = `${import.meta.env.BASE_URL}Quixo/QuixoImage.jpg`
const autoFestLogoUrl = `${import.meta.env.BASE_URL}AutoFest/logo.png`
const antLogoUrl = `${import.meta.env.BASE_URL}ANT/logo_ant.png`
const homePageUrl = window.location.pathname
let projectHistoryEntry = Boolean(window.history.state && window.history.state.portfolioProject)

const skills = [
  { name: 'Développement web', level: 'Vue.js · Node.js · JavaScript · HTML · CSS' },
  { name: 'Programmation', level: 'Java · Python · C · PHP' },
  { name: 'Bases de données', level: 'MySQL · phpMyAdmin · MongoDB' },
  { name: 'Outils', level: 'Git · GitHub · N8N · IntelliJ · Docker' },
  { name: 'Gestion de projet', level: 'Cahier des charges · Trello' },
  { name: 'Langues', level: 'Anglais (B2)' },
]

function closeMenu() {
  menuOpen.value = false
}

function sectionHref(section) {
  return activeProject.value ? `${homePageUrl}#${section}` : `#${section}`
}

function openProject(project) {
  const url = new URL(window.location.href)
  url.searchParams.set('projet', project)
  url.hash = ''
  window.history.pushState({ portfolioProject: true }, '', url)
  projectHistoryEntry = true
  activeProject.value = project
  window.scrollTo(0, 0)
}

function syncProjectFromLocation(event) {
  activeProject.value = getActiveProjectFromLocation()
  projectHistoryEntry = Boolean(event.state && event.state.portfolioProject)
}

function returnToProjects() {
  if (projectHistoryEntry) {
    window.history.back()
    return
  }

  window.history.replaceState({}, '', homePageUrl)
  activeProject.value = null
  requestAnimationFrame(() => document.getElementById('projets')?.scrollIntoView())
}

function goToHome() {
  window.history.replaceState({}, '', homePageUrl)
  activeProject.value = null
  window.scrollTo(0, 0)
}

onMounted(() => window.addEventListener('popstate', syncProjectFromLocation))
onBeforeUnmount(() => window.removeEventListener('popstate', syncProjectFromLocation))

async function sendMessage(event) {
  const form = event.target
  const formData = Object.fromEntries(new FormData(form).entries())

  isSending.value = true
  formFeedback.value = ''
  hasFormError.value = false

  try {
    const response = await fetch('https://formsubmit.co/ajax/martincuinet2006@gmail.com', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        Accept: 'application/json',
      },
      body: JSON.stringify(formData),
    })
    const result = await response.json()

    const submissionAccepted =
      result.success === true || result.success === 'true' || result.ok === true

    if (!response.ok || !submissionAccepted) {
      throw new Error(result.message || 'Le service de messagerie a refusé le message.')
    }

    form.reset()
    formFeedback.value = 'Message envoyé avec succès.'
  } catch (error) {
    console.error("Erreur lors de l'envoi du formulaire :", error)
    hasFormError.value = true
    formFeedback.value = 'Échec de l’envoi. Vérifier la connexion et réessayer.'
  } finally {
    isSending.value = false
  }
}
</script>

<template>
  <header class="site-header">
    <nav class="nav container" aria-label="Navigation principale">
      <a v-if="activeProject" class="brand" :href="homePageUrl" @click.prevent="goToHome">Martin <span>CUINET</span></a>
      <a v-else class="brand" href="#accueil" @click="closeMenu">Martin <span>CUINET</span></a>
      <button
        class="menu-button"
        type="button"
        :aria-expanded="menuOpen"
        aria-controls="menu"
        @click="menuOpen = !menuOpen"
      >
        <span class="sr-only">Ouvrir le menu</span>
        <span aria-hidden="true">☰</span>
      </button>
      <div id="menu" class="nav-links" :class="{ 'is-open': menuOpen }">
        <a :href="sectionHref('a-propos')" @click="closeMenu">À propos</a>
        <a :href="sectionHref('projets')" @click="closeMenu">Projets</a>
        <a :href="sectionHref('competences')" @click="closeMenu">Compétences</a>
        <a :href="sectionHref('parcours')" @click="closeMenu">Parcours</a>
        <a :href="sectionHref('contact')" @click="closeMenu">Contact</a>
      </div>
    </nav>
  </header>

  <Projet
    v-if="activeProject === 'jean-xtreme'"
    :home-page-url="homePageUrl"
    @back="returnToProjects"
  />
  <Quixo
    v-else-if="activeProject === 'quixo'"
    :home-page-url="homePageUrl"
    @back="returnToProjects"
  />
  <AutoFest
    v-else-if="activeProject === 'autofest'"
    :home-page-url="homePageUrl"
    @back="returnToProjects"
  />
  <Ant
    v-else-if="activeProject === 'ant'"
    :home-page-url="homePageUrl"
    @back="returnToProjects"
  />

  <main v-else id="contenu">
    <section id="accueil" class="hero-section">
      <div class="container hero-grid">
        <div>
          <h1>
            <span class="hero-name">Martin Cuinet</span>
            <em>Étudiant en informatique</em>
          </h1>
          <p class="hero-text">
            <strong>Bienvenue sur mon portfolio !</strong> J'aime développer des applications, des sites web et des programmes qui répondent à un vrai besoin.
          </p>
          <div class="actions">
            <a class="button button-primary" href="#projets">Découvrir mes projets</a>
            <a class="button button-secondary" :href="cvUrl" download>Télécharger mon CV <span aria-hidden="true">↓</span></a>
          </div>
        </div>
        <div class="hero-card" aria-label="Présentation rapide">
          <div class="avatar">
            <img :src="photoUrl" alt="Photo de Martin Cuinet" />
          </div>
          <div class="profile-facts" aria-label="Quelques informations sur moi">
            <p class="profile-fact">
              <span class="profile-icon" aria-hidden="true"><span class="profile-icon-glyph">🏠</span></span>
              <span><small>Ville</small>Belfort</span>
            </p>
            <p class="profile-fact">
              <span class="profile-icon" aria-hidden="true"><span class="profile-icon-glyph">🎓</span></span>
              <span><small>Études</small>BUT informatique</span>
            </p>
            <p class="profile-fact">
              <span class="profile-icon" aria-hidden="true"><span class="profile-icon-glyph">🎮</span></span>
              <span><small>Centres d’intérêt</small>Football · Jeux vidéo · Jeux de société</span>
            </p>
          </div>
        </div>
      </div>
    </section>

    <section id="a-propos" class="section section-light">
      <div class="container two-columns">
        <div class="etape2">
          <p class="eyebrow">01 · À propos</p>
          <h2>Un peu plus sur moi</h2>
        </div>
        <div class="prose">
          <p>
            Étudiant en <strong>3ᵉ année de BUT Informatique</strong>, j'aime comprendre un besoin et le transformer en <strong>application</strong> ou en site<strong> simple à utiliser</strong>. 
            Je fais attention à la <strong>qualité du code</strong> et à <strong>l'accessibilité</strong>.
          </p>
          <p>
            Mon <strong>stage</strong> m'a permis de développer mes <strong>compétences techniques</strong>, ainsi que mon sens du <strong>travail en équipe</strong> et ma capacité à <strong>m'adapter</strong> à la vie en entreprise.
          </p>
          <p>
            Passionné par les <strong>nouvelles technologies</strong>, je me tiens au courant des actualités du numérique et de l'informatique pour rester à jour sur les évolutions du secteur.
          </p>
          <h4 class="quality">Mes qualités</h4>
          <div class="keywords" aria-label="Mots-clés">
            <span>Rigueur</span><span>Autonomie  </span><span>Esprit d'équipe</span><span>Adaptation</span>
          </div>
        </div>
      </div>
    </section>

    <section id="projets" class="section">
      <div class="container">
        <div class="section-heading">
          <div>
            <p class="eyebrow">02 · Projets</p>
            <h2>Quelques réalisations</h2>
          </div>
          <p>Découvrez les projets auxquels j’ai contribué.</p>
        </div>
        <div class="project-grid">
          <a
            class="project-card"
            :href="`${homePageUrl}?projet=jean-xtreme`"
            aria-label="Découvrir le projet Jean Xtreme"
            @click.prevent="openProject('jean-xtreme')"
          >
            <img class="project-card-image" :src="projectImageUrl" alt="" />
            <h3>Jean Xtreme</h3>
          </a>
          <a
            class="project-card"
            :href="`${homePageUrl}?projet=quixo`"
            aria-label="Découvrir le projet Quixo"
            @click.prevent="openProject('quixo')"
          >
            <img class="project-card-image project-card-image-contain" :src="quixoImageUrl" alt="" />
            <h3>Quixo</h3>
          </a>
          <a
            class="project-card"
            :href="`${homePageUrl}?projet=autofest`"
            aria-label="Découvrir le projet AutoFest"
            @click.prevent="openProject('autofest')"
          >
            <img class="project-card-image autofest-card-image" :src="autoFestLogoUrl" alt="" />
            <h3>AutoFest</h3>
          </a>
          <a
            class="project-card"
            :href="`${homePageUrl}?projet=ant`"
            aria-label="Découvrir le projet ANT"
            @click.prevent="openProject('ant')"
          >
            <img class="project-card-image ant-card-image" :src="antLogoUrl" alt="" />
            <h3>ANT</h3>
          </a>
        </div>
      </div>
    </section>

    <section id="competences" class="section section-dark">
      <div class="container two-columns">
        <div>
          <p class="eyebrow eyebrow-light">03 · Compétences</p>
          <h2>Mes compétences</h2>
          <p class="muted">Compétences acquises en cours, en projet et en stage.</p>
        </div>
        <div class="skills-list">
          <div v-for="skill in skills" :key="skill.name" class="skill">
            <span>{{ skill.name }}</span><small>{{ skill.level }}</small>
          </div>
        </div>
      </div>
    </section>

    <section id="parcours" class="section section-light">
      <div class="container two-columns">
        <div>
          <p class="eyebrow">04 · Parcours</p>
          <h2>Ma formation</h2>
        </div>
        <div class="timeline">
          <article>
            <span>2024 — 2027</span>
            <h3>BUT Informatique</h3>
            <p><strong>IUT Nord Franche-Comté · Belfort</strong>
              <br><em>Parcours A : Réalisation d’applications, coloration C</em></p>
          </article>
          <article>
            <span>2021 — 2024</span>
            <h3>Baccalauréat général</h3>
            <p><strong>Lycée Louis Aragon · Héricourt</strong>
              <br><em>Spécialités mathématiques et Anglais monde contemporain (AMC).</em></p>
          </article>
        </div>
      </div>
    </section>

    <section id="contact" class="section contact-section">
      <div class="container contact-grid">
        <div class="contact-info">
          <p class="eyebrow">05 · Contact</p>
          <h2>Une question ?<br /><em>Contactez-moi</em></h2>
          <p class="contact-intro">Je répondrai avec plaisir à votre message.</p>
          <a class="email-link" href="mailto:martincuinet2006@gmail.com">martincuinet2006@gmail.com</a>
          <h4 class="contact-reseau">Mes réseaux</h4>
          <div class="social-links">
            <a href="https://github.com/Martin-cnt" target="_blank" rel="noreferrer">GitHub ↗</a>
            <a href="https://www.linkedin.com/in/martin-cuinet-bbba313bb/" target="_blank" rel="noreferrer">LinkedIn ↗</a>
          </div>
        </div>
        <form class="contact-form" @submit.prevent="sendMessage">
          <label for="name">Nom Prénom</label>
          <input id="name" name="name" type="text" autocomplete="name" placeholder="Votre nom prénom" required />
          <label for="email">Adresse e-mail</label>
          <input id="email" name="email" type="email" autocomplete="email" placeholder="vous@email.com" required />
          <label for="subject">Objet</label>
          <input id="subject" name="_subject" type="text" placeholder="Objet de votre message" required />
          <label for="message">Message</label>
          <textarea id="message" name="message" rows="4" placeholder="Votre message..." required></textarea>
          <button class="button button-primary" type="submit" :disabled="isSending">
            {{ isSending ? 'Envoi en cours…' : 'Envoyer le message' }}
            <span aria-hidden="true">↗</span>
          </button>
          <p v-if="formFeedback" class="form-note form-feedback" :class="{ 'form-error': hasFormError }" role="status" aria-live="polite">
            {{ formFeedback }}
          </p>
          <p class="form-note">Les informations saisies sont utilisées uniquement pour répondre.</p>
          
        </form>
      </div>
    </section>
  </main>

  <footer class="footer">
    <div class="container footer-content">
      <span>© 2026 Martin CUINET</span>
      <a v-if="activeProject" :href="homePageUrl" @click.prevent="goToHome">Retour à l’accueil ↑</a>
      <a v-else href="#accueil">Retour en haut ↑</a>
    </div>
  </footer>
</template>
