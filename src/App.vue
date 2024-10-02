<template>
  <div class="container">
    <EmployeeList
      :employees="employees"
      :searchQuery="searchQuery"
      @refresh="refresh"
      @edit="editEmployee"
      @confirmBlock="confirmBlock"
      @confirmDelete="confirmDelete"
    />

    <EmployeeForm
      v-if="showForm"
      :formMode="formMode"
      :employee="currentEmployee"
      @close="closeForm"
      @submit="submitForm"
    />

    <ConfirmationModal
      v-if="showBlockModal"
      title="Cảnh báo"
      message="Bạn có chắc chắn muốn chặn tài khoản này?"
      @cancel="closeBlockModal"
      @confirm="blockEmployee"
    />

    <ConfirmationModal
      v-if="showDeleteModal"
      title="Cảnh báo"
      message="Bạn có chắc chắn muốn xóa tài khoản này?"
      @cancel="closeDeleteModal"
      @confirm="deleteConfirmed"
    />
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import EmployeeList from "./components/EmployeeList.vue";
import EmployeeForm from "./components/EmployeeForm.vue";
import ConfirmationModal from "./components/ConfirmationModal.vue";

const employees = ref(JSON.parse(localStorage.getItem("employees")) || []);
const searchQuery = ref("");
const showForm = ref(false);
const currentEmployee = ref(null);
const formMode = ref("add");
const showBlockModal = ref(false);
const showDeleteModal = ref(false);
let currentEmail = ref(null);

const openForm = () => {
  showForm.value = true;
  formMode.value = "add";
  currentEmployee.value = null;
};

const closeForm = () => {
  showForm.value = false;
};

const submitForm = (employee) => {
  if (formMode.value === "add") {
    employees.value.push({ ...employee, status: "active" });
  } else {
    const index = employees.value.findIndex(
      (emp) => emp.email === employee.email
    );
    if (index !== -1) {
      employees.value[index] = { ...employee };
    }
  }
  localStorage.setItem("employees", JSON.stringify(employees.value));
  closeForm();
};

const editEmployee = (employee) => {
  currentEmployee.value = employee;
  formMode.value = "edit";
  showForm.value = true;
};

const confirmBlock = (email) => {
  currentEmail.value = email;
  showBlockModal.value = true;
};

const blockEmployee = () => {
  const employee = employees.value.find(
    (emp) => emp.email === currentEmail.value
  );
  if (employee) {
    employee.status = employee.status === "active" ? "inactive" : "active";
  }
  showBlockModal.value = false;
  localStorage.setItem("employees", JSON.stringify(employees.value));
};

const confirmDelete = (email) => {
  currentEmail.value = email;
  showDeleteModal.value = true;
};

const deleteConfirmed = () => {
  employees.value = employees.value.filter(
    (emp) => emp.email !== currentEmail.value
  );
  showDeleteModal.value = false;
  localStorage.setItem("employees", JSON.stringify(employees.value));
};

const closeBlockModal = () => {
  showBlockModal.value = false;
};

const closeDeleteModal = () => {
  showDeleteModal.value = false;
};

const refresh = () => {
  searchQuery.value = "";
};
</script>

<style>
/* Add your styles here */
</style>
