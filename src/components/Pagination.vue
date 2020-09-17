<template>
  <div>
    <div class="btn-container">
      <button v-if="page > 1" v-on:click="goToPage(page - 1)">Previous</button>
      <div>
        Page: {{ page }} /
        {{ Math.min(Math.floor(entryCount / perPage) + 1, Math.ceil(maxEntryCount / perPage)) }}
      </div>
      <button
        v-if="page * perPage < entryCount && page < Math.ceil(maxEntryCount / perPage)"
        v-on:click="goToPage(page + 1)"
      >Next</button>
    </div>
    <div>
      <form v-on:keydown.enter.prevent v-on:keyup.enter="formPageTransition">
        <label>Go to page:</label>
        <input class="pageSelectInput" v-model="selectedPage" type="number" name="number" />
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: "pagination",
  data: () => ({
    selectedPage: null,
  }),
  props: {
    page: Number,
    perPage: Number,
    entryCount: Number,
    maxEntryCount: Number,
    goToPage: Function,
  },
  methods: {
    formPageTransition: function () {
      const page = parseInt(this.selectedPage);
      if (
        page > 0 &&
        (page - 1) * this.perPage < this.entryCount &&
        page <= Math.ceil(this.maxEntryCount / this.perPage)
      )
        this.goToPage(page);
      else alert("Entered page is out of bounds");
    },
  },
};
</script>


<style scoped>
.btn-container {
  display: flex;
  align-items: center;
}
.pageSelectInput {
  width: 3rem;
  text-align: center;
}
</style>
