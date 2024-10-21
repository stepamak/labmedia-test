<template>
     <div class="sort-options">
        <span>Сортировка:</span>
        <button @click="sortBy('registration_date')" class="sort-button">Дата регистрации</button>
        <button @click="sortBy('rating')"  class="sort-button">Рейтинг</button>
    </div>
    <div class="user-table-container">
      <table>
        <thead>
          <tr>
            <th>Имя пользователя</th>
            <th>E-mail</th>
            <th>Дата регистрации</th>
            <th>Рейтинг</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="user in paginatedUsers" :key="user.id">
            <td class="td-user">{{ user.username }}</td>
            <td>{{ user.email }}</td>
            <td>{{ formatDate(user.registration_date) }}</td>
            <td>{{ user.rating }}</td>
            <td>
              <button 
                class="delete-button button-delete" 
                @click="openDeleteModal(user.id)">
                <img :src="deleteIcon" alt="Delete" class="button-image" />
              </button>
            </td>
          </tr>
        </tbody>
      </table>
  
      <div class="pagination">
        <button class="button-pagination" @click="previousPage" :disabled="currentPage === 1">Назад</button>
        <span>{{ currentPage }} из {{ totalPages }}</span>
        <button class="button-pagination" @click="nextPage" :disabled="currentPage === totalPages">Вперёд</button>
      </div>
  
      <DeleteModal
        :show="showDeleteModal"
        :userId="userIdToDelete"
        @close="closeDeleteModal"
        @confirm-delete="confirmDelete"
      />
    </div>
  </template>
  
  <script>
  import deleteIcon from '../assets/delete_icon.jpg';
  import DeleteModal from './DeleteModal.vue';
  
  export default {
    props: {
      users: Array,
      searchQuery: String,
    },
    components: {
      DeleteModal, 
    },
    data() {
      return {
        deleteIcon,
        currentPage: 1,
        usersPerPage: 5,
        userIdToDelete: null,
        showDeleteModal: false,
        currentSortField: null, 
        currentSortOrder: 'asc', 
      };
    },
    computed: {
      totalPages() {
        return Math.ceil(this.filteredUsers.length / this.usersPerPage);
      },
      paginatedUsers() {
        const start = (this.currentPage - 1) * this.usersPerPage;
        return this.filteredUsers.slice(start, start + this.usersPerPage);
      },
      filteredUsers() {
        return this.users.filter(user => {
            const query = this.searchQuery.toLowerCase();
            return (
                user.username.toLowerCase().includes(query) ||
                user.email.toLowerCase().includes(query)
            );
        });
      }
    },
    methods: {
      formatDate(dateString) {
        const options = { year: 'numeric', month: '2-digit', day: '2-digit' };
        const date = new Date(dateString);
        return date.toLocaleDateString('ru-RU', options).replace(/\//g, '.');
      },
      openDeleteModal(userId) {
        this.userIdToDelete = userId;
        this.showDeleteModal = true;
      },
      confirmDelete(userId) {
        this.$emit('delete-user', userId);
        this.userIdToDelete = null; 
        this.closeDeleteModal();
    },
      closeDeleteModal() {
        this.showDeleteModal = false;
      },
      nextPage() {
        if (this.currentPage < this.totalPages) {
          this.currentPage++;
        }
      },
      previousPage() {
        if (this.currentPage > 1) {
          this.currentPage--;
        }
      },
      sortBy(field) {
        if (this.currentSortField === field) {
            this.currentSortOrder = this.currentSortOrder === 'asc' ? 'desc' : 'asc';
        } else {
            this.currentSortField = field;
            this.currentSortOrder = 'asc';
        }

        this.users.sort((a, b) => {
            let comparison = 0;
            if (field === 'registration_date') {
                comparison = new Date(a.registration_date) - new Date(b.registration_date);
            } else {
                comparison = a[field] > b[field] ? 1 : -1;
            }
            return this.currentSortOrder === 'asc' ? comparison : -comparison;
        });
      },
    },
  };
  </script>
  
  <style scoped>
  * {
    font-family: 'Inter', sans-serif; 
  }
  .user-table-container {
    max-width: auto;
    margin: auto;
    background: #fff;
    padding: 20px;
    border-radius: 15px;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
  }
  
  h1 {
    text-align: center;
    margin-bottom: 20px;
  }
  
  .pagination {
    display: flex;
    justify-content: space-between;
    margin-top: 20px;
  }
  
  table {
    width: 100%;
    border-collapse: separate;
    border-spacing: 0 10px; 
  }
  
  th, td {
    padding: 15px;
    text-align: left;
    color: #9EAAB4;
  }
  .td-user {
    color: #0cb4f1;
    font-weight: bold;
  }
  
  .delete-button {
    background: none;
    border: none;
    padding: 0;
    cursor: pointer;
  }
  
  .button-image {
    width: 50%;
    height: 50%;
    object-fit: cover;
  }
  span {
    font-size: larger;
    font-weight: bold;
    color: #9EAAB4;
  }
  .button-pagination {
    font-size: larger;
    font-weight: bold;
    color: #9EAAB4;
    background: none;
    border: none;
    padding: 0;
    cursor: pointer;
  }
  .sort-options {
    padding-top: 70px;
    padding-bottom: 10px;
  }
  .sort-button {
    font-size: 16px;
    color:#9EAAB4;
    background: none; 
    border: none; 
    cursor: pointer;             
    border-radius: 5px;         
    margin-right: 10px;        
    cursor: pointer;          
  }

  button.sort-button:hover {
    color: #000;
    text-decoration: underline dashed ; 
  }
  </style>
  