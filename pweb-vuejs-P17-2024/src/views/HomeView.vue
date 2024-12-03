<script lang="ts">
import BookCard from "../components/BookCard.vue";

interface BookObject {
  _id: string;
  title: string;
  author: string;
  tags: string[];
  coverImage: string;
  publishedDate: string;
  initialQty: number;
  qty: number;
  publisher: string;
}

export default {
  name: "HomeView",
  data() {
    return {
      booksData: [] as BookObject[],
      fetchError: false,
    };
  },
  async mounted() {
    try {
      const response = await fetch("http://localhost:1717/book");
      if (!response.ok) {
        throw new Error(`Error: ${response.statusText}`);
      }
      const data = await response.json();
      this.booksData = data.data;
    } catch (error) {
      console.error("Fetch error:", error);
      this.fetchError = true;
    }
  },
  components: {
    BookCard,
  },
  computed: {
    formattedPublishedDate(): string {
      const date = new Date(this.bookDetail.publishedDate);
      return date.toLocaleDateString("en-US", {
        year: "numeric",
        month: "long",
        day: "numeric",
      });
    },
    starRating(): string {
      const stars = Math.floor(this.bookDetail.rating.average);
      return "⭐".repeat(stars);
    },
  },
};
</script>

<template>
  <main class="mt-6 mx-4 md:mt-10 md:mx-8 pb-24">
    <!-- Hero Section -->
    <section
      class="bg-blue-500 text-white py-6 px-4 md:py-10 md:px-8 rounded-lg shadow-md text-center"
    >
      <h1 class="font-bold text-2xl md:text-4xl mb-2 md:mb-4">
        Selamat Datang di Perpustakaan P17
      </h1>
      <p class="text-sm md:text-lg">
        Temukan koleksi buku favorit Anda dan perluas wawasan Anda.
      </p>
    </section>

    <!-- Books Section -->
    <section class="mt-8 md:mt-10">
      <h2 class="font-bold text-2xl md:text-3xl text-center mb-4 md:mb-6">
        Koleksi Buku
      </h2>

      <div
        class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4 md:gap-6"
      >
        <!-- Display Books -->
        <BookCard
          v-if="booksData.length"
          v-for="book in booksData"
          :key="book._id"
          :book="book"
          class="shadow-lg rounded-lg overflow-hidden"
        />

        <!-- Fetch Error Message -->
        <div
          v-else-if="fetchError"
          class="col-span-full text-center text-red-500 font-semibold text-lg md:text-xl"
        >
          Gagal memuat data buku. Silakan coba lagi nanti.
        </div>

        <!-- Loading Placeholder -->
        <div v-else class="col-span-full flex justify-center items-center">
          <div class="flex items-center space-x-2">
            <div class="w-4 h-4 md:w-5 md:h-5 bg-blue-500 rounded-full animate-bounce"></div>
            <div
              class="w-4 h-4 md:w-5 md:h-5 bg-blue-500 rounded-full animate-bounce delay-150"
            ></div>
            <div
              class="w-4 h-4 md:w-5 md:h-5 bg-blue-500 rounded-full animate-bounce delay-300"
            ></div>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>
