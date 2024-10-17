<template>
  <div class="container-fluid">
    <div class="row my-5 rounded-5">
      <div class="row d-flex justify-content-center">
      <div class="col-lg-6">
        <nuxt-link to="/rekapkelas/absen" class="text-decoration-none">
          <div class="card bg-kelas rounded-5 m-2 pt-5">
            <div class="card-body ">
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
              <h2 class="text-light text-center"> Izin Siswa</h2>
            </div>
          </div>
        </nuxt-link>
      </div>
        <div class="col-lg-6" >
          <nuxt-link to="/rekapizintamu/tamu" class="text-decoration-none">
            <div class="card bg-tamu rounded-5 m-3 pt-5">
              <div class="card-body">
                <h2 class="text-light text-center"> Izin Tamu</h2>
              </div>
            </div>
          </nuxt-link>
      </div>
      <form @submit.prevent="signOut">
          <button type="submit">Log out</button>
        </form>
    </div>
  </div>
</template>

<style scoped>
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
.card.bg-loby {
  background-image: url("../assets/img/ruangan.webp");
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center center; 
}
.card.bg-tamu {
  background-image: url("../assets/img/ruangan.webp");
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center center; 
}

</style>
<script setup>
const supabase = useSupabaseClient()

async function signOut() {
  const { error } = await supabase.auth.signOut()
  if(!error){
    navigateTo('/')
  }
}

</script>