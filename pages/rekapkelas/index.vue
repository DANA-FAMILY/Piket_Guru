<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-3">REKAPITULASI ABSENSI KELAS</h2>
        <div class="my-3 d-flex justify-content-end gap-3">
          <button type="button" class="btn btn-success btn-lg rounded-5 px-5" @click="printTable">Print</button>
        </div>
        <form @submit.prevent="getrekapkelas">
          <div class="my-3">
            <label for="tanggal">Pilih Tanggal:</label>
            <input type="date" id="tanggal" v-model="selectedDate" @change="onDateChange" />
          </div>
          <div class="my-3 text-muted">
            Menampilkan {{ visitors.length }} dari {{ jumlah }}
          </div>
          <div id="printableArea">
            <table style="width:100%" class="table text-center">
              <thead>
                <tr>
                  <th rowspan="2">No</th>
                  <th rowspan="2">Tanggal</th>
                  <th rowspan="2">Kelas</th>
                  <th rowspan="2">Guru Ngajar</th>
                  <th colspan="3">Jumlah Siswa</th>
                  <th colspan="5">Keterangan</th>
                </tr>
                <tr>
                  <th>Total</th>
                  <th>Hadir</th>
                  <th>Tidak Hadir</th>
                  <th>S</th>
                  <th>I</th>
                  <th>A</th>
                  <th>D</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(visitor, i) in visitors" :key="i">
                  <td>{{ i + 1 }}</td>
                  <td>{{ visitor.tanggal }}</td>
                  <td>{{ visitor.kelas.nama }}</td>
                  <td>{{ visitor.namaguru.NamaGuru }}</td>
                  <td>{{ visitor.total }}</td>
                  <td>{{ visitor.hadir }}</td>
                  <td>{{ visitor.tidak_hadir }}</td>
                  <td>{{ visitor.sakit }}</td>
                  <td>{{ visitor.izin }}</td>
                  <td>{{ visitor.alpa }}</td>
                  <td>{{ visitor.dispen }}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </form>
      </div>
      <NuxtLink to="/rekapkelas/absen" class="col-lg-6 d-flex justify-content-end">
        <button type="button" class="btn btn-primary btn-lg rounded-5 px-5">Tambah Data</button>
      </NuxtLink>
      <NuxtLink to="/home" class="col-lg-6 d-flex justify-content-start">
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
const supabase = useSupabaseClient();

const visitors = ref([]);
const jumlah = ref(0);
const selectedDate = ref(getTodayDate());

// Mendapatkan tanggal hari ini
function getTodayDate() {
  return new Date().toISOString().split('T')[0];
}

// Mendapatkan data dari Supabase
const getRekapKelas = async () => {
  const { data, error } = await supabase
    .from('rekapkelas')
    .select('*, namaguru(NamaGuru), kelas(nama)')
    .eq('tanggal', selectedDate.value);

  if (data) {
    visitors.value = data;
    jumlah.value = data.length;
  } else {
    console.error(error);
  }
};

// Fungsi untuk menyortir data berdasarkan jurusan
const sortedVisitors = computed(() => {
  const jurusanOrder = ['PPLG', 'TJKT', 'TBSM', 'DKV', 'TOI']; // Atur urutan jurusan
  return visitors.value.sort((a, b) => {
    const jurusanA = jurusanOrder.indexOf(a.kelas.nama.split(' ')[1]) || Infinity;
    const jurusanB = jurusanOrder.indexOf(b.kelas.nama.split(' ')[1]) || Infinity;
    return jurusanA - jurusanB;
  });
});

// Fungsi yang dipanggil saat tanggal berubah
const onDateChange = () => {
  getRekapKelas();
};

// Fungsi untuk mencetak tabel
const printTable = () => {
  const printContents = document.getElementById('printableArea').innerHTML;
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
        <h2 class="text-center">REKAPITULASI ABSENSI KELAS</h2>
        ${printContents}
      </body>
    </html>
  `;

  window.print();
  document.body.innerHTML = originalContents;
  window.location.reload();
};

// Memuat data saat halaman diakses
onMounted(() => {
  getRekapKelas();
});
</script>

