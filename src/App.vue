<script setup>
  import {ref} from 'vue';

  const name = ref("");
  const email = ref("");
  const phone = ref("");
  const users = ref([]);
  const selectedUser = ref(null);

  const addUser = () => {
    if (validateUser(email.value)) {
      let id = users.value.length;
      users.value.push({
        id: ++id,
        name: name.value,
        email: email.value,
        phone: phone.value
      });

      name.value = email.value = phone.value = null;
    }
  };

  const validateUser = (new_email) => {
    let index = users.value.findIndex((u) => u.email === new_email);
    if (index !== -1) {  // If email already exists
      alert("User with this email already exists!");
      return false;
    }
    return true;
  };

  const selectUser = (user) => {
    selectedUser.value = user;
    name.value = user.name;
    email.value = user.email;
    phone.value = user.phone;
  };

  const updateUser = () => {
    if (selectedUser.value) {
      let index = users.value.findIndex((u) => u.id === selectedUser.value.id);
      if (index !== -1) {
        users.value[index].name = name.value;
        users.value[index].email = email.value;
        users.value[index].phone = phone.value;
      }

      name.value = email.value = phone.value = selectedUser.value = null;
    }
  };

  const removeUser = (user) => {
    if (confirm(`Are you sure you want to delete ${user.name}?`)) {
      users.value = users.value.filter((u) => u.id !== user.id);
    }
  };

</script>

<template>
  <div class="container-fluid">
    <div class="user-form p-2 w-50">
      <h2 :class="{'text-primary': !selectedUser, 'text-success': selectedUser}">
        {{ selectedUser ? "UPDATE USER" : "ADD USER" }}
      </h2>
      <!-- User Form -->
      <form class="user-form" @submit.prevent="selectedUser ? updateUser() : addUser()">
        <div>
          <input v-model="name" type="text" placeholder="Name" class="form-control mb-2" required />
          <input v-model="email" type="email" placeholder="Email" class="form-control mb-2" required />
          <input v-model="phone" type="Number" class="form-control mb-2" placeholder="Phone Number" required />
        </div>
        <div class="d-flex justify-content-end">
          <button v-if="!selectedUser" class="btn btn-primary"><i class="bi bi-plus-circle"></i></button>
          <button v-else class="btn btn-success"><i class="bi bi-pencil-square"></i></button>
        </div>
      </form>
    </div>
    <br><hr>
    <!-- User Cards -->
    <div class="user-list p-2">
      <div  @click="selectUser(user)" v-for="(user, index) in users" :key="index" class="d-flex justify-content-between align-items-center p-2 border-bottom">
        <strong>{{ user.name }}</strong>
        <span>{{ user.email }}</span>
        <span>{{ user.phone }}</span>
        <button class="btn btn-danger btn-sm ms-1" @click.stop="removeUser(user)">
          <i class="bi bi-trash-fill"></i>
        </button>
      </div>
    </div>
  </div>

</template>


<style scoped>

</style>
