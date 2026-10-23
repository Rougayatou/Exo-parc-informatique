<script setup>

const props = defineProps(['ordinateur'])
const emit = defineEmits(['supprimer'])


function adresseImage(photo) {
  if (photo === '') {
    return new URL('../assets/images/defaut.svg', import.meta.url).href
  }

  if (photo.startsWith('http')) {
    return photo
  }

  return new URL(`../assets/images/${photo}`, import.meta.url).href
}

function cliquerSupprimer() {
  emit('supprimer', props.ordinateur.id)
}

</script>

<template>
  <div class="carte">
    <img class="photo" :src="adresseImage(ordinateur.photo)" :alt="ordinateur.modele" />

    <h3>{{ ordinateur.modele }}</h3>
    <p>Processeur : {{ ordinateur.processeur }}</p>
    <p>Mémoire : {{ ordinateur.memoire }} Go</p>
    <p>Disque : {{ ordinateur.disque }} Go</p>
    <p>Carte graphique : {{ ordinateur.carteGraphique }}</p>

    <div class="boutons">
      <RouterLink :to="'/ordinateurs/modifier/' + ordinateur.id" class="modifier">Modifier</RouterLink>
      <button class="supprimer" @click="cliquerSupprimer">Supprimer</button>
    </div>
  </div>
</template>

<style scoped>
.carte {
  background-color: white;
  border-radius: 10px;
  padding: 16px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.photo {
  width: 100%;
  height: 140px;
  object-fit: contain;
}

h3 {
  margin: 10px 0;
  font-size: 16px;
}

p {
  margin: 4px 0;
  font-size: 13px;
  color: #555;
}

.boutons {
  display: flex;
  gap: 8px;
  margin-top: 12px;
}

.modifier,
.supprimer {
  flex: 1;
  padding: 8px;
  border: none;
  border-radius: 6px;
  color: white;
  font-size: 14px;
  text-align: center;
  text-decoration: none;
  cursor: pointer;
}

.modifier {
  background-color: #1e63b8;
}

.supprimer {
  background-color: #dc2626;
}
</style>