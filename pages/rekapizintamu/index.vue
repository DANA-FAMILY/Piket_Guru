<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center">REKAPITULASI IZIN TAMU</h2>
        <form @submit.prevent="getRekapizintamu">
        <input v-model="keyword" type="text" class="form-control form-control-lg " placeholder="filter....">
        <div class="my-3 text-muted">Menampilkan {{ visitors.length }} dari {{ jumlah }} </div>
        <table class="table text-center" style="width:100%">
          <tbody>
            <tr>
              <th>No</th>
              <th>Nama</th>
              <th>Keanggotaan</th>
              <th>Waktu</th>
              <th>Tanggal</th>
              <th>Keperluan</th>
            </tr>
            <tr v-for="(visitor,i) in visitors" :key="i">
              <td>{{ i+1 }}</td>
              <td>{{ visitor.nama }}</td>
              <td>{{ visitor.keanggotaan.nama }}</td>
              <td>{{ visitor.waktu }}</td>
              <td>{{ visitor.tanggal }}</td>
              <td>{{ visitor.keperluan }}</td>
            </tr>
          </tbody>
        </table>
      </form>
        <NuxtLink to="/rekapizintamu/tamu" class="col-lg-6 d-flex justify-content-end">
        <button type="submit" class="btn btn-primary btn-lg rounded-5 px-5">Tambah Data</button>
        </NuxtLink>
      <NuxtLink to="/home" class="col-lg-6 d-flex justify-content-start">
          <button type="submit" class="btn btn-primary btn-lg rounded-5 px-5">Kembali</button>
        </NuxtLink>
      </div>\
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

const getRekapizintamu = async () => {
  const { data, error } = await supabase.from('rekapizintamu').select(`*, keanggotaan(nama)`)
  .ilike('nama', `%${keyword.value}%`)
  if(data) visitors.value = data
}

const totalPengunjung = async () => {
  const { data, count } = await supabase.from('rekapizintamu').select("*", { count: 'exact'})
  if (data) jumlah.value = count
}

onMounted (() => {
  getRekapizintamu()
  totalPengunjung()
})
</script>