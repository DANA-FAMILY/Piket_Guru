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
              <p>Riwayat Presensi kelas</p>
            </NuxtLink>
          </div>
          <!-- Input Nama Guru -->
          <div class="mb-3">
            <select v-model="form.namaguru" class="form-control form-control-lg form-select rounded-5 mb-2" required>
              <option value="" selected>Nama Guru Mengajar</option>
              <option v-for="(member, i) in members" :key="i" :value="member.id">{{ member.NamaGuru }}</option>
            </select>
          </div>
          <!-- Input Kelas -->
          <div class="mb-3">
            <select v-model="form.kelas" class="form-control form-control-lg form-select rounded-5 mb-2" required>
              <option value="">Kelas</option>
              <option v-for="(item, i) in objectives" :key="i" :value="item.id">{{ item.nama }}</option>
            </select>
          </div>
          
          <!-- Input Total Siswa -->
          <div class="col-md-12">
            <h6>Total:</h6>
            <input v-model.number="form.total" type="number" class="form-control form-control-lg rounded-5" placeholder="Total Siswa.." required />
          </div>
          
          <!-- Input Hadir dan Tidak Hadir -->
          <div class="mb-4">
            <div class="row">
              <div class="col-md-4 p-2">
                <h6>Hadir:</h6>
                <input v-model.number="form.hadir" @input="updateTidakHadir" type="number" class="form-control form-control-lg rounded-5" placeholder="Hadir.." required />
              </div>
              <div class="col-md-4 p-2">
                <h6>Tidak Hadir:</h6>
                <input v-model.number="form.tidak_hadir" type="number" class="form-control form-control-lg rounded-5" placeholder="Tidak Hadir.." disabled required />
              </div>
              <div class="col-md-4 p-2">
                <h6>Sakit:</h6>
                <input v-model.number="form.sakit" @input="validateForm" type="number" class="form-control form-control-lg rounded-5" placeholder="Sakit.." required />
              </div>
              <div class="col-md-4 p-2">
                <h6>Izin:</h6>
                <input v-model.number="form.izin" @input="validateForm" type="number" class="form-control form-control-lg rounded-5" placeholder="Izin.." required />
              </div>
              <div class="col-md-4 p-2">
                <h6>Alpa:</h6>
                <input v-model.number="form.alpa" @input="validateForm" type="number" class="form-control form-control-lg rounded-5" placeholder="Alpa.." required />
              </div>
              <div class="col-md-4 p-2">
                <h6>Dispen:</h6>
                <input v-model.number="form.dispen" @input="validateForm" type="number" class="form-control form-control-lg rounded-5" placeholder="Dispen.." required />
              </div>
            </div>
          </div>

          <!-- Tombol Kirim dan Kembali -->
          <div class="col-lg-6 d-flex justify-content-start">
            <button type="submit" class="btn btn-primary btn-lg rounded-5 px-5">Kirim</button>
            <NuxtLink to="/home" class="btn btn-primary btn-lg rounded-5 px-5">Kembali</NuxtLink>
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
  namaguru: "",
  kelas: "",
  total: 0,
  hadir: 0,
  tidak_hadir: 0,
  sakit: 0,
  izin: 0,
  alpa: 0,
  dispen: 0,
})

// Fungsi untuk mengosongkan form
const resetForm = () => {
  form.value = {
    namaguru: "",
    kelas: "",
    total: 0,
    hadir: 0,
    tidak_hadir: 0,
    sakit: 0,
    izin: 0,
    alpa: 0,
    dispen: 0,
  }
}

// Fungsi untuk menghitung otomatis "Tidak Hadir"
const updateTidakHadir = () => {
  const totalTidakHadir = form.value.total - form.value.hadir
  form.value.tidak_hadir = totalTidakHadir >= 0 ? totalTidakHadir : 0
}

// Fungsi untuk memvalidasi jumlah sakit, izin, alpa, dispen
const validateForm = () => {
  const totalTidakHadir = form.value.total - form.value.hadir
  const totalInputTidakHadir = form.value.sakit + form.value.izin + form.value.alpa + form.value.dispen

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
  const totalInputTidakHadir = form.value.sakit + form.value.izin + form.value.alpa + form.value.dispen

  // Pastikan nilai sakit, izin, alpa, dispen sesuai dengan tidak_hadir
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
  const { data } = await supabase.from('guru').select('*')
  if (data) members.value = data
}

const getKelas = async () => {
  const { data } = await supabase.from('kelas').select('*')
  if (data) objectives.value = data
}

onMounted(() => {
  getKelas()
  getGuru()
})
</script>
