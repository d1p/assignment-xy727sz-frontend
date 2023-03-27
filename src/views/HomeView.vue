<template>
  <div>
    <h2>List of Dogs</h2>
    <ul>
      <li v-for="dog in dogs" :key="dog.id">{{ dog.name }}</li>
    </ul>
  </div>
  <nav v-if="totalPages > 1">
    <ul class="pagination">
      <li :class="{ disabled: currentPage === 1 }">
        <a @click.prevent="fetchDogs(currentPage - 1)" href="#">Previous</a>
      </li>
      <li v-for="page in totalPages" :key="page" :class="{ active: page === currentPage }">
        <a @click.prevent="fetchDogs(page)" href="#">{{ page }}</a>
      </li>
      <li :class="{ disabled: currentPage === totalPages }">
        <a @click.prevent="fetchDogs(currentPage + 1)" href="#">Next</a>
      </li>
    </ul>
  </nav>
</template>

<script>
export default {
  data() {
    return {
      dogs: [],
      currentPage: 1,
      totalPages: 1
    };
  },
  mounted() {
    this.fetchDogs();
  },
  methods: {
    fetchDogs(page=1){
      fetch(`http://localhost/api/dogs/?page=${page}`, {
        headers: {
          'Content-Type': 'application/json',
          'Authorization': 'Bearer secret'
        }
      })
          .then(response => response.json())
          .then(data => {
            this.dogs = data.data.map(dog => dog.data),
                this.currentPage = data.current_page
            this.totalPages = data.last_page
          })
          .catch(error => console.error(error));
    }
  }
};
</script>