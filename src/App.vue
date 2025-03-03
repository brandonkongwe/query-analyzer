<template>
  <div class="container mx-auto p-6">
    <!-- title -->
    <h1 class="text-3xl font-bold text-center mb-6 text-blue-600">SQL Query Analyzer</h1>
    
    <!-- text box for entering query -->
    <div class="mb-6">
      <label for="query" class="block text-lg font-semibold mb-2 text-gray-700">Enter your SQL query:</label>
      <textarea id="query" v-model="query" rows="5" 
        class="w-full p-3 border border-gray-300 rounded-lg shadow-sm focus:ring-2 focus:ring-blue-500 focus:border-blue-500 transition duration-200"></textarea>
    </div>
    
    <!-- buttons for executing query and refreshing -->
    <div class="flex space-x-3 mb-6">
      <button @click="executeQuery" :disabled="isLoading" class="px-6 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 transition duration-200">
        {{ isLoading ? 'Executing...' : 'Execute Query' }}
      </button>
      <button @click="refresh" class="px-6 py-2 bg-gray-600 text-white rounded-lg hover:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-gray-500 focus:ring-offset-2 transition duration-200">Refresh</button>
    </div>

    <!-- showing loading message -->
    <div v-if="isLoading" class="mt-6 p-4 bg-blue-50 rounded-lg text-blue-700 text-center">
      <p>Executing your query, please wait...</p>
    </div>

    <!-- showing execution time -->
    <div v-if="executionTime" class="mt-6 p-4 bg-blue-50 rounded-lg">
      <h2 class="text-xl font-semibold text-blue-600">Execution Time</h2>
      <p class="text-gray-700">{{ executionTime }} ms</p>
    </div>

    <!-- showing optimization suggestions -->
    <div v-if="suggestions.length > 0" class="mt-6 p-4 bg-white rounded-lg shadow-lg">
      <h2 class="text-xl font-semibold text-blue-600">Optimization Suggestions</h2>
      <ul class="list-disc pl-5">
        <li v-for="suggestion in suggestions" :key="suggestion" class="text-gray-700">{{ suggestion }}</li>
      </ul>
    </div>
    
    <!-- showing results -->
    <div v-if="results" class="mt-6 p-4 bg-white rounded-lg shadow-lg">
      <h2 class="text-xl font-semibold mb-4 text-blue-600">Results</h2>
      <div class="overflow-x-auto">
        <table class="min-w-full bg-white border border-gray-200">
          <thead>
            <tr class="bg-blue-50">
              <th v-for="(value, key) in results[0]" :key="key" class="p-3 border border-gray-200 text-left text-blue-600">{{ key }}</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(row, index) in results" :key="index" class="hover:bg-gray-50 transition duration-200">
              <td v-for="value in row" :key="value" class="p-3 border border-gray-200 text-gray-700">{{ value }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <!-- ah -->
    
    <!-- showing query plan -->
    <div v-if="queryPlan" class="mt-6 p-4 bg-white rounded-lg shadow-lg">
      <h2 class="text-xl font-semibold text-blue-600">Query Plan</h2>
      <pre class="bg-blue-50 p-3 rounded-md overflow-auto text-gray-700">{{ queryPlan }}</pre>
    </div>
    
    <!-- showing error message, if any -->
    <div v-if="error" class="mt-6 p-4 bg-red-50 text-red-700 rounded-lg">
      <h2 class="text-xl font-semibold">Error</h2>
      <p>{{ error }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import "tailwindcss";

export default {
  data() {
    return {
      query: '',
      results: null,
      executionTime: null,
      queryPlan: null,
      suggestions: [],
      error: null,
      isLoading: false
    };
  },
  methods: {
    // query execution function
    async executeQuery() {
      this.isLoading = true;
      this.error = null;
      try {
        // sending query to the backend
        const response = await axios.post(import.meta.env.VITE_API_URL, { query: this.query });
        this.results = response.data.results;
        this.executionTime = response.data.executionTime;
        this.queryPlan = response.data.queryPlan;
        this.suggestions = response.data.suggestions;
      } catch (err) {
        this.error = err.response?.data?.error || err.message;
        this.results = null;
        this.executionTime = null;
        this.queryPlan = null;
        this.suggestions = [];
      } finally {
        this.isLoading = false;
      }
    },
    // refresh page function
    refresh() {
      this.query = '';
      this.results = null;
      this.executionTime = null;
      this.queryPlan = null;
      this.suggestions = [];
      this.error = null;
    }
  }
}
</script>

<style>
.container {
  max-width: 1200px;
  margin: auto;
}
table {
  width: 100%;
  border-collapse: collapse;
}
th, td {
  text-align: left;
  padding: 12px;
  border: 1px solid #ddd;
}
th {
  background-color: #f4f4f4;
}
</style>