<template>
  <div class="section">
    <div class="container">
      <div class="field">
        <label class="label">Filter by Gender</label>
        <div class="control">
          <div class="select">
            <select v-model="selectedGender">
              <option value="">All</option>
              <option value="male">Male</option>
              <option value="female">Female</option>
            </select>
          </div>
        </div>
      </div>
      <div class="columns is-multiline">
        <div
          class="column is-one-quarter"
          v-for="user in filteredUsers"
          :key="user.login.uuid"
        >
          <div class="card card-user">
            <div class="card-image">
              <figure class="image is-4by3">
                <img :src="user.picture.large" alt="User's profile picture" />
              </figure>
            </div>
            <div class="card-content">
              <p class="title is-4">
                {{ user.name.first }} {{ user.name.last }}
              </p>
              <p>{{ user.email }}</p>
              <p>{{ user.phone }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed, onMounted } from "vue";
import axios from "axios";

export default {
  setup() {
    const users = ref([]);
    const selectedGender = ref("");

    const fetchUsers = async () => {
      try {
        const response = await axios.get(
          "https://randomuser.me/api/?results=50"
        );
        users.value = response.data.results;
      } catch (error) {
        console.error(error);
      }
    };

    onMounted(fetchUsers);

    const filteredUsers = computed(() => {
      if (!selectedGender.value) {
        return users.value;
      }
      return users.value.filter((user) => user.gender === selectedGender.value);
    });

    return { users, selectedGender, filteredUsers };
  },
};
</script>

<style scoped>
.card-user {
  height: 380px;
  word-break: break-all;
}
</style>
