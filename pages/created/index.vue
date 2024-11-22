<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-3">Profil Keluarga Dana</h2>
      </div>
    </div>
    <div class="row">
      <!-- Anggota 1 -->
      <div class="col-lg-4 d-flex flex-column justify-content-center align-items-center my-3">
        <img src="/assets/img/poto1.jpg" alt="Anggota 1" width="200" height="200" class="rounded-circle">
        <h3 class="mt-3">Nama Anggota 1</h3>
        <p>Deskripsi singkat tentang anggota 1, misalnya hobi, pekerjaan, atau kepribadian.</p>
      </div>
      <!-- Anggota 2 -->
      <div class="col-lg-4 d-flex flex-column justify-content-center align-items-center my-3">
        <img src="/assets/img/poto1.jpg" alt="Anggota 2" width="200" height="200" class="rounded-circle">
        <h3 class="mt-3">Nama Anggota 2</h3>
        <p>Deskripsi singkat tentang anggota 2, misalnya hobi, pekerjaan, atau kepribadian.</p>
      </div>
      <!-- Anggota 3 -->
      <div class="col-lg-4 d-flex flex-column justify-content-center align-items-center my-3">
        <img src="/assets/img/poto1.jpg" alt="Anggota 3" width="200" height="200" class="rounded-circle">
        <h3 class="mt-3">Nama Anggota 3</h3>
        <p>Deskripsi singkat tentang anggota 3, misalnya hobi, pekerjaan, atau kepribadian.</p>
      </div>
    </div>
    <NuxtLink to="/" class="p-3 m-2 d-flex justify-content-center">
      <button class="btn btn-primary">Kembali</button>
    </NuxtLink>
  </div>
</template>

<style scoped>
  .d-flex {
    display: flex;
  }
  .navbar-brand {
    display: flex;
  }
  h2 {
    margin-bottom: 30px;
  }
  .rounded-circle {
    border: 2px solid #ccc;
  }
</style>
