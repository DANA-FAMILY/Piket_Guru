<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-12 text-center">
        <h1>Selamat Datang Admin</h1>
        <div class="info">
          <p>Silakan isi form di bawah ini untuk menambahkan data guru, mapel, dan kelas:</p>
        </div>
        <div class="form-container">
          <!-- Form untuk menambahkan data guru -->
          <h2>Tambah Data Guru</h2>
          <div class="form-group">
            <div class="mb-3">
              <input v-model="newGuru" type="text" class="form-control form-control-lg rounded-5" placeholder="Tambah Nama Guru Baru.." />
              <button type="button" class="btn btn-success mt-2" @click="addGuru">Tambah Guru</button>
            </div>
          </div>

          <!-- Form untuk menambahkan data mapel -->
          <h2>Tambah Data Mapel</h2>
          <div class="form-group">                                                                                                  
            <div class="mb-3">
              <input v-model="newMapel" type="text" class="form-control form-control-lg rounded-5" placeholder="Tambah Mata Pelajaran.." />
              <button type="button" class="btn btn-success mt-2" @click="addMapel">Tambah Mapel</button>
            </div>
          </div>

          <!-- Form untuk menambahkan data kelas -->
          <h2>Tambah Data Kelas</h2>
          <div class="form-group">                                                                                                  
            <div class="mb-3">
              <input v-model="newKelas" type="text" class="form-control form-control-lg rounded-5" placeholder="Tambah Nama Kelas.." />
              <button type="button" class="btn btn-success mt-2" @click="addKelas">Tambah Kelas</button>
            </div>
          </div>
        </div>
      </div>

      <!-- Tombol logout -->
      <form @submit.prevent="signOut">
        <button type="submit" class="btn btn-danger">Log out</button>
      </form>
    </div>
  </div>
</template>

<style scoped>
.form-container {
  max-width: 600px;
  margin: 20px auto;
  padding: 20px;
  background: white;
  border-radius: 5px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
.form-group {
  margin-bottom: 15px;
}
.form-group label {
  display: block;
  margin-bottom: 5px;
}
.form-group input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 3px;
}
.btn {
  background-color: #28a745;
  color: white;
  border: none;
  padding: 10px 15px;
  cursor: pointer;
  border-radius: 3px;
}
.btn-danger {
  background-color: #dc3545;
}
.btn:hover {
  background-color: #218838;
}
</style>

<script setup>

const supabase = useSupabaseClient()

// State untuk menyimpan input baru
const newGuru = ref('')
const newMapel = ref('')
const newKelas = ref('') // State untuk kelas baru

// Fungsi untuk menambahkan guru ke Supabase
const addGuru = async () => {
  if (newGuru.value) {
    const { error } = await supabase.from('guru').insert([{ NamaGuru: newGuru.value }])
    if (!error) {
      alert('Guru berhasil ditambahkan!')
      newGuru.value = "" // Kosongkan input setelah berhasil
      getGuru() // Refresh daftar guru (opsional jika Anda ingin menambahkan fungsi ini)
    } else {
      alert('Terjadi kesalahan saat menambah guru')
    }
  } else {
    alert('Nama Guru tidak boleh kosong')
  }
}

// Fungsi untuk menambahkan mapel ke Supabase
const addMapel = async () => {
  if (newMapel.value) {
    const { error } = await supabase.from('mata_pelajaran').insert([{ Mapel: newMapel.value }])
    if (!error) {
      alert('Mata Pelajaran berhasil ditambahkan!')
      newMapel.value = "" // Kosongkan input setelah berhasil
      getMapel() // Refresh daftar mapel (opsional jika Anda ingin menambahkan fungsi ini)
    } else {
      alert('Terjadi kesalahan saat menambah mapel')
    }
  } else {
    alert('Nama Mata Pelajaran tidak boleh kosong')
  }
}

// Fungsi untuk menambahkan kelas ke Supabase
const addKelas = async () => {
  if (newKelas.value) {
    const { error } = await supabase.from('kelas').insert([{ nama: newKelas.value }])
    if (!error) {
      alert('Kelas berhasil ditambahkan!')
      newKelas.value = "" // Kosongkan input setelah berhasil
      getKelas() // Refresh daftar kelas (opsional jika Anda ingin menambahkan fungsi ini)
    } else {
      alert('Terjadi kesalahan saat menambah kelas')
    }
  } else {
    alert('Nama Kelas tidak boleh kosong')
  }
}

// Fungsi untuk logout
async function signOut() {
  const { error } = await supabase.auth.signOut()
  if (!error) {
    navigateTo('/') // Navigasi ke halaman login
  }
}
</script>
