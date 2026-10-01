<script setup lang="ts">
import { ref } from 'vue'
import usersData from '../data/users.json'
import type { User } from '../types/user'
import UserCard from './UserCard.vue'

const users = ref<User[]>(usersData as User[])
</script>

<template>
  <section class="users">
    <UserCard v-if="users[0]" :user="users[0]" />
  </section>
</template>