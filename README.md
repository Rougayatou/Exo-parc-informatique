# Gestion d'un parc informatique

Application web réalisée avec Vue.js 3 (Vite) qui permet de consulter, ajouter, modifier et supprimer des ordinateurs. Les données sont sauvegardées dans le navigateur (localStorage), sans serveur ni base de données.

## Installation

Il faut d'abord avoir installé [Node.js](https://nodejs.org). Ensuite, dans le dossier du projet :

```
npm install
```

## Lancement

```
npm run dev
```

Puis ouvrir l'adresse affichée dans le terminal (en général http://localhost:5173/).

## Fonctions réalisées

- Affichage de la liste des ordinateurs sous forme de cartes (photo, modèle, processeur, mémoire, disque, carte graphique)
- Ajout d'un ordinateur avec un formulaire
- Modification d'un ordinateur : la même page que l'ajout, avec les champs préremplis
- Suppression d'un ordinateur avec une confirmation (`confirm()`), la liste est mise à jour immédiatement
- Contrôles du formulaire : modèle et processeur obligatoires, mémoire et disque strictement positifs
- Sauvegarde dans le `localStorage` : les données sont conservées après un rechargement de la page, les 5 ordinateurs d'exemple ne sont ajoutés qu'à la première utilisation
- Choix d'une image avec le bouton « Choisir une image » ou en écrivant le nom du fichier

## Bonus réalisés

- Recherche par modèle ou par processeur
- Compteur d'ordinateurs
- Tri par capacité mémoire (croissant ou décroissant)

## Pages de l'application

| Adresse | Page |
|---|---|
| `/ordinateurs` | Liste des ordinateurs |
| `/ordinateurs/ajouter` | Ajout d'un ordinateur |
| `/ordinateurs/modifier/:id` | Modification d'un ordinateur |

## Organisation du projet

```
src/
  assets/images/               images des ordinateurs
  components/OrdinateurCard.vue  carte d'un ordinateur (props et emit)
  views/ListeOrdinateurs.vue     page de la liste (recherche, tri, suppression)
  views/EditOrdinateur.vue       page d'ajout et de modification
  router/index.js                les routes (Vue Router)
  App.vue                        page principale
  main.js                        point d'entrée
```

## Ajouter une photo

Copier l'image dans `src/assets/images/`, puis saisir son nom exact (avec l'extension, par exemple `dell.jpeg`) dans le champ « Photo » du formulaire, ou choisir le fichier avec le bouton « Choisir une image ».