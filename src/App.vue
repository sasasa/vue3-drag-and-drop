<script setup>
import { ref } from "vue";

const users = ref([]);
const dragIndex = ref(null);

const getUsers = async () => {
  const response = await fetch("https://jsonplaceholder.typicode.com/users");
  const data = await response.json();
  users.value = data;
};

getUsers();

const dragStart = (index) => {
  dragIndex.value = index;
};

const dragEnter = (index) => {
  if (index === dragIndex) return;
  const deleteElement = users.value.splice(dragIndex.value, 1)[0];
  users.value.splice(index, 0, deleteElement);
  dragIndex.value = index;
};

const dragEnd = (ev) => {
  // 並び替え後のusers配列をサーバに送信する処理を追加する
  dragIndex.value = null;
};
</script>

<template>
  <table>
    <thead>
      <tr>
        <th>ID</th>
        <th>名前</th>
        <th>ユーザ名</th>
        <th>Email</th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="(user, index) in users"
        :key="user.id"
        :draggable="true"
        @dragstart="dragStart(index)"
        @dragenter="dragEnter(index)"
        @dragover.prevent
        @dragend="dragEnd"
        :class="index === dragIndex ? 'dragging' : ''"
      >
        <td>{{ user.id }}</td>
        <td>{{ user.name }}</td>
        <td>{{ user.username }}</td>
        <td>{{ user.email }}</td>
      </tr>
    </tbody>
  </table>
</template>

<style>
table,
th,
td {
  border: 1px solid black;
  border-collapse: collapse;
  padding: 1em;
}
.dragging {
  background-color: #aaa;
}
</style>