<template>
  <div class="container mt-5">
    <h1>Manajemen Proyek</h1>

    <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#tambahProyek">Tambah <i class="fa-solid fa-plus"></i></button>

    <Main />

  </div>
</template>

<script>
import Main from './components/Main.vue';

export default {
  name: 'App',
  components: {
    Main,
  },
  data() {
    return {
      projects: [],
    };
  },
};
</script>

<style scoped>

</style>
