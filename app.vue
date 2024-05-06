<template>
  <div class="container mx-auto py-8">
    <h1 class="text-2xl font-bold mb-4">Liste des offres d'emploi</h1>
    <div v-if="loading" class="text-gray-600">Chargement en cours...</div>
    <div v-else>
      <div v-for="job in displayedJobs" :key="job.id" class="bg-white shadow-md rounded-lg p-6 mb-6">
        <h2 class="text-2xl font-bold mb-2">{{ job.title }}</h2>
        <p class="text-gray-600 mb-2">Date de création : {{ formatDate(job.createdAt) }}</p>
        <p class="text-gray-600 mb-2">Slug : {{ job.slug }}</p>
        <div class="flex flex-wrap items-center">
          <p class="mr-4 text-gray-700"><span class="font-bold">Type :</span> {{ job.type }}</p>
          <p class="mr-4 text-gray-700"><span class="font-bold">Email d'application :</span> <a :href="'mailto:' + job.applicationEmail" class="text-blue-500 hover:underline">{{ job.applicationEmail }}</a></p>
        </div>
      </div>
      <div class="flex justify-center mt-4">
        <button @click="prevPage" :disabled="currentPage === 1" class="mr-2 px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">Précédent</button>
        <button @click="nextPage" :disabled="currentPage === totalPages" class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">Suivant</button>
      </div>
    </div>
  </div>
</template>

<script>
import { format } from 'date-fns';

export default {
  data() {
    return {
      jobs: [],
      loading: true,
      currentPage: 1,
      itemsPerPage: 3 // Nombre d'offres par page
    }
  },
  computed: {
    totalPages() {
      return Math.ceil(this.jobs.length / this.itemsPerPage);
    },
    displayedJobs() {
      const startIndex = (this.currentPage - 1) * this.itemsPerPage;
      const endIndex = startIndex + this.itemsPerPage;
      return this.jobs.slice(startIndex, endIndex);
    }
  },
  async mounted() {
    try {
      const response = await fetch('https://app.staging.profilpublic.fr/api/jobs');
      const data = await response.json();
      this.jobs = data.data;
      this.loading = false;
    } catch (error) {
      console.error('Error fetching data:', error);
      this.loading = false;
    }
  },
  methods: {
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
    formatDate(date) {
      return format(new Date(date), 'dd/MM/yyyy HH:mm');
    }
  }
}
</script>
