<template>
<div class="container-fluid">
  <div class="row">
    <div class="col-lg-12">
      <h2 class="text-center">Rekapitulasi Izin Siswa</h2>
      <form @submit.prevent="getrekapsiswa">
      <input v-model="keyword" type="text" class="form-control form-control-lg " placeholder="filter....">
      <div class="my-3 text-muted">Menampilkan {{ visitors.length }} dari {{ jumlah }}</div>
      <table class="table text-center" style="width:100%">
          <tr>
            <th>No</th>
            <th>Nama</th>
            <!-- <th>Keanggotaan</th> -->
            <th>Kelas</th>
            <th>Waktu</th>
            <th>Tanggal</th>
            <th>Keperluan</th>
            <th>Waktu_Kembali</th>
          </tr>
          <tbody>
            <tr v-for="(visitor,i) in visitors" :key="i">
              <td>{{ i+1 }}</td>
              <td>{{ visitor.nama }}</td>
              <!-- <td>{{ visitor.keanggotaan.nama }}</td> -->
              <td>{{ visitor.kelas.nama }}</td>
              <td>{{ visitor.waktu }}</td>
              <td>{{ visitor.tanggal }}</td>
              <td>{{ visitor.keperluan }}</td>
              <td>{{ visitor.waktu_kembali }}</td>
            </tr>
          </tbody>
      </table>
    </form>
    </div>
    <NuxtLink to="/rekapizinsiswa/siswa" class="col-lg-6 d-flex justify-content-end">
      <button class="btn btn-primary btn-lg rounded-5 ">Tambah Data</button>
    </NuxtLink>
    <NuxtLink to="/home" class="col-lg-6 d-flex justify-content-start">
      <button class="btn btn-primary btn-lg rounded-5 ">Kembali</button>
    </NuxtLink>
  </div>
</div>

</template>
<style scoped>
table ,th, td{
  border: 1px solid black;
  border-collapse: collapse;
}

</style>
<script setup>
const supabase = useSupabaseClient()

const visitors = ref([])
const jumlah = ref(0)
const keyword = ref('')

const getrekapsiswa = async () => {
  const { data, error } = await supabase.from('rekapizinsiswa').select(`*, kelas(*)`)
  .ilike('nama', `%${keyword.value}%`)
  if(data) visitors.value = data
}

const totalrekapsiswa = async () => {
  const { data, count } = await supabase.from('rekapizinsiswa').select("*", { count: 'exact'})
  if (data) jumlah.value = count
}


onMounted (() => {
  getrekapsiswa()
  totalrekapsiswa()
})

</script>