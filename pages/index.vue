<template>
  <div>
    <v-row>
      <v-col md="4">
        <h2 class="text-center mb-5">Recently Read Books</h2>
        <v-row v-for="(item,index) in recentBooks" :key="index">
          <BookCard
            class="mb-5"
            :bookTitle="item.title"
            :bookAuthor="item.author"
            :bookDescription="item.description"
          >
            <template v-slot:button>
              <nuxt-link :to="`/books/${item.title}`">
                <v-btn>View</v-btn>
              </nuxt-link>
              <v-btn @click.stop="edit(item,index)">Edit</v-btn>
              <v-btn @click.stop="remove(item.category, index)">Remove</v-btn>
            </template>
          </BookCard>
        </v-row>
      </v-col>
      <v-col md="4">
        <h2 class="text-center mb-5">Favourite Books</h2>
        <v-row v-for="(item,index) in favouriteBooks" :key="index">
          <BookCard
            class="mb-5"
            :bookTitle="item.title"
            :bookAuthor="item.author"
            :bookDescription="item.description"
          >
            <template v-slot:button>
              <v-btn @click.stop="edit(item,index)">Edit</v-btn>
              <v-btn @click.stop="remove(item.category, index)">Remove</v-btn>
            </template>
          </BookCard>
        </v-row>
      </v-col>
      <v-col md="4">
        <h2 class="text-center mb-5">Best of the Best</h2>
        <v-row v-for="(item,index) in bestOfTheBest" :key="index">
          <BookCard
            class="mb-5"
            :bookTitle="item.title"
            :bookAuthor="item.author"
            :bookDescription="item.description"
          >
            <template v-slot:button>
              <v-btn @click.stop="edit(item,index)">Edit</v-btn>
              <v-btn @click.stop="remove(item.category, index)">Remove</v-btn>
            </template>
          </BookCard>
        </v-row>
      </v-col>
    </v-row>
    <BookModal />
  </div>
</template>

<script>
import BookCard from "@/components/BookCard";
import BookModal from "@/components/BookModal";
import { eventBus } from "@/eventBus";

export default {
  components: {
    BookCard,
    BookModal
  },
  data() {
    return {
      recentBooks: [],
      favouriteBooks: [],
      bestOfTheBest: []
    };
  },
  created() {
    eventBus.$on("save-book", cardData => {
      if (cardData.category === "Recently read books") {
        this.recentBooks.push(cardData);
        this.recentBooks.sort((a, b) => b - a);
      }
      if (cardData.category === "Favourite books") {
        this.favouriteBooks.push(cardData);
        this.favouriteBooks.sort((a, b) => b - a);
      }
      if (cardData.category === "Best of the best") {
        this.bestOfTheBest.push(cardData);
        this.bestOfTheBest.sort((a, b) => b - a);
      }
    });
  },
  methods: {
    remove(category, index) {
      if (category === "Recently read books") {
        this.recentBooks.splice(index, 1);
      }
      if (category === "Favourite books") {
        this.favouriteBooks.splice(index, 1);
      }
      if (category === "Best of the best") {
        this.bestOfTheBest.splice(index, 1);
      }
    },
    edit(item, index) {
      if (item.category === "Recently read books") {
        eventBus.$emit("open-add-book-modal", item);
        this.recentBooks.splice(index, 1);
      }
      if (item.category === "Favourite books") {
        eventBus.$emit("open-add-book-modal", item);
        this.favouriteBooks.splice(index, 1);
      }
      if (item.category === "Best of the best") {
        eventBus.$emit("open-add-book-modal", item);
        this.bestOfTheBest.splice(index, 1);
      }
    }
  }
};
</script>
<style lang="scss" scoped>
</style>