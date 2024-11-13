<template>
  <div class="container-fluid">
    <div class="row my-5 rounded-5">
      <div class="row d-flex justify-content-center">
        <div class="col-lg-6">
          <nuxt-link to="/rekapkelas/absen" class="text-decoration-none">
            <div class="card bg-kelas rounded-5 m-2 pt-5">
              <div class="card-body">
                <h2 class="text-light text-center">Piket Mengontrol Kelas</h2>
              </div>
            </div>
          </nuxt-link>
        </div>
      </div>
      <div class="col-lg-6">
        <nuxt-link to="/rekapizinsiswa/siswa" class="text-decoration-none">
          <div class="card bg-loby rounded-5 m-3 pt-5">
            <div class="card-body">
              <h2 class="text-light text-center">Izin Siswa</h2>
            </div>
          </div>
        </nuxt-link>
      </div>
      <div class="col-lg-6">
        <nuxt-link to="/rekapizintamu/tamu" class="text-decoration-none">
          <div class="card bg-tamu rounded-5 m-3 pt-5">
            <div class="card-body">
              <h2 class="text-light text-center">Izin Tamu</h2>
            </div>
          </div>
        </nuxt-link>
      </div>
      <form @submit.prevent="signOut">
        <button type="submit" :disabled="isLoading">Log out</button>
      </form>
    </div>
    <div v-if="isLoading" class="loading-overlay">
      <div class="spinner"></div>
    </div>
  </div>
</template>

<style scoped>
.container-fluid {
  position: relative;
}
.card {
  height: 250px;
  box-shadow: 1px 1px 10px #424242;
}
.card.bg-kelas {
  background-image: url("../assets/img/kelas.jpg");
  background-repeat: no-repeat;
  background-position: center center;
  background-size: cover;
}
.card.bg-loby, .card.bg-tamu {
  background-image: url("../assets/img/ruangan.webp");
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center center;
}
.loading-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}
.spinner {
  width: 50px;
  height: 50px;
  border: 5px solid #1374e2;
  border-top: 5px solid #333;
  border-radius: 50%;
  animation: spin 100s linear infinite;
}
@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>

<script setup>
const supabase = useSupabaseClient();
const isLoading = ref(false);

async function signOut() {
  isLoading.value = true;
  const { error } = await supabase.auth.signOut();
  isLoading.value = false;
  if (!error) {
    navigateTo('/');
  }
}
</script>

