<script setup>
import { nextTick, onBeforeUnmount, ref } from 'vue'

defineProps({
  homePageUrl: {
    type: String,
    required: true,
  },
})

const emit = defineEmits(['back'])
const current = ref(0)
const isImageOpen = ref(false)
const imageButton = ref(null)
const closeButton = ref(null)
let previousBodyOverflow = ''
const screenshots = [
  {
    src: `${import.meta.env.BASE_URL}AutoFest/map.png`,
    alt: 'Carte interactive AutoFest avec les différentes zones de l’événement',
    caption: 'Carte interactive et informations sur les zones de l’événement.',
  },
  {
    src: `${import.meta.env.BASE_URL}AutoFest/panier.png`,
    alt: 'Panier de réservations et formulaire de paiement AutoFest',
    caption: 'Panier regroupant les achats et les réservations.',
  },
  {
    src: `${import.meta.env.BASE_URL}AutoFest/espace%20prestataire.png`,
    alt: 'Tableau de bord des ventes dans un espace prestataire.',
    caption: 'Espace prestataire et suivi des ventes.',
  },
  {
    src: `${import.meta.env.BASE_URL}AutoFest/tab%20de%20bord%20admin.png`,
    alt: 'Gestion des utilisateurs depuis le tableau de bord administrateur AutoFest',
    caption: 'Panel d’administration : gestion des utilisateurs et demande de prestation.',
  },
  {
    src: `${import.meta.env.BASE_URL}AutoFest/creer%20une%20prestation.png`,
    alt: 'Formulaire de création d’une prestation AutoFest',
    caption: 'Formulaire de création d’une prestation par un prestataire.',
  },
]

function openImage() {
  previousBodyOverflow = document.body.style.overflow
  document.body.style.overflow = 'hidden'
  isImageOpen.value = true
  nextTick(() => closeButton.value?.focus())
}

function closeImage() {
  if (!isImageOpen.value) return

  isImageOpen.value = false
  document.body.style.overflow = previousBodyOverflow
  nextTick(() => imageButton.value?.focus())
}

onBeforeUnmount(() => {
  if (isImageOpen.value) {
    document.body.style.overflow = previousBodyOverflow
  }
})

function next() {
  current.value = (current.value + 1) % screenshots.length
}

function previous() {
  current.value = (current.value - 1 + screenshots.length) % screenshots.length
}
</script>

<template>
  <main id="contenu" class="project-detail-page" @keydown.esc.window="closeImage">
    <article class="project-detail container">
      <a class="back-link" :href="homePageUrl" @click.prevent="emit('back')">&lt; Retour aux projets</a>

      <p class="eyebrow project-meta">SAÉ universitaire · Septembre 2025 — mars 2026</p>
      <h1>AutoFest</h1>

      <section class="project-context">
        <h2>Contexte</h2>
        <p>
          Ce projet a été réalisé <strong>en équipe de 5</strong>, durant l'intégralité de ma deuxième année de BUT Informatique, et consistait à concevoir une <strong>application web complète</strong>
          pour un <strong>rassemblement de véhicules</strong> organisé sur un circuit. L’événement réunissait de
          nombreuses activités et plusieurs prestataires : karting, motocross, courses, concert, stands
          de nourriture, parc d’exposition, garage et postes de secours.
        </p>
      </section>

      <section class="carousel" aria-roledescription="carrousel" aria-label="Captures d’écran du projet AutoFest">
        <div class="carousel-stage">
          <Transition name="carousel-fade" mode="out-in">
            <figure :key="current" class="carousel-slide">
              <button
                ref="imageButton"
                class="carousel-image-button"
                type="button"
                aria-label="Afficher l’image en grand format"
                @click="openImage"
              >
                <img :src="screenshots[current].src" :alt="screenshots[current].alt" />
              </button>
              <figcaption>{{ screenshots[current].caption }}</figcaption>
            </figure>
          </Transition>
          <div class="carousel-arrows">
            <button class="carousel-button" type="button" aria-label="Image précédente" @click="previous">←</button>
            <button class="carousel-button" type="button" aria-label="Image suivante" @click="next">→</button>
          </div>
        </div>
        <p class="carousel-count" aria-live="polite">{{ current + 1 }} / {{ screenshots.length }}</p>
      </section>

      <div class="project-copy">
        <section>
          <h2>Fonctionnalités</h2>
          <p>
            Une <strong>carte interactive</strong> permet aux visiteurs de découvrir les informations des prestataires
            en survolant les différentes zones, puis d’accéder à leur espace. Chaque prestataire peut
            proposer des services au public, comme un <strong>livre d’or</strong>, un <strong>formulaire de contact</strong> ou la
            <strong>réservation d’une séance</strong>.
          </p>
          <p>
            Les visiteurs peuvent ajouter des achats et des réservations à un <strong>panier</strong>, puis les régler
            grâce au <strong>système de paiement</strong>. Les prestataires disposent d’un <strong>espace dédié</strong> pour consulter
            leurs <strong>statistiques et leurs ventes</strong>, tandis que l’administrateur gère l’événement depuis un
            <strong>panel d’administration</strong>.
          </p>
          <p>
            L’application gère plusieurs rôles : <strong>visiteur, prestataire et administrateur</strong>, avec une
            <strong>authentification</strong> et des menus adaptés à l’utilisateur connecté.
          </p>
        </section>
        <section>
          <h2>Mon rôle</h2>
          <p>
            J’ai participé avec le groupe à toutes les étapes du projet : <strong>conception de la base de
            données</strong>, création de l’<strong>API avec Node.js</strong>, développement du <strong>front-end avec Vue.js</strong> et mise en
            place de l’<strong>authentification</strong>. Nous avons réparti le travail et suivi l’avancement avec une
            <strong>méthode agile</strong>, organisée en <strong>sprints de deux semaines</strong>.
          </p>
        </section>
        <section>
          <h2>Ce que j’ai appris</h2>
          <p>
            Ce projet m’a appris à concevoir une <strong>application web complète</strong>, du front-end au back-end,
            à <strong>créer et documenter une API</strong>, et à relier une <strong>base de données</strong> à un serveur Node.js. J’ai
            également appris à mettre en place une <strong>authentification</strong>, un <strong>panier</strong> et un <strong>système de
            paiement</strong>, tout en <strong>travaillant en équipe</strong>.
          </p>
        </section>
        <section>
          <h2>Technologies utilisées</h2>
          <div class="tag-list" aria-label="Technologies utilisées">
            <span>Vue.js</span><span>Node.js</span><span>API REST</span><span>SQL</span><span>Trello</span><span>Git</span>
          </div>
        </section>
      </div>
    </article>

    <div
      v-if="isImageOpen"
      class="image-lightbox"
      role="dialog"
      aria-modal="true"
      :aria-label="screenshots[current].alt"
      @click.self="closeImage"
    >
      <button
        ref="closeButton"
        class="image-lightbox-close"
        type="button"
        aria-label="Fermer l’image agrandie"
        @click="closeImage"
      >
        ×
      </button>
      <figure class="image-lightbox-figure">
        <img :src="screenshots[current].src" :alt="screenshots[current].alt" />
        <figcaption>{{ screenshots[current].caption }}</figcaption>
      </figure>
    </div>
  </main>
</template>