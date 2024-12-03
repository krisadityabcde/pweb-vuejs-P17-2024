<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "AddBookView",
  data() {
    return {
      title: "",
      author: "",
      publishedDate: "",
      publisher: "",
      description: "",
      coverImage: "",
      ratingAverage: 0,
      ratingCount: 0,
      tags: "",
      initialQty: 0,
      qty: 0,
      error: false,
      errorMsg: "",
    };
  },
  methods: {
    formatDate(date: string): string {
      return format(new Date(date), "dd MMMM yyyy");
    },
    async addBook() {
      const newBook = {
        title: this.title,
        author: this.author,
        publishedDate: this.formatDate(this.publishedDate),
        publisher: this.publisher,
        description: this.description,
        coverImage: this.coverImage,
        rating: {
          average: this.ratingAverage,
          count: this.ratingCount,
        },
        tags: this.tags.split(",").map((tag) => tag.trim()),
        initialQty: this.initialQty,
        qty: this.qty,
      };

      const response = await fetch("http://localhost:1717/book", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(newBook),
      });

      if (response.ok) {
        alert("Book added successfully!");
        this.error = false;
        this.$router.push("/");
      } else {
        this.error = true;
        const data = await response.json();
        if (data.message.includes("duplicate")) {
          this.errorMsg = "Book already exists!";
        } else {
          this.errorMsg = data.message;
        }
      }
    },
  },
});
</script>

<template>
  <main class="mt-6 mx-4 md:mt-10 md:mx-8 pb-24">
    <!-- Header -->
    <h1 class="font-bold text-2xl md:text-3xl text-center mb-8 text-white">
      Add a New Book
    </h1>

    <!-- Form -->
    <form
      @submit.prevent="addBook"
      class="max-w-2xl mx-auto bg-white p-6 md:p-8 rounded-lg shadow-lg border border-gray-200"
    >
      <!-- Title -->
      <div class="mb-6">
        <label for="title" class="block text-gray-800 font-medium mb-2">
          Book Title
        </label>
        <input
          v-model="title"
          id="title"
          type="text"
          class="w-full px-4 py-3 border rounded-lg shadow-sm focus:ring-2 focus:ring-blue-500 focus:outline-none text-gray-800"
          
          required
        />
      </div>

      <!-- Author -->
      <div class="mb-6">
        <label for="author" class="block text-gray-800 font-medium mb-2">
          Author
        </label>
        <input
          v-model="author"
          id="author"
          type="text"
          class="w-full px-4 py-3 border rounded-lg shadow-sm focus:ring-2 focus:ring-blue-500 focus:outline-none text-gray-800"
          
          required
        />
      </div>

      <!-- Published Date -->
      <div class="mb-6">
        <label for="publishedDate" class="block text-gray-800 font-medium mb-2">
          Published Date
        </label>
        <input
          v-model="publishedDate"
          id="publishedDate"
          type="date"
          class="w-full px-4 py-3 border rounded-lg shadow-sm focus:ring-2 focus:ring-blue-500 focus:outline-none text-gray-800"
          required
        />
      </div>

      <!-- Publisher -->
      <div class="mb-6">
        <label for="publisher" class="block text-gray-800 font-medium mb-2">
          Publisher
        </label>
        <input
          v-model="publisher"
          id="publisher"
          type="text"
          class="w-full px-4 py-3 border rounded-lg shadow-sm focus:ring-2 focus:ring-blue-500 focus:outline-none text-gray-800"
          
          required
        />
      </div>

      <!-- Description -->
      <div class="mb-6">
        <label for="description" class="block text-gray-800 font-medium mb-2">
          Description
        </label>
        <textarea
          v-model="description"
          id="description"
          rows="4"
          class="w-full px-4 py-3 border rounded-lg shadow-sm focus:ring-2 focus:ring-blue-500 focus:outline-none text-gray-800"
          required
        ></textarea>
      </div>

      <!-- Cover Image -->
      <div class="mb-6">
        <label for="coverImage" class="block text-gray-800 font-medium mb-2">
          Cover Image URL
        </label>
        <input
          v-model="coverImage"
          id="coverImage"
          type="url"
          class="w-full px-4 py-3 border rounded-lg shadow-sm focus:ring-2 focus:ring-blue-500 focus:outline-none text-gray-800"
          required
        />
      </div>

      <!-- Tags -->
      <div class="mb-6">
        <label for="tags" class="block text-gray-800 font-medium mb-2">
          Tags 
        </label>
        <input
          v-model="tags"
          id="tags"
          type="text"
          class="w-full px-4 py-3 border rounded-lg shadow-sm focus:ring-2 focus:ring-blue-500 focus:outline-none text-gray-800"
          required
        />
      </div>

      <!-- Rating -->
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
        <div>
          <label for="ratingAverage" class="block text-gray-800 font-medium mb-2">
            Average Rating
          </label>
          <input
            v-model="ratingAverage"
            id="ratingAverage"
            type="number"
            step="0.1"
            class="w-full px-4 py-3 border rounded-lg shadow-sm focus:ring-2 focus:ring-blue-500 focus:outline-none text-gray-800"
            required
          />
        </div>
        <div>
          <label for="ratingCount" class="block text-gray-800 font-medium mb-2">
            Rating Count
          </label>
          <input
            v-model="ratingCount"
            id="ratingCount"
            type="number"
            class="w-full px-4 py-3 border rounded-lg shadow-sm focus:ring-2 focus:ring-blue-500 focus:outline-none text-gray-800"
            required
          />
        </div>
      </div>

      <!-- Stock -->
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
        <div>
          <label for="initialQty" class="block text-gray-800 font-medium mb-2">
            Initial Quantity
          </label>
          <input
            v-model="initialQty"
            id="initialQty"
            type="number"
            class="w-full px-4 py-3 border rounded-lg shadow-sm focus:ring-2 focus:ring-blue-500 focus:outline-none text-gray-800"
            required
          />
        </div>
        <div>
          <label for="qty" class="block text-gray-800 font-medium mb-2">
            Current Quantity
          </label>
          <input
            v-model="qty"
            id="qty"
            type="number"
            class="w-full px-4 py-3 border rounded-lg shadow-sm focus:ring-2 focus:ring-blue-500 focus:outline-none text-gray-800"
            required
          />
        </div>
      </div>

      <!-- Error Message -->
      <div
        class="text-center text-red-500 font-medium mb-4"
        v-if="error"
      >
        {{ errorMsg }}
      </div>

      <!-- Submit Button -->
      <div class="flex justify-center">
        <button
          type="submit"
          class="px-6 py-3 bg-blue-500 text-white font-semibold rounded-lg shadow-md hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-400 transition duration-200"
        >
          Add Book
        </button>
      </div>
    </form>
  </main>
</template>


