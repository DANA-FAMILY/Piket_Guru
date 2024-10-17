<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center">REKAPITULASI IZIN TAMU</h2>
        <div class="my-3 d-flex justify-content-start gap-3">
          <button type="button" class="btn btn-success btn-lg rounded-4 px-4" @click="printTable">Print</button>
        </div>
        <form @submit.prevent="getRekapizintamu">
          <div class="d-flex gap-3 mb-3">
            <input v-model="keyword" type="text" class="form-control form-control-lg" placeholder="Cari Nama...." @input="getRekapizintamu">
            <input v-model="tanggal" type="date" class="form-control form-control-lg" @change="getRekapizintamu">
          </div>
          <div class="my-3 text-muted"> Menampilkan {{ visitors.length }} dari {{ jumlah }} </div>
          <table class="table text-center" style="width:100%" id="printableArea">
            <thead>
              <tr>
                <th>No</th>
                <th>Nama</th>
                <th>Keanggotaan</th>
                <th>Asal</th>
                <th>Waktu</th>
                <th>Tanggal</th>
                <th>Keperluan</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(visitor, i) in visitors" :key="i">
                <td>{{ i + 1 }}</td>
                <td>{{ visitor.nama }}</td>
                <td>{{ visitor.keanggotaan.nama }}</td>
                <td>{{ visitor.asal }}</td>
                <td>{{ visitor.waktu }}</td>
                <td>{{ visitor.tanggal }}</td>
                <td>{{ visitor.keperluan }}</td>
              </tr>
            </tbody>
          </table>
        </form>
        <div class="col-lg-6 d-flex justify-content-start">
          <NuxtLink to="/rekapizintamu/tamu">
          <button class="btn btn-primary btn-lg rounded-5 px-5"> Tambah Data </button>
        </NuxtLink>
        <NuxtLink to="/home">
          <button class="btn btn-primary btn-lg rounded-5 px-5"> Kembali </button>
        </NuxtLink>
      </div>
        
      </div>
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
const supabase = useSupabaseClient();

const visitors = ref([]);
const jumlah = ref(0);
const keyword = ref('');
const tanggal = ref('');

// Fungsi untuk mendapatkan data rekap izin tamu
const getRekapizintamu = async () => {
  let query = supabase.from('rekapizintamu').select(`*, keanggotaan(nama)`)
    .ilike('nama', `%${keyword.value}%`);

  // Jika tanggal terisi, tambahkan filter berdasarkan tanggal
  if (tanggal.value) {
    query = query.eq('tanggal', tanggal.value);
  } else {
    // Jika tidak ada tanggal yang dipilih, ambil data untuk tanggal hari ini
    const today = new Date().toISOString().split('T')[0];
    query = query.eq('tanggal', today);
  }

  const { data, error } = await query;

  if (data) {
    visitors.value = data;
    totalPengunjung(); // Update jumlah pengunjung
  }
};

// Fungsi untuk menghitung total pengunjung berdasarkan tanggal yang dipilih
const totalPengunjung = async () => {
  let query = supabase.from('rekapizintamu').select("*", { count: 'exact' });

  if (tanggal.value) {
    query = query.eq('tanggal', tanggal.value);
  } else {
    const today = new Date().toISOString().split('T')[0];
    query = query.eq('tanggal', today);
  }

  const { data, count } = await query;
  if (data) jumlah.value = count;
};

// Fungsi untuk mencetak tabel dan navbar
const printTable = () => {
  const navbar = document.getElementById('navbar')?.outerHTML || ''; // Mengambil isi navbar
  const printContents = document.getElementById('printableArea').outerHTML; // Mengambil isi tabel
  const originalContents = document.body.innerHTML;

  // Membangun halaman untuk mencetak
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
        <h2 class="text-center">REKAPITULASI IZIN TAMU</h2>
        ${printContents}
      </body>
    </html>
  `;
  
  window.print();
  document.body.innerHTML = originalContents; // Mengembalikan konten asli

  // Refresh halaman setelah pencetakan
  window.location.reload();
};

// Memanggil fungsi saat komponen dimuat
onMounted(() => {
  getRekapizintamu();
  totalPengunjung();
});
</script>
