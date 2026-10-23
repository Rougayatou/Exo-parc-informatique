<script setup>
import { ref } from 'vue'
import { useRoute, useRouter } from 'vue-router'

const route = useRoute()
const router = useRouter()

const liste = JSON.parse(localStorage.getItem('ordinateurs'))

const idModif = route.params.id
const modeModification = idModif !== undefined


const modele = ref('')
const processeur = ref('')
const memoire = ref('')
const disque = ref('')
const carteGraphique = ref('')
const photo = ref('')
const erreur = ref('')


function adresseImage(photo) {
  
  if (photo === '') {
    return new URL('../assets/images/defaut.svg', import.meta.url).href
  }

  
  if (photo.startsWith('http')) {
    return photo
  }

  return new URL(`../assets/images/${photo}`, import.meta.url).href
}


if (modeModification) {
  for (let i = 0; i < liste.length; i++) {
    if (liste[i].id === Number(idModif)) {
      modele.value = liste[i].modele
      processeur.value = liste[i].processeur
      memoire.value = liste[i].memoire
      disque.value = liste[i].disque
      carteGraphique.value = liste[i].carteGraphique
      photo.value = liste[i].photo
    }
  }
}


function enregistrer() {
 
  if (modele.value.trim() === '') {
    erreur.value = 'Le modèle est obligatoire'
    return
  }
  if (processeur.value.trim() === '') {
    erreur.value = 'Le processeur est obligatoire'
    return
  }
  if (Number(memoire.value) <= 0) {
    erreur.value = 'La mémoire doit être supérieure à 0'
    return
  }
  if (Number(disque.value) <= 0) {
    erreur.value = 'Le disque doit être supérieur à 0'
    return
  }

  if (modeModification) {
    
    for (let i = 0; i < liste.length; i++) {
      if (liste[i].id === Number(idModif)) {
        liste[i].modele = modele.value
        liste[i].processeur = processeur.value
        liste[i].memoire = Number(memoire.value)
        liste[i].disque = Number(disque.value)
        liste[i].carteGraphique = carteGraphique.value
        liste[i].photo = photo.value
      }
    }
  } else {
    let plusGrandId = 0
    for (let i = 0; i < liste.length; i++) {
      if (liste[i].id > plusGrandId) {
        plusGrandId = liste[i].id
      }
    }

    const nouveau = {
      id: plusGrandId + 1,
      modele: modele.value,
      processeur: processeur.value,
      memoire: Number(memoire.value),
      disque: Number(disque.value),
      carteGraphique: carteGraphique.value,
      photo: photo.value
    }
    liste.push(nouveau)
  }

  localStorage.setItem('ordinateurs', JSON.stringify(liste))

  router.push('/ordinateurs')
}


function choisirImage(event) {
  const fichier = event.target.files[0]

  if (fichier !== undefined) {
    photo.value = fichier.name
  }
}

function annuler() {
  router.push('/ordinateurs')
}
</script>

<template>
  <div class="contenu">
    <h2 v-if="modeModification">Modifier un ordinateur</h2>
    <h2 v-else>Ajouter un ordinateur</h2>

    <form class="formulaire" @submit.prevent="enregistrer">
      <div class="apercu">
          <img :src="adresseImage(photo)" alt="Aperçu de l'ordinateur" />

          <input type="file" accept="image/*" id="fichier-image" class="fichier-cache" @change="choisirImage" />
          <label for="fichier-image" class="bouton-image">Choisir une image</label>
          <p class="petit-texte">(ou indiquer un nom de fichier)</p>
      </div>

      <div class="champs">
        <label>Modèle *</label>
        <input type="text" v-model="modele" />

        <label>Processeur *</label>
        <input type="text" v-model="processeur" />

        <label>Mémoire (Go) *</label>
        <input type="number" v-model="memoire" />

        <label>Disque (Go) *</label>
        <input type="number" v-model="disque" />

        <label>Carte graphique</label>
        <input type="text" v-model="carteGraphique" />

        <label>Photo (nom du fichier, ex : dell.svg)</label>
        <input type="text" v-model="photo" />

        <p v-if="erreur !== ''" class="erreur">{{ erreur }}</p>

        <div class="boutons">
          <button type="button" class="annuler" @click="annuler">Annuler</button>
          <button type="submit" class="enregistrer">Enregistrer</button>
        </div>
      </div>
    </form>
  </div>
</template>

<style scoped>
.contenu {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.formulaire {
  display: flex;
  gap: 24px;
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.apercu {
  width: 40%;
  text-align: center;
}

.fichier-cache {
  display: none;
}

.bouton-image {
  display: inline-block;
  margin-top: 12px;
  padding: 10px 16px;
  background-color: #e5e7eb;
  border-radius: 6px;
  font-size: 14px;
  font-weight: bold;
  cursor: pointer;
}

.petit-texte {
  margin: 8px 0 0 0;
  font-size: 12px;
  color: #777;
}

.apercu img {
  width: 100%;
  object-fit: contain;
}

.champs {
  flex: 1;
  display: flex;
  flex-direction: column;
}

label {
  font-size: 14px;
  font-weight: bold;
  margin-top: 10px;
}

input {
  padding: 8px;
  margin-top: 4px;
  border: 1px solid #ccc;
  border-radius: 6px;
  box-sizing: border-box;
}

.erreur {
  color: #dc2626;
  font-weight: bold;
}

.boutons {
  display: flex;
  justify-content: flex-end;
  gap: 8px;
  margin-top: 16px;
}

.annuler,
.enregistrer {
  padding: 10px 18px;
  border: none;
  border-radius: 6px;
  font-size: 14px;
  cursor: pointer;
}

.annuler {
  background-color: #e5e7eb;
}

.enregistrer {
  background-color: #1e63b8;
  color: white;
}

@media (max-width: 600px) {
  .formulaire {
    flex-direction: column;
  }
  .apercu {
    width: 100%;
  }
}
</style>