<template>
    <div class="overlay">
      <div class="modal-custom">
        <div class="modal-title">
          <h4>{{ title }}</h4>
          <i class="fa-solid fa-xmark" @click="$emit('cancel')"></i>
        </div>
        <div class="modal-body-custom">
          <span>{{ message }}</span>
        </div>
        <div class="modal-footer-custom">
          <button class="btn btn-light" @click="$emit('cancel')">Hủy</button>
          <button class="btn btn-danger" @click="$emit('confirm')">Xác nhận</button>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  const props = defineProps(['title', 'message']);
  </script>
  
  <style scoped>
  .overlay {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .modal-custom {
    background-color: #fff;
    padding: 20px 24px;
    border-radius: 4px;
    width: 400px;
  }
  
  .modal-title {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  
  .modal-body-custom {
    padding: 20px 0;
  }
  
  .modal-footer-custom {
    display: flex;
    justify-content: end;
    gap: 8px;
  }
  </style>
  