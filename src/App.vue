<template>
  <div id="app">
      <h1>Список пользователей</h1>
      <SearchSort @update-search="updateSearch" @sort-by="sortUsers" />
      <UserTable 
          :users="filteredUsers" 
          @delete-user="removeUser" 
          :searchQuery="searchQuery" 
      />
  </div>
</template>

<script>
import UserTable from './components/UserTable.vue';
import SearchSort from './components/SearchSort.vue';

export default {
  components: {
      UserTable,
      SearchSort,
  },
  data() {
      return {
          users: [],
          searchQuery: '',
      };
  },
  computed: {
      filteredUsers() {
          return this.users.filter(user => {
              const query = this.searchQuery.toLowerCase();
              return (
                  user.username.toLowerCase().includes(query) ||
                  user.email.toLowerCase().includes(query)
              );
          });
      },
  },
  methods: {
      async fetchUsers() {
          try {
              const response = await fetch('https://5ebbb8e5f2cfeb001697d05c.mockapi.io/users');
              this.users = await response.json();
          } catch (error) {
              console.error("Ошибка при загрузке пользователей:", error);
          }
      },
      updateSearch(query) {
          this.searchQuery = query;
      },
      sortUsers(field) {
          if (field === 'registrationDate') {
              this.users.sort((a, b) => new Date(b.registration_date) - new Date(a.registration_date));
          } else if (field === 'rating') {
              this.users.sort((a, b) => b.rating - a.rating);
          }
      },
      removeUser(userId) {
          this.users = this.users.filter(user => user.id !== userId);
      },
  },
  mounted() {
      this.fetchUsers(); 
  },
};
</script>
<style>
* {
    box-sizing: inherit;
}
body {
    font-family: 'Inter', sans-serif; 
    margin: 0; 
    padding: 0; 
    box-sizing: border-box;
    background-color: #f7f7f7;
}
#app {
  font-family: 'Inter', sans-serif;
  margin: 0;
  padding-left: 20px;
  padding-right: 20px;
}
</style>