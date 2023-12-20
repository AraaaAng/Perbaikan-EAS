<template>
  <div>
    <div class="flex justify-center items-center h-screen w-full bg-blue-400">
      <div class="w-1/2 bg-white rounded shadow-2xl p-8 m-4">
        <h1
          class="block w-full text-center text-gray-800 text-2xl font-bold mb-6"
        >
          Register
        </h1>
        <form @submit.prevent="submitForm">
          <div class="flex flex-col mb-4">
            <label class="mb-2 font-bold text-lg text-gray-900" for="nama"
              >Nama</label
            >
            <input
              type="text"
              id="nama"
              v-model="formData.nama"
              required
              class="border py-2 px-3 text-grey-800"
            />
          </div>

          <div class="flex flex-col mb-4">
            <label class="mb-2 font-bold text-lg text-gray-900" for="email"
              >Email</label
            >
            <input
              type="email"
              id="email"
              v-model="formData.email"
              required
              class="border py-2 px-3 text-grey-800"
            />
          </div>
          <div class="flex flex-col mb-4">
            <label class="mb-2 font-bold text-lg text-gray-900" for="sekolah"
              >Asal Sekolah</label
            >
            <input
              class="border py-2 px-3 text-grey-800"
              type="text"
              id="sekolah"
              v-model="formData.sekolah"
              required
            />
          </div>
          <div class="flex flex-col mb-4">
            <label for="tanggal" class="mb-2 font-bold text-lg text-gray-900"
              >Tanggal Pendaftaran:</label
            >
            <input
              type="date"
              id="tanggal"
              v-model="formData.tanggal"
              required
              class="border py-2 px-3 text-grey-800"
            />
          </div>

          <button
            class="block bg-teal-400 hover:bg-teal-600 text-white uppercase text-lg mx-auto p-4 rounded"
            type="submit"
          >
            Daftar
          </button>
        </form>
        <a
          class="block w-full text-center no-underline mt-4 text-sm text-gray-700 hover:text-gray-900"
          href="/status"
          >Cek Status Pendaftaran</a
        >
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        nama: "",
        email: "",
        sekolah: "",
        tanggal: "",
      },
    };
  },
  methods: {
    async submitForm() {
      try {
        const apiUrl = "http://localhost:3000/api/regis";

        const response = await fetch(apiUrl, {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(this.formData),
        });

        const responseData = await response.json();

        console.log("Data Pendaftaran Berhasil Dikirim:", responseData);

        // Reset form setelah pengiriman
        this.formData = {
          nama: "",
          email: "",
          sekolah: "",
          tanggal: "",
        };
      } catch (error) {
        console.error("Terjadi kesalahan saat mengirim data:", error);
      }
    },
  },
};
</script>
