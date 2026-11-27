<script setup>
import { nextTick, onBeforeUnmount, ref } from 'vue'

defineProps({
  homePageUrl: {
    type: String,
    required: true,
  },
})

const emit = defineEmits(['back'])
const githubProjectUrl = 'https://github.com/mdagbert/SAE_2.04'

const images = [
  {
    src: `${import.meta.env.BASE_URL}JNX/authentification.png`,
    alt: 'Formulaire d’authentification de Jean Xtreme',
    description: 'Écran de connexion pour accéder au site.',
  },
  {
    src: `${import.meta.env.BASE_URL}JNX/liste-envie.png`,
    alt: 'Liste d’envies du site Jean Xtreme',
    description: 'Liste  et historique : les articles peuvent être ré-organisés, commandés et supprimés.',
  },
  {
    src: `${import.meta.env.BASE_URL}JNX/graphic.png`,
    alt: 'Graphiques de la partie administrateur de Jean Xtreme',
    description: 'Tableaux de bord et graphiques sur les données du site (partie administrateur).',
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

      <p class="eyebrow project-meta">Projet universitaire · Février - mars 2025</p>
      <h1>Jean Xtreme</h1>

      <section class="project-context">
        <h2>Contexte</h2>
        <p>
          Ce projet a été réalisé en 2025, en <strong>équipe
          de 4 personnes</strong>. Il consistait à créer un <strong>site e-commerce de jeans</strong>, avec une <strong>partie client</strong> et
          une <strong>partie administrateur</strong> protégées par une <strong>authentification</strong>.
        </p>
      </section>

      <section class="carousel" aria-roledescription="carrousel" aria-label="Captures d’écran du projet">
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
          <div v-if="images.length > 1" class="carousel-arrows">
            <button class="carousel-button" type="button" aria-label="Image précédente" @click="previous">←</button>
            <button class="carousel-button" type="button" aria-label="Image suivante" @click="next">→</button>
          </div>
        </div>
        <p v-if="images.length > 1" class="carousel-count" aria-live="polite">{{ current + 1 }} / {{ images.length }}</p>
      </section>

      <div class="project-copy">
        <section>
          <h2>Fonctionnalités</h2>
          <p>
            Le site propose un <strong>catalogue de jeans</strong> que l’on peut <strong>filtrer</strong> selon plusieurs critères, ainsi
            qu’une <strong>liste d’envies</strong> et un <strong>historique</strong> des articles consultés. Le client peut <strong>passer
            commande</strong>, et l’administrateur peut <strong>valider les commandes</strong> et consulter des <strong>graphiques</strong>
            détaillant les jeans.
          </p>
        </section>
        <section>
          <h2>Mon rôle</h2>
          <p>
            J’ai développé la <strong>liste d’envies</strong> : chaque article possède un <strong>bouton cœur</strong> pour l’y ajouter,
            puis on peut le supprimer, passer commande ou le faire monter dans la liste avec des
            flèches. Une commande passée retire l’article de la liste. J’ai aussi créé l’<strong>historique</strong>,
            qui affiche les <strong>6 derniers articles consultés</strong>. Pour la partie administrateur, j’ai réalisé
            plusieurs <strong>graphiques</strong> sur les jeans présents dans les listes d’envies et dans l’historique.
          </p>
        </section>
        <section>
          <h2>Ce que j’ai appris</h2>
          <p>
            Ce projet m’a appris à développer avec <strong>Flask et SQL</strong>, à <strong>créer des graphiques</strong> et à mettre en
            place un <strong>système d’authentification</strong> avec login et mot de passe.
          </p>
        </section>
        <section>
          <h2>Technologies utilisées</h2>
          <div class="tag-list" aria-label="Technologies utilisées">
            <span>Python</span><span>Flask</span><span>SQL</span>
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