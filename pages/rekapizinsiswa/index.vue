<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center">Rekapitulasi Izin Siswa</h2>
        <div class="my-3 d-flex justify-content-start gap-3">
          <button type="button" class="btn btn-success btn-lg rounded-5 px-5" @click="printCards">Print</button>
        </div>
        <form @submit.prevent="getrekapsiswa">
          <div class="row mb-3">
            <div class="col-md-6">
              <input v-model="keyword" type="text" class="form-control form-control-lg" placeholder="Cari Nama..." @input="getrekapsiswa">
            </div>
            <div class="col-md-6">
              <input v-model="selectedDate" type="date" class="form-control form-control-lg" @change="getrekapsiswa">
            </div>
          </div>
          <div class="my-3 text-muted">Menampilkan {{ filteredVisitors.length }} dari {{ jumlah }}</div>
          <div id="princardArea" class="row">
            <div class="col-lg-4 mb-4" v-for="(visitor, i) in filteredVisitors" :key="i">
              <div class="card">
                <div class="card-body">
                  <h5 class="card-title text-center">{{ visitor.nama }}</h5>
                  <h6 class="card-subtitle mb-2 text-muted text-center">{{ visitor.kelas.nama }}</h6>
                  <p class="card-text">
                    <strong>Waktu:</strong> {{ visitor.waktu }}<br>
                    <strong>Tanggal:</strong> {{ visitor.tanggal }}<br>
                    <strong>Keperluan:</strong> {{ visitor.keperluan }}<br>
                    <strong>Waktu Kembali:</strong> {{ visitor.waktu_kembali }}
                  </p>
                </div>
              </div>
            </div>
          </div>
        </form>
      </div>
      <NuxtLink to="/rekapizinsiswa/siswa" class="col-lg-6 d-flex justify-content-end">
        <button class="btn btn-primary btn-lg rounded-5">Tambah Data</button>
      </NuxtLink>
      <NuxtLink to="/home" class="col-lg-6 d-flex justify-content-start">
        <button class="btn btn-primary btn-lg rounded-5">Kembali</button>
      </NuxtLink>
    </div>
  </div>
</template>

<style scoped>
.card {
  margin-bottom: 20px;
}
</style>

<script setup>

const supabase = useSupabaseClient();

const visitors = ref([]);
const jumlah = ref(0);
const keyword = ref('');
const selectedDate = ref('');

// Fungsi untuk mendapatkan data rekap siswa
const getrekapsiswa = async () => {
  const { data, error } = await supabase.from('rekapizinsiswa').select(`*, kelas(*)`);
  
  if (data) {
    // Mengurutkan data berdasarkan tanggal terbaru
    visitors.value = data.sort((a, b) => new Date(b.tanggal) - new Date(a.tanggal));
    jumlah.value = visitors.value.length;
  } else {
    visitors.value = [];
    jumlah.value = 0;
  }
};

// Fungsi untuk mendapatkan filtered visitors berdasarkan keyword dan selectedDate
const filteredVisitors = computed(() => {
  return visitors.value.filter(visitor => {
    const matchesName = visitor.nama.toLowerCase().includes(keyword.value.toLowerCase());
    const matchesDate = selectedDate.value ? visitor.tanggal === selectedDate.value : true;
    return matchesName && matchesDate;
  });
});

// Fungsi untuk mencetak card
const printCards = () => {
  const printContents = document.getElementById('princardArea').innerHTML;
  const originalContents = document.body.innerHTML;

  document.body.innerHTML = printContents;
  window.print();
  document.body.innerHTML = originalContents;

  // Memastikan Vue me-render ulang halaman setelah pencetakan
  window.location.reload();
};

// Memanggil fungsi saat komponen dimuat
onMounted(() => {
  getrekapsiswa();
});
</script>
