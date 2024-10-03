<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center p-1 m-1"> Presensi Kelas</h2>
        <form @submit.prevent="KirimData">
          <NuxtLink to="/rekapkelas" class="d-flex justify-content-end text-decoration-none">
            <p>Riwayat Presensi Kelas</p>
          </NuxtLink>
          <div class="mb-3">
            <select v-model="form.namaguru" class="form-control form-control-lg form-select rounded-5 mb-2">
              <option value="" selected>Nama Guru Ngajar</option>
              <option v-for="(member,i) in members" :key="i" :value="member.id">{{ member.NamaGuru }}</option>
            </select>
          </div>
          <div class="mb-3">
            <select v-model="form.kelas" class="form-control from-control-lg form-select rounded-5 mb-2">
              <option value="">Kelas</option>
              <option v-for="(item,i) in objectives" :key="i" :value="item.id">{{ item.nama }}</option>
            </select>
          </div>
          <div class="col-md-12 ">
            <input v-model="form.total" type="number" class="form-control form-control-lg rounded-5" placeholder="Total Siswa..">
          </div>
              <div class="mb-4">
                <div class="row">
                  <div class="col-md-4 p-2">
                    <input v-model="form.hadir"  type="number" class="form-control form-control-lg rounded-5" placeholder="Hadir..">
              </div>
              <div class="col-md-4 p-2">
                <input v-model="form.tidak_hadir"  type="number" class="form-control form-control-lg rounded-5" placeholder="Tidak Hadir..">
              </div>
              <div class="col-md-4 p-2">
                <input v-model="form.sakit"  type="number" class="form-control form-control-lg rounded-5" placeholder="Sakit..">
            </div>
            <div class="col-md-4 p-2">
              <input v-model="form.izin"  type="number" class="form-control form-control-lg rounded-5" placeholder="Izin..">
            </div>
            <div class="col-md-4 p-2">
              <input v-model="form.alpa" type="number" class="form-control form-control-lg rounded-5" placeholder="Alpa..">
            </div>
            <div class="col-md-4 p-2">
              <input v-model="form.dispen" type="number" class="form-control form-control-lg rounded-5" placeholder="Dispen..">
            </div>
            </div>
          </div> 
          <div class="col-lg-6 d-flex justify-content-start">
            <button type="submit" class="btn btn-primary btn-lg rounded-5 px-5">kirim</button>
            <NuxtLink to="/home">
              <button type="submit" class="btn btn-primary btn-lg rounded-5 px-5">kembali</button>
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
  namaguru: "",
  kelas: "",
  total: "",
  hadir: "",
  tidak_hadir: "",
  sakit: "",
  izin: "",
  alpa: "",
  dispen: "",
})

const KirimData = async () => {
  console.log(form.value)
  const { error } = await supabase.from('rekapkelas').insert([form.value])
  // if(!error) navigateTo('/rekapkelas')
  if(!error) {
    alert("Presensi Berhasil")
  }
}

const getGuru = async () => {
  const { data, error } = await supabase.from('guru').select('*')
  if(data) members.value = data
}

const getKelas = async () => {
  const { data, error } = await supabase.from('kelas').select('*')
  if(data) objectives.value = data
}

onMounted(() => {
  getGuru()
  getKelas()
})

</script>