<template>
  <div class="modal" v-if="show">
    <div class="modal-content">
      <p>Вы уверены, что хотите удалить пользователя?</p>
      <button class="modal-button-confirm" @click="confirmDelete">Да</button>
      <button class="modal-button-reject" @click="closeModal">Нет</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    show: {
      type: Boolean,
      required: true,
    },
    userId: {
      type: Number,
      required: false,
    },
  },
  methods: {
    closeModal() {
      this.$emit('close');
    },
    confirmDelete() {
      this.$emit('confirm-delete', this.userId);
    },
  },
};
</script>

<style scoped>
.modal {
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.4);
}

.modal-content {
  background-color: #fefefe;
  padding: 20px;
  border-radius: 10px;
  text-align: center;
}

.modal-button-confirm {
  width: 30%;
  background-color: #e0e0e0;
  border-radius: 10px;
  border: none;
  padding: 10px 20px;
  color: #757575;
  cursor: pointer;
  margin: 0 20px; 
}

.modal-button-reject {
  width: 30%;
  background-color: #0cb4f1; /* Цвет для кнопки отмены */
  border-radius: 10px;
  border: none;
  padding: 10px 20px;
  color: white;
  cursor: pointer;
  margin: 0 20px; 
}
</style>
