<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center p-1 m-1">Presensi Kelas</h2>
        <form @submit.prevent="KirimData">
          <div class="col-lg-12 d-flex justify-content-end ">
            <NuxtLink to="/kehadiran/tambah" class="text-decoration-none p-3">
            <p>Kehadiran Guru</p>
          </NuxtLink>
          <NuxtLink to="/rekapkelas" class="text-decoration-none p-3">
            <p>Riwayat Prensensi kelas</p>
          </NuxtLink>
          </div>
          <!-- Input Nama Guru -->
          <div class="mb-3">
            <select v-model="form.namaguru" class="form-control form-control-lg form-select rounded-5 mb-2">
              <option value="Nama Guru Mengajar" selected>Nama Guru Mengajar</option>
              <option v-for="(member, i) in members" :key="i" :value="member.id">{{ member.NamaGuru }}</option>
            </select>
          </div>
          <!-- Input Kelas -->
          <div class="mb-3">
            <select v-model="form.kelas" class="form-control form-control-lg form-select rounded-5 mb-2">
              <option value="">Kelas</option>
              <option v-for="(item, i) in objectives" :key="i" :value="item.id">{{ item.nama }}</option>
            </select>
          </div>
          
          <!-- Input Total Siswa -->
          <div class="col-md-12">
            <input v-model="form.total" type="number" class="form-control form-control-lg rounded-5" placeholder="Total Siswa.." />
          </div>
          
          <!-- Input Hadir dan Tidak Hadir -->
          <div class="mb-4">
            <div class="row">
              <div class="col-md-4 p-2">
                <input v-model="form.hadir" @input="updateTidakHadir" type="number" class="form-control form-control-lg rounded-5" placeholder="Hadir.." />
              </div>
              <div class="col-md-4 p-2">
                <input v-model="form.tidak_hadir" type="number" class="form-control form-control-lg rounded-5" placeholder="Tidak Hadir.." disabled />
              </div>
              
              <!-- Input Sakit, Izin, Alpa, Dispen -->
              <div class="col-md-4 p-2">
                <input v-model="form.sakit" @input="validateForm" type="number" class="form-control form-control-lg rounded-5" placeholder="Sakit.." />
              </div>
              <div class="col-md-4 p-2">
                <input v-model="form.izin" @input="validateForm" type="number" class="form-control form-control-lg rounded-5" placeholder="Izin.." />
              </div>
              <div class="col-md-4 p-2">
                <input v-model="form.alpa" @input="validateForm" type="number" class="form-control form-control-lg rounded-5" placeholder="Alpa.." />
              </div>
              <div class="col-md-4 p-2">
                <input v-model="form.dispen" @input="validateForm" type="number" class="form-control form-control-lg rounded-5" placeholder="Dispen.." />
              </div>
            </div>
          </div>

          <!-- Tombol Kirim dan Kembali -->
          <div class="col-lg-6 d-flex justify-content-start">
            <button type="submit" class="btn btn-primary btn-lg rounded-5 px-5">Kirim</button>
            <NuxtLink to="/home">
              <button type="button" class="btn btn-primary btn-lg rounded-5 px-5">Kembali</button>
            </NuxtLink>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>


<script setup>
const supabase = useSupabaseClient()

const objectives = ref([])
const members = ref([])

const form = ref({
  kelas: "",
  total: "",
  hadir: "",
  tidak_hadir: "",
  sakit: "",
  izin: "",
  alpa: "",
  dispen: "",
})

// Fungsi untuk mengosongkan form
const resetForm = () => {
  form.value = {
    kelas: "",
    total: "",
    hadir: "",
    tidak_hadir: "",
    sakit: "",
    izin: "",
    alpa: "",
    dispen: "",
  }
}

// Fungsi untuk menghitung otomatis "Tidak Hadir"
const updateTidakHadir = () => {
  if (form.value.total && form.value.hadir >= 0) {
    const totalTidakHadir = form.value.total - form.value.hadir
    form.value.tidak_hadir = totalTidakHadir >= 0 ? totalTidakHadir : 0
  }
}

// Fungsi untuk memvalidasi jumlah sakit, izin, alpa, dispen
const validateForm = () => {
  const totalTidakHadir = form.value.total - form.value.hadir
  const totalInputTidakHadir = parseInt(form.value.sakit || 0) +
                                parseInt(form.value.izin || 0) +
                                parseInt(form.value.alpa || 0) +
                                parseInt(form.value.dispen || 0)

  if (totalInputTidakHadir > totalTidakHadir) {
    alert(`Jumlah Sakit + Izin + Alpa + Dispen tidak sesuai dengan ${totalTidakHadir}`)
    form.value.sakit = 0
    form.value.izin = 0
    form.value.alpa = 0
    form.value.dispen = 0
  }
}


const KirimData = async () => {
  const totalTidakHadir = form.value.total - form.value.hadir
  const totalInputTidakHadir = parseInt(form.value.sakit || 0) +
                                parseInt(form.value.izin || 0) +
                                parseInt(form.value.alpa || 0) +
                                parseInt(form.value.dispen || 0)

  // Pastikan nilai `sakit`, `izin`, `alpa`, `dispen` sesuai dengan `tidak_hadir`
  if (totalInputTidakHadir !== totalTidakHadir) {
    alert(`Jumlah Sakit + Izin + Alpa + Dispen harus sama dengan ${totalTidakHadir}`)
    return
  }

  const { error } = await supabase.from('rekapkelas').insert([form.value])
  if (!error) {
    alert("Presensi Berhasil")
    resetForm()
  }
}

const getGuru = async () => {
  const { data, error } = await supabase.from('guru').select('*')
  if (data) members.value = data
}

const getKelas = async () => {
  const { data, error } = await supabase.from('kelas').select('*')
  if (data) objectives.value = data
}

onMounted(() => {
  getKelas()
  getGuru()
})
</script>

