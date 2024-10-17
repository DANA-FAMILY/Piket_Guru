<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center p-3 m-1">Kehadiran Guru</h2>
        <NuxtLink to="/kehadiran" class="text-decoration-none">
          <p class="d-flex justify-content-end">Riwayat Kehadiran Guru</p>
        </NuxtLink>
        <form @submit.prevent="KirimData">
          <div class="mb-3">
            <select v-model="form.NamaGuru" class="form-control form-control-lg form-select rounded-5 mb-2" required>
              <option value="" selected>Nama Guru</option>
              <option v-for="(member, i) in members" :key="i" :value="member.id">{{ member.NamaGuru }}</option>
            </select>
          </div>
          <div class="mb-3">
            <select v-model="form.MataPelajaran" class="form-control form-control-lg form-select rounded-5 mb-2" required>
              <option value="" selected>Mata Pelajaran</option>
              <option v-for="(item, i) in objectives" :key="i" :value="item.id">{{ item.Mapel }}</option>
            </select>
          </div>
          <div class="mb-3">
            <input v-model="form.alasan" type="text" class="form-control form-control-lg rounded-5" placeholder="Alasan.." required />
          </div>
          <div class="mb-3">
            <input v-model="form.tugas" type="text" class="form-control form-control-lg rounded-5" placeholder="Tugas.." required />
          </div>
          <div class="mb-3">
            <input v-model="form.keterangan" type="text" class="form-control form-control-lg rounded-5" placeholder="Keterangan.." required />
          </div>
          <div class="col-lg-6 d-flex justify-content-start">
            <button type="submit" class="btn btn-primary btn-lg rounded-5 px-5">Kirim</button>
            <NuxtLink to="/rekapkelas/absen">
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
const router = useRouter()

const members = ref([])
const objectives = ref([])

const form = ref({
  NamaGuru: "",
  MataPelajaran: "",
  alasan: "",
  tugas: "",
  keterangan: "",
})

// Fungsi untuk mengirim data ke tabel 'kehadiran' di Supabase
const KirimData = async () => {
  try {
    const { error } = await supabase
      .from('kehadiran')
      .insert([form.value])  // Mengirim data dari form ke Supabase

    if (error) {
      throw error // Menangani error jika ada
    }

    // Reset form setelah pengiriman berhasil
    form.value = {
      NamaGuru: "",
      MataPelajaran: "",
      alasan: "",
      tugas: "",
      keterangan: "",
    }

    // Arahkan ke halaman 'kehadiran' setelah sukses
    router.push('/kehadiran')
    // alert("Presensi Berhasil")
    // resetForm()
  } catch (err) {
    console.error('Gagal mengirim data:', err.message)
    alert('Gagal mengirim data. Silakan coba lagi.')
  }
}

// Fungsi untuk mendapatkan daftar nama guru dari Supabase
const getGuru = async () => {
  const { data, error } = await supabase.from('guru').select('*')
  if (data) members.value = data
  else console.error('Gagal memuat data guru:', error.message)
}

// Fungsi untuk mendapatkan daftar mata pelajaran dari Supabase
const getMapel = async () => {
  const { data, error } = await supabase.from('mata_pelajaran').select('*')
  if (data) objectives.value = data
  else console.error('Gagal memuat data mata pelajaran:', error.message)
}

// Mendapatkan data guru dan mata pelajaran saat komponen dimuat
onMounted(() => {
  getGuru()
  getMapel()
})
</script>
