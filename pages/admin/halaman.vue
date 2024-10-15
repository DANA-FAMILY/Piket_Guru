<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-12 text-center">
        <h1>Selamat Datang Admin</h1>
        <div class="info">
          <p>Silakan isi form di bawah ini untuk menambahkan data guru:</p>
        </div>
        <div class="form-container">
        <h2>Tambah Data Guru</h2>
        <div class="form-group">
          <div class="mb-3">
            <input v-model="newGuru" type="text" class="form-control form-control-lg rounded-5" placeholder="Tambah Nama Guru Baru.." />
            <button type="button" class="btn btn-success mt-2" @click="addGuru">Tambah Guru</button>
          </div>
        </div>
      </div>
      </div>

      

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

const newGuru = ref('')

// Fungsi untuk menambahkan guru ke Supabase
const addGuru = async () => {
  if (newGuru.value) {
    const { error } = await supabase.from('guru').insert([{ NamaGuru: newGuru.value }])
    if (!error) {
      alert('Guru berhasil ditambahkan!')
      newGuru.value = "" // Kosongkan input setelah berhasil
      getGuru() // Refresh daftar guru
    } else {
      alert('Terjadi kesalahan saat menambah guru')
    }
  } else {
    alert('Nama Guru tidak boleh kosong')
  }
}

// Fungsi untuk logout
async function signOut() {
  const { error } = await supabase.auth.signOut()
  if (!error) {
    navigateTo('/')
  }
}
</script>
