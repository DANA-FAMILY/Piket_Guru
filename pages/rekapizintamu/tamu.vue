<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-3">REKAPITULASI IZIN TAMU</h2>
        <form @submit.prevent="KirimData">
        <div class="my-3">
          <NuxtLink to="/rekapizintamu" class="d-flex justify-content-end text-decoration-none">
            <p>Riwayat izin tamu</p>
          </NuxtLink>
          <input v-model="form.nama" type="text" class="form-control form-control-lg rounded-5" placeholder="Nama">
        </div>
        <div class="my-3">
          <select v-model="form.keanggotaan" class="form-control form-control-lg form-select rounded-5">
            <option value="">Keanggotaan</option>
            <option v-for="(member,i) in members" :key="i" :value="member.id">{{ member.nama }}</option>
          </select>
        </div>
        <div class="mb-3"  v-if="form.keanggotaan == '2'">
        <select v-model="form.kelas" class="form-control from-control-lg form-select rounded-5 mb-2">
          <option value="">Kelas</option>
          <option v-for="(item,i) in objectives" :key="i" :value="item.id">{{ item.nama }}</option>
        </select>
      </div>
      <div class="mb-3">
        <input v-model="form.asal" type="text" class="form-control form-control-lg rounded-5" placeholder="Asal...">
      </div>
        <div class="my-3">
          <input v-model="form.keperluan" type="text" class="form-control form-control-lg rounded-5" placeholder="Keperluan">
        </div>
          <div class="col-lg-6 d-flex justify-content-start">
        <button type="submit" class="btn btn-primary btn-lg rounded-5 px-5">Kirim</button>
        <NuxtLink to="/home">
          <button type="submit" class="btn btn-primary btn-lg rounded-5 px-5">Kembali</button>
        </NuxtLink>
          </div>
      </form>
      </div>
        
    </div>
  </div>
  </template>
  <script setup>
const supabase = useSupabaseClient()

const members = ref ([])
const objectives = ref ([])

const form = ref({
  nama: "",
  keanggotaan: "",
  asal: "",
  keperluan: "",
})

const KirimData = async () => {
  console.log(form.value)
  const { error } = await supabase.from('rekapizintamu').insert([form.value])
  if(!error) navigateTo('/rekapizintamu')
}

const getKeanggotaan = async () => {
  const { data, error } = await supabase.from('keanggotaan').select('*')
  if(data) members.value = data
}

const getKelas = async () => {
  const { data,error } = await supabase.from('kelas').select('*')
  if(data) objectives.value = data
}

onMounted(() => {
  getKeanggotaan()
  getKelas()
})

</script>