<script setup>

const exemples = [
  { id: 1, modele: 'Dell Latitude 5540', processeur: 'Intel Core i5', memoire: 16, disque: 512, carteGraphique: 'Intel Iris Xe', photo: 'dell.jpeg' },
  { id: 2, modele: 'HP ProBook 450 G10', processeur: 'Intel Core i7', memoire: 32, disque: 1000, carteGraphique: 'NVIDIA GeForce MX', photo: 'hp.jpeg' },
  { id: 3, modele: 'Lenovo ThinkPad E14', processeur: 'AMD Ryzen 5', memoire: 16, disque: 512, carteGraphique: 'Radeon Graphics', photo: 'lenovo.jpeg' },
  { id: 4, modele: 'ASUS TUF Gaming F15', processeur: 'Intel Core i7', memoire: 32, disque: 1000, carteGraphique: 'NVIDIA GeForce RTX', photo: 'asus.jpeg' },
  { id: 5, modele: 'Acer Aspire 5', processeur: 'Intel Core i5', memoire: 8, disque: 256, carteGraphique: 'Intel UHD Graphics', photo: 'acer.jpeg' }
]


if (localStorage.getItem('ordinateurs') === null) {
  localStorage.setItem('ordinateurs', JSON.stringify(exemples))
}

</script>

<template>
  <header class="entete">
    <h1>Parc informatique</h1>
    <p>Gestion des ordinateurs</p>
  </header>

  <RouterView />
</template>

<style>
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background-color: #f3f6fa;
  color: #222;
}

.entete {
  background-color: #1e63b8;
  color: white;
  padding: 16px 24px;
}

.entete h1 {
  margin: 0;
  font-size: 24px;
}

.entete p {
  margin: 4px 0 0 0;
  font-size: 14px;
}
</style>