<template>
  <div class="container-fluid">
    <!-- Tombol Kembali -->
    <div class="row">
      <div class="col-lg-12 d-flex justify-content-end p-3">
        <NuxtLink to="/">
          <button class="btn btn-primary">Kembali</button>
        </NuxtLink>
      </div>
    </div>

    <!-- Foto Utama -->
    <div class="row">
      <div class="col-lg-12 d-flex justify-content-center align-items-center my-3">
        <img src="/assets/img/poto1.jpg" alt="Foto Keluarga" width="500" height="300" class="img-fluid rounded">
      </div>
    </div>

    <!-- Profil Anggota -->
    <div class="row d-flex justify-content-center">
      <!-- Anggota 1 -->
      <div class="col-lg-3 col-md-4 col-sm-6 d-flex flex-column justify-content-center align-items-center my-3">
        <img src="/assets/img/poto1.jpg" alt="Anggota 1" width="150" height="150" class="rounded-circle">
        <h3 class="mt-3">Ega Suzana</h3>
        <p class="text-center">Front end</p>
      </div>
      <!-- Anggota 2 -->
      <div class="col-lg-3 col-md-4 col-sm-6 d-flex flex-column justify-content-center align-items-center my-3">
        <img src="/assets/img/poto1.jpg" alt="Anggota 2" width="150" height="150" class="rounded-circle">
        <h3 class="mt-3">Irgi Cahya</h3>
        <p class="text-center">Desain</p>
      </div>
      <!-- Anggota 3 -->
      <div class="col-lg-3 col-md-4 col-sm-6 d-flex flex-column justify-content-center align-items-center my-3">
        <img src="/assets/img/poto1.jpg" alt="Anggota 3" width="150" height="150" class="rounded-circle">
        <h3 class="mt-3">Sani Sahrul M</h3>
        <p class="text-center">Backend</p>
      </div>
      <div class="col-lg-3 col-md-4 col-sm-6 d-flex flex-column justify-content-center align-items-center my-3">
        <img src="/assets/img/poto1.jpg" alt="Anggota 3" width="150" height="150" class="rounded-circle">
        <h3 class="mt-3">Gilang A</h3>
        <p class="text-center">Prototipe</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
  .d-flex {
    display: flex;
  }
  .rounded-circle {
    border: 2px solid #ccc;
  }
  .btn-primary {
    margin-left: 20px;
  }
  h3 {
    margin-top: 15px;
  }
  p {
    text-align: center;
  }
</style>
