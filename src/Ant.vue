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
    src: `${import.meta.env.BASE_URL}ANT/home%20ANT.png`,
    alt: 'Accueil d’ANT avec la liste des workflows disponibles',
    caption: 'Accueil de l’application ANT et liste des workflows.',
  },
  {
    src: `${import.meta.env.BASE_URL}ANT/sh%C3%A9ma%20workflow%20modif%20dimension%20d%20image.png`,
    alt: 'Schéma illustrant les images sources et le résultat redimensionné par l’intelligence artificielle',
    caption: 'Schéma du workflow de modification de dimension de l’image avec l’IA.',
  },
  {
    src: `${import.meta.env.BASE_URL}ANT/workflow%20modif%20dimension%20image.png`,
    alt: 'Workflow N8N de traitement d’image avec OpenAI Image',
    caption: 'Workflow N8N de modification de dimension de l’image et appel à l’API OpenAI.',
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
      <a class="back-link" :href="homePageUrl" @click.prevent="emit('back')"> < Retour aux projets</a>

      <p class="eyebrow project-meta">Stage · Avril — juin 2026</p>
      <h1>ANT</h1>
      <p class="project-intro">Application d’automatisation de workflows</p>

      <section class="project-context">
        <h2>Contexte</h2>
        <p>
          Réalisé lors de mon <strong>stage de 8 semaines</strong>, d’avril à juin 2026, au sein de la <strong>direction
          des systèmes d’information (DSI)</strong> de <strong>Vetoquinol</strong>, entreprise leader dans le domaine de la santé animale, située à Magny Vernoy. Ce projet avait pour objectif d’<strong>automatiser des tâches</strong> pour les équipes et de
          leur proposer un outil simple pour lancer ces automatisations.
        </p>
      </section>

      <section class="carousel" aria-roledescription="carrousel" aria-label="Captures d’écran du projet ANT">
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
            J’ai développé <strong>deux workflows</strong> sur <strong>N8N</strong>, une plateforme de création de workflows sans code.
            Le premier <strong>redimensionne des images</strong> pour le service marketing en faisant appel à une
            <strong>intelligence artificielle</strong> via une <strong>API REST</strong>. Le second génère automatiquement un <strong>tableau
            Excel</strong> comparant les produits de l’entreprise à ceux de ses concurrents.
          </p>
          <p>
            J’ai également développé <strong>ANT (Automated N8N Taskhub)</strong> et crée son <strong>cahier des charges</strong>. ANT est une application qui permet de lancer
            ces workflows <strong>de façon sécurisée</strong>, de suivre <strong>qui</strong> les a exécutés et <strong>à quel moment</strong>, puis de
            récupérer directement leurs résultats. Elle pourra accueillir d’autres workflows par la
            suite.
          </p>
        </section>
        <section>
          <h2>Mon rôle</h2>
          <p>
            Avec l’accompagnement de mon tuteur, j’ai <strong>conçu et développé l’application ANT</strong> et créé
            les <strong>workflows N8N</strong>.
          </p>
        </section>
        <section>
          <h2>Ce que j’ai appris</h2>
          <p>
            Ce stage m’a appris à travailler au sein d’une <strong>équipe informatique en entreprise</strong>, à
            développer une application avec <strong>Symfony et Twig</strong>, à automatiser des processus avec <strong>N8N</strong> et
            à communiquer avec une intelligence artificielle via une <strong>API REST</strong>. J’ai aussi appris à
            concevoir une solution répondant à un <strong>besoin concret</strong> exprimé par des utilisateurs et à crée son cahier des charges.
          </p>
        </section>
        <section>
          <h2>Technologies utilisées</h2>
          <div class="tag-list" aria-label="Technologies utilisées">
            <span>PHP</span><span>Symfony</span><span>Twig</span><span>phpMyAdmin</span><span>N8N</span><span>API REST</span>
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