<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-3">REKAPITULASI ABSENSI KELAS</h2>
        <form @submit.prevent="getrekapkelas">
          <div class="my-3">
            <label for="tanggal">Pilih Tanggal:</label>
            <input type="date" id="tanggal" v-model="selectedDate" @change="onDateChange" />
          </div>
            <div class="my-3 text-muted">
                menampilkan {{ visitors.length }} dari {{ jumlah }}
            </div>
          <table style="width:100%" class="table text-center">
            <tr>
              <th rowspan="2">No</th>
              <th rowspan="2">Tangggal</th>
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

// Mendapatkan tanggal hari ini dalam format "YYYY-MM-DD"
function getTodayDate() {
  return new Date().toISOString().split('T')[0];
}

const getrekapkelas = async (date = selectedDate.value) => {
  const { data, error } = await supabase
    .from('rekapkelas')
    .select(`*, namaguru(NamaGuru), kelas(nama)`)
    .eq('tanggal', date); // Filter berdasarkan tanggal yang dipilih
  if (data) visitors.value = data;
};

const totalrekapkelas = async (date = selectedDate.value) => {
  const { data, count } = await supabase
    .from('rekapkelas')
    .select("*", { count: 'exact' })
    .eq('tanggal', date); // Filter berdasarkan tanggal yang dipilih
  if (data) jumlah.value = count;
};

const onDateChange = () => {
  getrekapkelas();
  totalrekapkelas();
};

onMounted(() => {
  getrekapkelas();
  totalrekapkelas();
});
</script>

  onMounted(() => {
    getrekapkelas()
    totalrekapkelas()
  })
</script>
