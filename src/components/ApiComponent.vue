<template>
  <main>
    <loading
      v-model:active="isLoading"
      :can-cancel="false"
      :is-full-page="fullPage"
    />

    <div v-if="entries?.length">
      <div class="header">
        <div class="search">
          <label for="">Search by Title: </label>
          <input
            type="text"
            v-model="title"
            placeholder=""
            v-on:change="onChange"
          />
          <button v-on:click="onChange">Search</button>
        </div>
        <div class="category">
          <label>Category: </label>
          <select v-model="category" v-on:change="onChange">
            <option value="all">All</option>
            <option
              v-for="category in categories"
              v-bind:value="category"
              v-bind:key="category"
            >
              {{ category }}
            </option>
          </select>
        </div>
      </div>
      <div class="list">
        <ol>
          <li v-for="post of filteredEntries" v-bind:key="post">
            <p>
              <strong>Name: {{ post.API }}</strong>
            </p>
            <p>
              <b>Description: {{ post.Description }}</b>
            </p>
            <p>
              <b>Category: {{ post.Category }}</b>
            </p>
          </li>
        </ol>
      </div>
    </div>

    <ul v-if="errors?.length">
      <li v-for="error of errors" v-bind:key="error">
        {{ error.message }}
      </li>
    </ul>

    <div v-if="!isDataAvailable" class="text-center">
      <h3>No Records found!</h3>
    </div>
  </main>
</template>

<script>
import axios from "axios";
import Loading from "vue-loading-overlay";
import "vue-loading-overlay/dist/vue-loading.css";
const BASE_URL ="https://api.publicapis.org";
export default {
    name : "ApiComponent",
    data() {
        return {
        entries: [],
        filteredEntries: [],
        categories: [],
        errors: [],
        isLoading: true,
        title: "",
        category: "all",
        isDataAvailable: true,
        };
    },
    components: {
        Loading,
    },
    async created() {
        try {
            const entriesPromise = axios.get(`${BASE_URL}/entries`);
            const categoriesPromise = axios.get(`${BASE_URL}/categories`);
            const [entriesRes, categoriesRes] = await Promise.all([
                entriesPromise,
                categoriesPromise,
            ]);

            this.entries = entriesRes.data.entries;
            this.filteredEntries = entriesRes.data.entries;
            this.categories = categoriesRes.data.categories
            this.isLoading = false;
        } catch (e) {
            console.error("Error", e);
            this.errors.push(e);
            this.isLoading = false;
          }
        },
    methods: {
        onChange: function() {
            if(this.category.toLowerCase() === "all") {
                this.filteredEntries = this.entries;
            } else {
                this.filteredEntries = this.entries.filter((data) =>
                data.Category.toLowerCase().includes(this.category.toLowerCase())
              );
            }
            if (this.title.length) {
                this.filteredEntries = this.filteredEntries.filter((data) =>
                data.API.toLowerCase().includes(this.title.toLowerCase())
                );
            }
            this.isDataAvailable = !!this.filteredEntries.length;
        }
    },

    };
</script>

<style scoped>
main {
    margin: 0px 50px;
    padding: 8px 30px;
    background: aliceblue;
    min-height: 90vh;
}

.header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 8px;
    margin: 16px;
}

.label {
    font-size: 1.2rem;
}

button {
    border: none;
    padding: 8px 12px;
    margin: 8px;
    color: #fff;
    background-color: #00a8ff;
    border-radius: 4px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    box-shadow: 0 8px 16px 0 rgb(0 0 0 / 20%), 0 6px 20px 0 rgb(0 0 0 / 19%);
    cursor: pointer;
}

input,
select {
    min-width: 250px;
    height: 30px;
    padding: 4px 8px;
    margin: 8px 0;
    display: inline-block;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
    background-color: inherit;
    cursor: pointer;
}

ol {
    padding: 16px;
}

li{
margin: 8px;
padding: 8px;
}

.list {
    padding: 8px;
    margin: 0;
}

.text-center {
    text-align: center;
}
</style>
