<template>
  <div class="flex justify-center items-center h-screen w-full bg-blue-400">
    <div class="w-1/2 bg-white rounded shadow-2xl p-8 m-4">
      <h1
        class="block w-full text-center text-gray-800 text-2xl font-bold mb-6"
      >
        Cek Status Pendaftaran
      </h1>
      <form @submit.prevent="checkStatus">
        <div class="flex flex-col mb-4">
          <label class="mb-2 font-bold text-lg text-gray-900" for="email"
            >Email</label
          >
          <input
            class="border py-2 px-3 text-grey-800"
            type="email"
            id="email"
            v-model="email"
            required
          />
        </div>

        <button
          class="block bg-teal-400 hover:bg-teal-600 text-white uppercase text-lg mx-auto p-4 rounded"
          type="submit"
        >
          Cek Status
        </button>
      </form>

      <div v-if="status !== null" class="mt-4">
        <h3 class="text-lg font-weight-bold">Status Pendaftaran:</h3>
        <p class="mt-2">{{ status }}</p>
      </div>

      <a
        class="block w-full text-center no-underline mt-4 text-sm text-gray-700 hover:text-gray-900"
        href="/"
        >Belum Daftar?</a
      >
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      email: "",
      status: null,
    };
  },
  methods: {
    async checkStatus() {
      try {
        //ambil url email yang diinput
        const apiUrl = `http://localhost:3000/api/regis?email=${this.email}`;
        const response = await fetch(apiUrl, {
          method: "GET",
          headers: {
            "Content-Type": "application/json",
          },
        });

        if (response.ok) {
          const responseData = await response.json();
          console.log("API Response:", responseData);

          if (responseData.docs && responseData.docs.length > 0) {
            //mencari email di doc
            const userDocument = responseData.docs.find(
              (doc) => doc.email === this.email
            );

            if (userDocument) {
              //kalo ketemu, status diambil
              this.status = userDocument.status;
            } else {
              console.error("Error: Not found.");
            }
          } else {
            console.error("Error: Not Found.");
          }
        } else {
          console.error(
            "Failed to fetch status. Status code:",
            response.status
          );
        }
      } catch (error) {
        console.error("Error:", error);
      }
    },
  },
};
</script>
