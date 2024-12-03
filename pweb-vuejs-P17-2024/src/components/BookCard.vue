<script lang="ts">
import { defineComponent } from "vue";
import { RouterLink } from "vue-router";
import type { PropType } from "vue";

export interface BookObject {
  _id: string;
  title: string;
  author: string;
  coverImage: string;
  publishedDate: string;
  tags: string[];
  initialQty: number;
  qty: number;
  publisher: string;
}

export default defineComponent({
  name: "BookCard",
  components: {
    RouterLink,
  },
  props: {
    book: {
      type: Object as PropType<BookObject>,
      required: true,
    },
  },
  methods: {
    getReadMoreLink(id: string) {
      return `/detail/${id}`;
    },
  },
  computed: {
    formattedPublishedDate(): string {
      const date = new Date(this.book.publishedDate);
      return date.toLocaleDateString("en-US", {
        year: "numeric",
        month: "long",
        day: "numeric",
      });
    },
  },
});
</script>

<template>
  <div class="border border-gray-700 rounded-xl overflow-hidden shadow-md bg-black text-white">
    <!-- Cover Image -->
    <img
      :src="book.coverImage"
      alt="Book cover"
      class="w-full h-48 object-cover"
    />

    <!-- Book Information -->
    <div class="p-6">
      <!-- Book Title -->
      <h3 class="text-lg font-bold text-white mb-2">{{ book.title }}</h3>

      <!-- Published Date -->
      <p class="text-sm font-medium mb-2">
        <strong>Published:</strong> {{ formattedPublishedDate }}
      </p>

      <!-- Author -->
      <p class="text-sm font-medium mb-2">
        <strong>Author:</strong> {{ book.author }}
      </p>

      <!-- Category -->
      <p class="text-sm font-medium mb-2">
        <strong>Category:</strong> {{ book.tags.join(", ") }}
      </p>

      <!-- Quantity -->
      <p class="text-sm font-medium mb-2">
        <strong>Quantity:</strong> {{ book.qty }} of {{ book.initialQty }} books
      </p>

      <!-- Read More Button -->
      <RouterLink
        :to="getReadMoreLink(book._id)"
        class="inline-block mt-4 px-6 py-2 bg-gray-800 text-white font-semibold rounded-md hover:bg-gray-600 transition"
      >
        Read More
      </RouterLink>
    </div>
  </div>
</template>
