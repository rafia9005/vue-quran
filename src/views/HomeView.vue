<template>
  <main>
    <h1 class="text-center font-bold text-3xl mt-5">Online Qur'an</h1>
    <div class="flex justify-center items-center mt-3">
      <input
        v-model="searchTerm"
        type="text"
        placeholder="Search Surat by Latin Name"
        class="p-2 border w-[300px] border-gray-300 rounded-md"
      />
    </div>
    <div class="flex justify-center items-center h-[80vh]" v-if="!data">
      <img class="h-16 w-16" src="https://icons8.com/preloaders/preloaders/1488/Iphone-spinner-2.gif" alt="">
    </div>
    <div class="flex justify-center items-center mt-5" v-else>
      <div class="grid grid-cols-1 sm:grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
        <div v-for="item in filteredData" :key="item.nomor">
          <div class="bg-white p-4 rounded-lg shadow-md">
            <router-link :to="'/surat/' + item.nomor">
              <h2 class="text-lg font-semibold">{{ item.nama }}</h2>
              <p class="text-gray-600">{{ item.namaLatin }}</p>
              <p class="text-sm mt-2">Jumlah Ayat : {{ item.jumlahAyat }}</p>
            </router-link>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      data: null,
      searchTerm: '',
    };
  },
  computed: {
    filteredData() {
      if (!this.data) return null;

      const searchTermLowerCase = this.searchTerm.toLowerCase();
      return this.data.filter((item) =>
        item.namaLatin.toLowerCase().includes(searchTermLowerCase)
      );
    },
  },
  async created() {
    const response = await axios.get("https://equran.id/api/v2/surat");
    this.data = response.data.data;
  },
};
</script>
