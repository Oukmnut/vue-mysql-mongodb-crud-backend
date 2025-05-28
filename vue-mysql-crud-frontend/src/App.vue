<template>
  <div class="min-h-screen bg-gray-100 flex flex-col items-center py-10 px-4">
    <div class="bg-white p-8 rounded-2xl shadow-xl w-full max-w-xl">
      <h1 class="text-2xl font-bold text-center mb-6 text-blue-600">User Management</h1>

      <form @submit.prevent="saveUser" class="space-y-4">
        <input v-model="form.name" type="text" placeholder="Name"
          class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500" />
        <input v-model="form.email" type="email" placeholder="Email"
          class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500" />
        <button type="submit"
          class="w-full bg-blue-600 text-white py-3 rounded-lg hover:bg-blue-700 transition">
          {{ form.id ? 'Update User' : 'Create User' }}
        </button>
      </form>

      <hr class="my-6 border-gray-300" />

      <ul class="space-y-4">
        <li v-for="user in users" :key="user.id"
            class="flex items-center justify-between bg-gray-50 p-4 rounded-lg shadow-sm">
          <div>
            <p class="font-medium">{{ user.name }}</p>
            <p class="text-sm text-gray-500">{{ user.email }}</p>
          </div>
          <div class="flex gap-2">
            <button @click="editUser(user)"
              class="text-sm px-3 py-1 bg-yellow-400 text-white rounded hover:bg-yellow-500">
              Edit
            </button>
            <button @click="deleteUser(user.id)"
              class="text-sm px-3 py-1 bg-red-500 text-white rounded hover:bg-red-600">
              Delete
            </button>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      users: [],
      form: { id: null, name: '', email: '' }
    };
  },
  methods: {
    fetchUsers() {
      axios.get('http://localhost:3000/users')
        .then(res => this.users = res.data);
    },
    saveUser() {
      const request = this.form.id
        ? axios.put(`http://localhost:3000/users/${this.form.id}`, this.form)
        : axios.post('http://localhost:3000/users', this.form);

      request.then(() => {
        this.fetchUsers();
        this.form = { id: null, name: '', email: '' };
      });
    },
    editUser(user) {
      this.form = { ...user };
    },
    deleteUser(id) {
      axios.delete(`http://localhost:3000/users/${id}`)
        .then(() => this.fetchUsers());
    }
  },
  mounted() {
    this.fetchUsers();
  }
};
</script>
