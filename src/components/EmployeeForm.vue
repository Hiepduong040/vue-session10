<template>
  <div class="overlay">
    <form class="form" @submit.prevent="submit">
      <div class="d-flex justify-content-between align-items-center">
        <h4>
          {{
            formMode === "add" ? "Thêm mới nhân viên" : "Chỉnh sửa nhân viên"
          }}
        </h4>
        <i class="fa-solid fa-xmark" @click="$emit('close')"></i>
      </div>
      <div>
        <label class="form-label" for="userName">Họ và tên</label>
        <input
          v-model="form.name"
          id="userName"
          type="text"
          class="form-control"
          required
        />
        <small v-if="errors.name" class="text-danger">{{ errors.name }}</small>
      </div>
      <div>
        <label class="form-label" for="dateOfBirth">Ngày sinh</label>
        <input
          v-model="form.birthDate"
          id="dateOfBirth"
          type="date"
          class="form-control"
          required
        />
        <small v-if="errors.birthDate" class="text-danger">{{
          errors.birthDate
        }}</small>
      </div>
      <div>
        <label class="form-label" for="email">Email</label>
        <input
          v-model="form.email"
          id="email"
          type="email"
          class="form-control"
          required
        />
        <small v-if="errors.email" class="text-danger">{{
          errors.email
        }}</small>
      </div>
      <div>
        <label class="form-label" for="address">Địa chỉ</label>
        <textarea
          v-model="form.address"
          class="form-control"
          id="address"
          rows="3"
        ></textarea>
        <small v-if="errors.address" class="text-danger">{{
          errors.address
        }}</small>
      </div>
      <div>
        <button class="w-100 btn btn-primary">
          {{ formMode === "add" ? "Thêm mới" : "Lưu thay đổi" }}
        </button>
      </div>
    </form>
  </div>
</template>

<script setup>
import { ref, watch } from "vue";

const props = defineProps(["employee", "formMode"]);
const emit = defineEmits(["close", "submit"]);

const form = ref({
  name: "",
  birthDate: "",
  email: "",
  address: "",
});

const errors = ref({});

watch(
  () => props.employee,
  (newVal) => {
    if (newVal) {
      form.value = { ...newVal };
    }
  }
);

const submit = () => {
  // Reset errors
  errors.value = {};

  // Basic validation (expand as needed)
  if (!form.value.name) errors.value.name = "Họ và tên không được để trống.";
  if (!form.value.email) errors.value.email = "Email không được để trống.";
  if (!form.value.birthDate)
    errors.value.birthDate = "Ngày sinh không được để trống.";

  if (Object.keys(errors.value).length === 0) {
    emit("submit", form.value);
  }
};
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
.form {
  background-color: #fff;
  width: 500px;
  padding: 20px 24px;
  border-radius: 4px;
  display: flex;
  flex-direction: column;
  gap: 12px;
}
.form-label {
  font-weight: 600;
  margin-bottom: 8px;
}
.error {
  color: red !important;
}
</style>
