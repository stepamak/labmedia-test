<template>
    <div class="search-bar">
        <div class="input-container">
            <img :src="findIcon" alt="Search" class="search-icon" />
            <input
                type="text"
                v-model="searchQuery"
                placeholder="Поиск по имени или e-mail"
                @input="updateSearchQuery" 
            />
        </div>
        <button class="clear-filter" @click="clearFilter">
            <img :src="clearIcon" alt="Clear" class="clear-icon" />
            Очистить фильтр
        </button>
    </div>
</template>

<script>
import clearIcon from '../assets/clear_icon.jpg'; 
import findIcon from '../assets/find_icon.jpg'; 

export default {
    data() {
        return {
            clearIcon,
            findIcon,
            searchQuery: '',
        };
    },
    methods: {
        clearFilter() {
            this.searchQuery = '';
            this.$emit('update-search', this.searchQuery); 
        },
        updateSearchQuery() {
            this.$emit('update-search', this.searchQuery); 
        },
    },
};
</script>

<style scoped>
  * {
    font-family: 'Inter', sans-serif; 
  }
  span {
    color:#9EAAB4;
    font-size: 16px;
  }
  .search-bar {
    max-width: auto;
    margin: auto;
    background: #fff;
    padding: 20px;
    border-radius: 15px;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
  }
  .input-container {
    position: relative; /* Make the container relative for absolute positioning */
    display: flex; /* Flexbox layout to align items */
    align-items: center; /* Center align items vertically */
  }
  
  input[type="text"] {
    padding: 10px 10px 10px 40px; 
    width: 95%;
    border: none;
    border-radius: 5px;
    background-color: #eceff0;
  }
  .search-icon {
    position: absolute; 
    left: 10px; 
    width: 25px; 
    height: 25px; 
    opacity: 0.5;
  }  
  button.clear-filter {
    font-size: small;
    display: flex; 
    align-items: center; 
    margin-top: 10px; 
    background: none; 
    border: none; 
    cursor: pointer; 
  }

  .clear-icon {
    width: 20px; 
    height: 20px; 
    margin-right: 8px; 
  }

  button.clear-filter:hover {
    text-decoration: underline; 
  }
</style>
  