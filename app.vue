<template>
  <div class="container mx-auto py-8">
    <h1 class="text-2xl font-bold mb-4">Liste des offres d'emploi</h1>
    <div v-if="loading" class="text-gray-600">Chargement en cours...</div>
    <div v-else>
      <div v-for="job in jobs" :key="job.id" class="bg-white shadow p-4 rounded mb-4">
        <h2 class="text-xl font-bold">{{ job.title }}</h2>
        <p class="text-gray-600">Date of creation :{{ job.createdAt }}</p>
        <p class="text-gray-600">Slug :{{ job.slug }}</p>
        <div class="mt-2">
          <p class="mr-2">Status :{{ job.status }}</p>
          <p class="mr-2">Type:{{ job.type }}</p>
          <p class="mr-2">Application Email :{{ job. applicationEmail }}</p>

       </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      jobs: [],
      loading: true
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
  }
}
</script>