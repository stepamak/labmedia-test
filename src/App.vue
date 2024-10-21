<template>
  <div id="app">
      <h1>Список пользователей</h1>
      <SearchSort @update-search="updateSearchQuery"/>
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
          if (!this.searchQuery) return this.users;
          const query = this.searchQuery.toLowerCase();
          return this.users.filter(user => 
              user.username.toLowerCase().includes(query) ||
              user.email.toLowerCase().includes(query)
          );
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
      removeUser(userId) {
          this.users = this.users.filter(user => user.id !== userId);
      },
      updateSearchQuery(query) {
          this.searchQuery = query;
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