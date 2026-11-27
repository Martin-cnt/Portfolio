<script setup>
import { nextTick, onBeforeUnmount, ref } from 'vue'

defineProps({
  homePageUrl: {
    type: String,
    required: true,
  },
})

const emit = defineEmits(['back'])
const githubProjectUrl = 'https://github.com/Natan90/Quixo'
const images = [
  {
    src: `${import.meta.env.BASE_URL}Quixo/QuixoImage.jpg`,
    alt: 'Cubes et plateau du jeu Quixo',
    description: 'Le jeu de plateau Quixo.',
  },
  {
    src: `${import.meta.env.BASE_URL}Quixo/comm.png`,
    alt: 'Plateau Quixo affiché sans la version graphique du jeu',
    description: 'Le plateau de jeu sans interface graphique avec les coordonnées et les pions X et O.',
  },
  {
    src: `${import.meta.env.BASE_URL}Quixo/Board.png`,
    alt: 'Interface graphique du jeu Quixo',
    description: 'L’interface graphique du jeu Quixo.',
  },
  {
    src: `${import.meta.env.BASE_URL}Quixo/QuixoComm.png`,
    alt: 'Score des IA affiché dans la version console du jeu Quixo',
    description: ' Aperçu d’une partie et des scores attribués pour chaque possible de l’IA.',
  },
]
const current = ref(0)
const isImageOpen = ref(false)
const imageButton = ref(null)
const closeButton = ref(null)
let previousBodyOverflow = ''

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
  current.value = (current.value + 1) % images.length
}

function previous() {
  current.value = (current.value - 1 + images.length) % images.length
}
</script>

<template>
  <main id="contenu" class="project-detail-page" @keydown.esc.window="closeImage">
    <article class="project-detail container">
      <a class="back-link" :href="homePageUrl" @click.prevent="emit('back')">&lt; Retour aux projets</a>

      <p class="eyebrow project-meta">Projet universitaire · Avril — juin 2025</p>
      <h1>Quixo</h1>

      <section class="project-context">
        <h2>Contexte</h2>
        <p>
          Réalisé d’avril à juin 2025 dans le cadre de ma première année de BUT Informatique, ce projet
          consistait à développer le <strong>jeu de plateau Quixo en Java</strong>, en <strong>groupe de 5 personnes</strong> et en
          respectant un <strong>cahier des charges</strong>.
        </p>
      </section>

      <section class="carousel" aria-roledescription="carrousel" aria-label="Captures d’écran du projet Quixo">
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
                <img :src="images[current].src" :alt="images[current].alt" />
              </button>
              <figcaption>{{ images[current].description }}</figcaption>
            </figure>
          </Transition>
          <div class="carousel-arrows">
            <button class="carousel-button" type="button" aria-label="Image précédente" @click="previous">←</button>
            <button class="carousel-button" type="button" aria-label="Image suivante" @click="next">→</button>
          </div>
        </div>
        <p class="carousel-count" aria-live="polite">{{ current + 1 }} / {{ images.length }}</p>
      </section>

      <div class="project-copy">
        <section>
          <h2>Fonctionnalités</h2>
          <p>
            Nous avons d’abord développé une <strong>version console</strong>, jouable avec des commandes dans un
            terminal, puis une <strong>version graphique</strong>. La partie graphique se joue entièrement <strong>à la souris</strong>. Le jeu propose aussi
            <strong>deux intelligences artificielles</strong>, qui peuvent jouer contre une personne ou l’une contre
            l’autre.
          </p>
        </section>
        <section>
          <h2>Les deux intelligences artificielles</h2>
          <p>
            La première adopte une <strong>stratégie défensive</strong> : elle bloque le joueur dès qu’il aligne plus
            de trois pions et joue toujours au même endroit dans les autres cas. La seconde privilégie
            l’<strong>attaque</strong> : elle analyse le plateau et les coups possibles, puis choisit son coup à l’aide
            de <strong>coefficients</strong>. Elle cherche à créer une <strong>ligne de cinq pions</strong> tout en bloquant le joueur
            lorsqu’il peut gagner.
          </p>
        </section>
        <section>
          <h2>Mon rôle</h2>
          <p>
            J’ai participé avec le groupe à <strong>toutes les étapes</strong> : la version console, l’interface
            graphique, la conception des <strong>deux intelligences artificielles</strong> et les <strong>tests
            unitaires</strong>. Nous avons réparti le travail et avancé ensemble pour respecter le cahier des
            charges.
          </p>
        </section>
        <section>
          <h2>Ce que j’ai appris</h2>
          <p>
            Ce projet m’a appris à <strong>travailler en équipe</strong>, à utiliser <strong>JavaFX</strong> et à écrire des <strong>tests
            unitaires</strong>. J’ai également approfondi le développement et l’implémentation d’<strong>intelligences
            artificielles</strong>.
          </p>
        </section>
        <section>
          <h2>Technologies utilisées</h2>
          <div class="tag-list" aria-label="Technologies utilisées">
            <span>Java</span><span>JavaFX</span><span>GitLab</span><span>Tests unitaires</span>
          </div>
        </section>
      </div>

      <div class="project-detail-footer">
        <a class="button button-primary" :href="githubProjectUrl" target="_blank" rel="noreferrer">
          Voir le code sur GitHub <span aria-hidden="true">↗</span>
        </a>
      </div>
    </article>

    <div
      v-if="isImageOpen"
      class="image-lightbox"
      role="dialog"
      aria-modal="true"
      :aria-label="images[current].alt"
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
        <img :src="images[current].src" :alt="images[current].alt" />
        <figcaption>{{ images[current].description }}</figcaption>
      </figure>
    </div>
  </main>
</template>