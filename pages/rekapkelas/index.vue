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
            menampilkan {{ visitors.length }} dari {{ jumlah }}
          </div>
          <div id="printableArea">
            <table style="width:100%" class="table text-center">
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
            <!-- <h6 class="text-end mt-5">Tasikmalaya,...,...,20..</h6> -->
          </div>
        </form>
      </div>
      <!-- <h6 class="p-3 text-end mt-5">......................................</h6> -->
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

function getTodayDate() {
  return new Date().toISOString().split('T')[0];
}

const getrekapkelas = async (date = selectedDate.value) => {
  const { data, error } = await supabase
    .from('rekapkelas')
    .select(`*, namaguru(NamaGuru), kelas(nama)`)
    .eq('tanggal', date);
  if (data) {
    visitors.value = data.sort((a, b) => new Date(b.tanggal) - new Date(a.tanggal));
  }
};

const totalrekapkelas = async (date = selectedDate.value) => {
  const { data, count } = await supabase
    .from('rekapkelas')
    .select("*", { count: 'exact' })
    .eq('tanggal', date);
  if (data) jumlah.value = count;
};

const onDateChange = () => {
  getrekapkelas();
  totalrekapkelas();
};

const getFormattedTodayDate = () => {
  const today = new Date();
  const day = String(today.getDate()).padStart(2, '0');
  const month = String(today.getMonth() + 1).padStart(2, '0');
  const year = today.getFullYear();
  return `${day}/${month}/${year}`;
};


const printTable = () => {
  const navbar = document.getElementById('navbar').outerHTML; // Ambil isi navbar
  const printContents = document.getElementById('printableArea').innerHTML; // Ambil isi tabel
  const formattedDate = getFormattedTodayDate();
  const tandaTangan = `
    <h6 class="text-end mt-5">Tasikmalaya, ${formattedDate}</h6>
    <h6 class="text-end mt-5">............................................</h6>
  `;
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
        <h2 class="text-center">REKAPITULASI ABSENSI KELAS</h2>
        ${printContents}
        ${tandaTangan}
      </body>
    </html>
  `;

  window.print();
  document.body.innerHTML = originalContents; 

  window.location.reload();
};


onMounted(() => {
  getrekapkelas();
  totalrekapkelas();
});

</script>
