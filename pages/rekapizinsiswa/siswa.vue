<template>
<div class="container-fluid">
  <div class="row">
    <div class="col-lg-12">
      <h2 class="text-center my-3">FORM IZIN SISWA</h2>
      <form @submit.prevent="KirimData">
        
      <div class="my-3">
        <NuxtLink to="/rekapizinsiswa" class="d-flex justify-content-end">
            <p>Riwayat Izin Siswa</p>
          </NuxtLink>
        <input v-model="form.nama" type="text" class="form-control form-control-lg rounded-5" placeholder="Nama">
      </div>
      <!-- <div class="my-3">
          <select v-model="form.keanggotaan" class="form-control form-control-lg form-select rounded-5">
            <option value="">Keanggotaan</option>
            <option v-for="(member,i) in members" :key="i" :value="member.id">{{ member.nama }}</option>
          </select>
      </div> -->
      <div class="mb-3">
        <select v-model="form.kelas" class="form-control from-control-lg form-select rounded-5 mb-2">
          <option value="">Kelas</option>
          <option v-for="(item,i) in objectives" :key="i" :value="item.id">{{ item.nama }}</option>
        </select>
      </div>
      <div class="mb-3">
        <input v-model="form.keperluan" type="text" class="form-control form-control-lg rounded-5" placeholder="Keperluan..">
      </div>
      <div class="mb-3">
        <input v-model="form.waktu_kembali" type="time" placeholder="waktu kembali" class="form-control form-control-lg rounded-3">
      </div>
      <div class="kirim col-lg-6 d-flex justify-content-end">
          <button type="submit" class="btn btn-primary btn-lg rounded-5 px-5">kirim</button>
      </div>
    </form>
      <div class="kirim col-lg-6 d-flex justify-content-start">
        <NuxtLink to="/home">
          <button type="submit" class="btn btn-primary btn-lg rounded-5 px-5">kembali</button>
        </NuxtLink>
      </div>
    </div>
  </div>
</div>
</template>

<script setup>
const supabase = useSupabaseClient()

// const members = ref ([])
const objectives = ref ([])

const form = ref({
  nama: "",
  // keanggotaan: "",
  kelas: "",
  keperluan: "",
  waktu_kembali: "",
})

const KirimData = async () => {
  console.log(form.value)
  const { error } = await supabase.from('rekapizinsiswa').insert([form.value])
  if(!error) navigateTo('/rekapizinsiswa')
}

// const getKeanggotaan = async () => {
//   const { data, error } = await supabase.from('keanggotaan').select('*')
//   if(data) members.value = data
// }

const getKelas = async () => {
  const { data, error } = await supabase.from('kelas').select('*')
  if(data) objectives.value = data
}

onMounted(() => {
  getKelas()
})
</script>