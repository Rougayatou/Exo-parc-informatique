<script setup>
import { ref, computed } from 'vue'
import OrdinateurCard from '../components/OrdinateurCard.vue'


const ordinateurs = ref(JSON.parse(localStorage.getItem('ordinateurs')))
const recherche = ref('')
const tri = ref('aucun')
const ordinateursAffiches = computed(function () {
  const texte = recherche.value.trim().toLowerCase()

  const resultat = []


  for (let i = 0; i < ordinateurs.value.length; i++) {
    const pc = ordinateurs.value[i]
    const modele = pc.modele.toLowerCase()
    const processeur = pc.processeur.toLowerCase()

    if (texte === '' || modele.includes(texte) || processeur.includes(texte)) {
      resultat.push(pc)
    }
  }

  if (tri.value === 'croissant' || tri.value === 'decroissant') {
    
    for (let i = 0; i < resultat.length; i++) {
      for (let j = 0; j < resultat.length - 1 - i; j++) {
        const premier = resultat[j]
        const suivant = resultat[j + 1]

        let echanger = false

        if (tri.value === 'croissant' && premier.memoire > suivant.memoire) {
          echanger = true
        }
        if (tri.value === 'decroissant' && premier.memoire < suivant.memoire) {
          echanger = true
        }

        if (echanger === true) {
          resultat[j] = suivant
          resultat[j + 1] = premier
        }
      }
    }
  }

  return resultat
})


function supprimer(id) {
  const reponse = confirm('Voulez-vous vraiment supprimer cet ordinateur ?')
  if (reponse === false) {
    return
  }

  const nouvelleListe = []
  for (let i = 0; i < ordinateurs.value.length; i++) {
    if (ordinateurs.value[i].id !== id) {
      nouvelleListe.push(ordinateurs.value[i])
    }
  }

  ordinateurs.value = nouvelleListe

  localStorage.setItem('ordinateurs', JSON.stringify(ordinateurs.value))
}
</script>

<template>
  <div class="contenu">
    <div class="barre">
      <p>Nombre d'ordinateurs : {{ ordinateurs.length }}</p>
      <RouterLink to="/ordinateurs/ajouter" class="bouton-ajouter">+ Ajouter un ordinateur</RouterLink>
    </div>

    <div class="outils">
      <input
        type="text"
        class="recherche"
        v-model="recherche"
        placeholder="Rechercher par modèle ou processeur..."
      />

      <select v-model="tri" class="tri">
        <option value="aucun">Pas de tri</option>
        <option value="croissant">Mémoire : de la plus petite à la plus grande</option>
        <option value="decroissant">Mémoire : de la plus grande à la plus petite</option>
      </select>
    </div>

    <p v-if="ordinateurs.length === 0">Aucun ordinateur pour le moment.</p>
    <p v-else-if="ordinateursAffiches.length === 0">Aucun résultat pour cette recherche.</p>

    <div class="grille">
      <OrdinateurCard
        v-for="pc in ordinateursAffiches"
        :key="pc.id"
        :ordinateur="pc"
        @supprimer="supprimer"
      />
    </div>
  </div>
</template>

<style scoped>
.contenu {
  max-width: 1000px;
  margin: 0 auto;
  padding: 20px;
}

.barre {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.bouton-ajouter {
  background-color: #16a34a;
  color: white;
  padding: 10px 16px;
  border-radius: 6px;
  text-decoration: none;
  font-size: 14px;
}

.outils {
  display: flex;
  gap: 12px;
  margin-bottom: 20px;
}

.recherche {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 6px;
  font-size: 14px;
  box-sizing: border-box;
}

.tri {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 6px;
  font-size: 14px;
  background-color: white;
}

.grille {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 16px;
}
</style>