<template>
    <div class="flex justify-center items-center h-[100vh]" v-if="!data">
        <img class="h-16 w-16" src="https://icons8.com/preloaders/preloaders/1488/Iphone-spinner-2.gif" alt="">
    </div>
    <main v-else>
        <h1 v-if="data" class="text-center mt-5 text-3xl font-bold">{{ data.nama }} {{ data.namaLatin }}</h1>
        <div class="flex justify-center items-center w-[90%] mx-[5%]">
            <div class="mt-6 border-t border-gray-100">
                <dl class="divide-y divide-gray-100 px-4 py-6 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-0"
                    v-for="(item, key) in data.ayat" :key="key">
                    <div class="sm:col-span-1 mb-4">
                        <h1 class="text-xl">{{ item.teksArab }}</h1>
                    </div>
                    <div class="sm:col-span-2 mb-4">
                        <h1 class="text-md">{{ item.teksLatin }}</h1>
                        <p class="text-sm mt-2">{{ item.teksIndonesia }}</p>
                    </div>
                </dl>
            </div>
        </div>
        <audio autoplay>
            <source v-if="data" :src="getAudioSource(1)" type="audio/mp3">
        </audio>
    </main>
</template>
  
<script>
import axios from 'axios'

export default {
    data() {
        return {
            data: null,
        }
    },
    async created() {
        const url = this.$route.path;
        const segments = url.split("/");
        const id = segments[2];
        this.idItem = parseInt(id);
        console.log(this.idItem)
        try {
            const response = await axios.get(`https://equran.id/api/v2/surat/${this.idItem}`)
            this.data = response.data.data;
        } catch {
        }
    },
    methods: {
        getAudioSource(index) {
            const key = index.toString().padStart(2, '0');
            return this.data.audioFull[key] || "";
        }
    }
}
</script>
  