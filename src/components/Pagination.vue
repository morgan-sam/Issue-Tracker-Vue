<template>
  <div>
    <div class="btn-container">
      <button v-if="page > 1" v-on:click="goToPage(page - 1)">Previous</button>
      <div class="pageCount">
        Page: {{ page }} /
        {{
          maxEntryCount === null
            ? Math.floor(entryCount / perPage) + 1
            : Math.min(
                Math.floor(entryCount / perPage) + 1,
                Math.ceil(maxEntryCount / perPage)
              )
        }}
      </div>
      <button
        v-if="
          page * perPage < entryCount &&
          (maxEntryCount === null
            ? true
            : page < Math.ceil(maxEntryCount / perPage))
        "
        v-on:click="goToPage(page + 1)"
      >
        Next
      </button>
    </div>
    <form
      class="goToPageForm"
      v-on:keydown.enter.prevent
      v-on:keyup.enter="formPageTransition"
    >
      <label class="pageSelectLabel">Go to page:</label>
      <input
        class="pageSelectInput"
        v-model="selectedPage"
        type="number"
        name="number"
      />
    </form>
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
    maxEntryCount: { type: Number, default: null },
    goToPage: Function,
  },
  methods: {
    formPageTransition: function () {
      console.log(this.maxEntryCount);
      const page = parseInt(this.selectedPage);
      if (
        page > 0 &&
        (page - 1) * this.perPage < this.entryCount &&
        this.maxEntryCount === null
          ? true
          : page <= Math.ceil(this.maxEntryCount / this.perPage)
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
  margin: 0.25rem;
}
.btn-container > * {
  margin: 0.25rem;
}
.goToPageForm > * {
  margin: 0.25rem;
}
.pageSelectInput {
  width: 3rem;
  text-align: center;
}
</style>
