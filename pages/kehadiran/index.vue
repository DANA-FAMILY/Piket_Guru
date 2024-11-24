<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-3">Rekapitulasi Kehadiran Guru</h2>
        <div class="my-3 d-flex justify-content-end gap-3">
          <button type="button" class="btn btn-success btn-lg rounded-5 px-5" @click="printTable">Print</button>
        </div>
        <form @submit.prevent="getKehadiran">
          <div class="my-3 col-lg-3">
            <label for="dateSearch" class="form-label">Cari Berdasarkan Tanggal:</label>
            <input type="date" id="dateSearch" v-model="selectedDate" @change="filterByDate" class="form-control"/>
          </div>
          <div id="printableArea">
            <table style="width:100%" class="table text-center">
              <thead>
                <tr>
                  <th>No</th>
                  <th>Tanggal</th>
                  <th>Nama Guru</th>
                  <th>Mata Pelajaran</th>
                  <th>Alasan</th>
                  <th>Tugas</th>
                  <th>Keterangan</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(visitor, i) in filteredVisitors" :key="i">
                  <td>{{ i + 1 }}</td>
                  <td>{{ visitor.tanggal }}</td>
                  <td>{{ visitor.NamaGuru.NamaGuru }}</td>
                  <td>{{ visitor.MataPelajaran.Mapel }}</td>
                  <td>{{ visitor.alasan }}</td>
                  <td>{{ visitor.tugas }}</td>
                  <td>{{ visitor.keterangan }}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </form>
      </div>
      <NuxtLink to="/kehadiran/tambah" class="col-lg-6 d-flex justify-content-end">
        <button type="button" class="btn btn-primary btn-lg rounded-5 px-5">Tambah Data</button>
      </NuxtLink>
      <NuxtLink to="/rekapkelas/absen" class="col-lg-6 d-flex justify-content-start">
        <button type="button" class="btn btn-primary btn-lg rounded-5 px-5">Kembali</button>
      </NuxtLink>
    </div>
  </div>
</template>

<style scoped>
  table, th, td {
    border: 1px solid black;
    border-collapse: collapse;
  }
</style>

<script setup>
const supabase = useSupabaseClient(); // Inisialisasi Supabase client
const visitors = ref([]);
const selectedDate = ref(getTodayDate()); // Variable untuk menyimpan tanggal yang dipilih
const filteredVisitors = ref([]);

function getTodayDate() {
  return new Date().toISOString().split('T')[0];
}

// Fungsi untuk mendapatkan data kehadiran dari Supabase
const getKehadiran = async () => {
  try {
    const { data, error } = await supabase
      .from('kehadiran')
      .select('*, NamaGuru(NamaGuru), MataPelajaran(Mapel)');

    if (error) throw error;
    if (data) visitors.value = data;
    
    filterByDate(); // Memfilter data segera setelah didapat
  } catch (error) {
    console.error('Error fetching data:', error.message);
  }
};

// Fungsi untuk memfilter berdasarkan tanggal
const filterByDate = () => {
  if (selectedDate.value) {
    filteredVisitors.value = visitors.value.filter((visitor) =>
      visitor.tanggal === selectedDate.value
    );
  }
};

// Fungsi untuk mencetak tabel
const printTable = () => {
  const navbar = document.getElementById('navbar').outerHTML; // Ambil isi navbar
  const printContents = document.getElementById('printableArea').innerHTML; // Ambil isi tabel
  const originalContents = document.body.innerHTML;

  document.body.innerHTML = `
    <html>
      <head>
        <title>Print</title>
        <style>
          table, th, td {
            border: 1px solid black;
            border-collapse: collapse;
            text-align: center;
          }
        </style>
      </head>
      <body>
        ${navbar}
        <h2 class="text-center">Kehadiran Guru</h2>
        ${printContents}
      </body>
    </html>
  `;

  window.print();
  document.body.innerHTML = originalContents; 

  window.location.reload();
};

// Fetch data kehadiran saat komponen di-mount
onMounted(() => {
  getKehadiran();
});
</script>
