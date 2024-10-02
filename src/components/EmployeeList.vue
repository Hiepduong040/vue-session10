<template>
  <div class="container my-4">
    <header class="d-flex justify-content-between mb-3">
      <h3>Danh sách Nhân viên</h3>
      <button class="btn btn-primary" @click="$emit('addEmployee')">
        Thêm mới nhân viên
      </button>
    </header>
    <div class="d-flex align-items-center justify-content-end gap-2 mb-3">
      <input
        v-model="searchQuery"
        type="text"
        class="form-control"
        placeholder="Tìm kiếm theo email"
        style="width: 350px"
      />
      <i
        class="fa-solid fa-arrows-rotate"
        title="Refresh"
        @click="$emit('refresh')"
      ></i>
    </div>
    <table class="table table-bordered table-hover table-striped">
      <thead>
        <tr>
          <th>STT</th>
          <th>Họ và tên</th>
          <th>Ngày sinh</th>
          <th>Email</th>
          <th>Địa chỉ</th>
          <th>Trạng thái</th>
          <th colspan="2">Chức năng</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(employee, index) in filteredEmployees"
          :key="employee.email"
        >
          <td>{{ index + 1 }}</td>
          <td>{{ employee.name }}</td>
          <td>{{ employee.birthDate }}</td>
          <td>{{ employee.email }}</td>
          <td>{{ employee.address }}</td>
          <td>
            <div class="d-flex align-items-center">
              <div
                :class="
                  employee.status === 'active'
                    ? 'status status-active'
                    : 'status status-stop'
                "
              ></div>
              <span>{{
                employee.status === "active"
                  ? "Đang hoạt động"
                  : "Ngừng hoạt động"
              }}</span>
            </div>
          </td>
          <td>
            <span
              class="button button-block"
              @click="$emit('confirmBlock', employee.email)"
            >
              {{ employee.status === "active" ? "Chặn" : "Bỏ chặn" }}
            </span>
          </td>
          <td>
            <span class="button button-edit" @click="$emit('edit', employee)"
              >Sửa</span
            >
          </td>
          <td>
            <span
              class="button button-delete"
              @click="$emit('confirmDelete', employee.email)"
              >Xóa</span
            >
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref, computed, toRefs } from "vue";

const props = defineProps(["employees", "searchQuery"]);
const { employees, searchQuery } = toRefs(props);

const filteredEmployees = computed(() => {
  if (!searchQuery.value) return employees.value;
  return employees.value.filter((emp) =>
    emp.email.toLowerCase().includes(searchQuery.value.toLowerCase())
  );
});
</script>

<style scoped>
.status {
  height: 12px;
  width: 12px;
  border-radius: 50%;
  margin-right: 5px;
}
.status-active {
  background-color: green;
}
.status-stop {
  background-color: red;
}
.button {
  padding: 5px 10px;
  border-radius: 4px;
  color: #fff;
  cursor: pointer;
}
.button-edit {
  background-color: orange;
}
.button-delete {
  background-color: red;
}
.button-block {
  background-color: green;
}
</style>
