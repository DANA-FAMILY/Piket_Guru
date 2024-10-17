<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-3">Created By: Dana Family</h2>
      </div>
      <div class="col-lg-12 d-flex justify-content-center align-items-center">
        <a class="navbar-brand" href="#">
          <img src="/assets/img/poto1.jpg" alt="" width="500" height="300">
        </a>
      </div>
    </div>
    <NuxtLink to="/" class="p-3 m-2">
          <Button>Kembali</Button>
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
  h2.ms-3 {
    margin-left: 20px;
  }
</style>


