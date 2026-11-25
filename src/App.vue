<script setup>
import { ref } from 'vue'

const menuOpen = ref(false)
const isSending = ref(false)
const formFeedback = ref('')
const hasFormError = ref(false)
const cvUrl = '/Portfolio/CUINET_Martin_CV.pdf'
const photoUrl = `${import.meta.env.BASE_URL}Photo_CV.jpg`

const projects = [
  {
    title: 'Projet de stage ou d’apprentissage',
    type: 'Expérience professionnelle',
    description:
      'Présentez ici le contexte, votre rôle et la solution réalisée. Expliquez simplement le besoin auquel le projet répond.',
    technologies: ['Vue.js', 'JavaScript', 'Git'],
    link: '#contact',
  },
  {
    title: 'Application web universitaire',
    type: 'Projet étudiant',
    description:
      'Décrivez un projet concret : ses fonctionnalités principales, les utilisateurs concernés et les résultats obtenus.',
    technologies: ['HTML', 'CSS', 'API REST'],
    link: '#contact',
  },
  {
    title: 'Projet personnel',
    type: 'Projet personnel',
    description:
      'Mettez en avant une réalisation qui montre votre curiosité, votre autonomie et votre façon de résoudre un problème.',
    technologies: ['Vue.js', 'Responsive design', 'Figma'],
    link: '#contact',
  },
]

const skills = [
  { name: 'HTML / CSS', level: 'Solide' },
  { name: 'JavaScript / Vue.js', level: 'En progression' },
  { name: 'Git / GitHub', level: 'Solide' },
  { name: 'Conception responsive', level: 'Solide' },
  { name: 'Méthode agile', level: 'En progression' },
  { name: 'Communication', level: 'Solide' },
]

function closeMenu() {
  menuOpen.value = false
}

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
      <a class="brand" href="#accueil" @click="closeMenu">Martin <span>CUINET</span></a>
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
        <a href="#a-propos" @click="closeMenu">À propos</a>
        <a href="#projets" @click="closeMenu">Projets</a>
        <a href="#competences" @click="closeMenu">Compétences</a>
        <a href="#parcours" @click="closeMenu">Parcours</a>
        <a href="#contact" @click="closeMenu">Contact</a>
      </div>
    </nav>
  </header>

  <main id="contenu">
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
          <p>Trois projets bien présentés valent mieux qu’une longue liste.</p>
        </div>
        <div class="project-grid">
          <article v-for="(project, index) in projects" :key="project.title" class="project-card">
            <div class="project-number">0{{ index + 1 }}</div>
            <p class="card-label">{{ project.type }}</p>
            <h3>{{ project.title }}</h3>
            <p>{{ project.description }}</p>
            <div class="tag-list">
              <span v-for="technology in project.technologies" :key="technology">{{ technology }}</span>
            </div>
            <a class="text-link" :href="project.link">Voir le projet <span aria-hidden="true">↗</span></a>
          </article>
        </div>
      </div>
    </section>

    <section id="competences" class="section section-dark">
      <div class="container two-columns">
        <div>
          <p class="eyebrow eyebrow-light">03 · Compétences</p>
          <h2>Ce que je peux apporter à votre équipe.</h2>
          <p class="muted">Les niveaux sont indicatifs : l’important est de continuer à apprendre.</p>
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
          <p class="form-note">Les informations saisies sont utilisées uniquement pour répondre.</p>
          <p v-if="formFeedback" class="form-note form-feedback" :class="{ 'form-error': hasFormError }" role="status" aria-live="polite">
            {{ formFeedback }}
          </p>
        </form>
      </div>
    </section>
  </main>

  <footer class="footer">
    <div class="container footer-content">
      <span>© 2026 Martin CUINET</span>
      <a href="#accueil">Retour en haut ↑</a>
    </div>
  </footer>
</template>
