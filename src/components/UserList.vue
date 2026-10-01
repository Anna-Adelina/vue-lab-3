<script setup lang="ts">
import { computed, ref } from 'vue'
import usersData from '../data/users.json'
import type { User } from '../types/user'
import UserCard from './UserCard.vue'

type GenderFilter = 'all' | 'male' | 'female'
type AgeFilter = 'all' | 'adult'
type SortKey = 'none' | 'nameAsc' | 'nameDesc' | 'ageAsc' | 'ageDesc'

const users = ref<User[]>(usersData as User[])

const genderFilter = ref<GenderFilter>('all')
const ageFilter = ref<AgeFilter>('all')
const sortKey = ref<SortKey>('none')

const visibleUsers = computed(() => {
  const result = users.value.filter((u) => {
    const genderOk = genderFilter.value === 'all' || u.gender === genderFilter.value
    const ageOk = ageFilter.value === 'all' || u.dob.age >= 18
    return genderOk && ageOk
  })

  switch (sortKey.value) {
    case 'nameAsc':
      result.sort((a, b) => a.name.first.localeCompare(b.name.first, 'uk'))
      break
    case 'nameDesc':
      result.sort((a, b) => b.name.first.localeCompare(a.name.first, 'uk'))
      break
    case 'ageAsc':
      result.sort((a, b) => a.dob.age - b.dob.age)
      break
    case 'ageDesc':
      result.sort((a, b) => b.dob.age - a.dob.age)
      break
  }
  return result
})

function resetAll(): void {
  genderFilter.value = 'all'
  ageFilter.value = 'all'
  sortKey.value = 'none'
}
</script>

<template>
  <section class="users">
    <div class="toolbar">
      <div class="toolbar__group">
        <button :class="{ active: genderFilter === 'all' }" @click="genderFilter = 'all'">Всі</button>
        <button :class="{ active: genderFilter === 'male' }" @click="genderFilter = 'male'">Чоловіки</button>
        <button :class="{ active: genderFilter === 'female' }" @click="genderFilter = 'female'">Жінки</button>
      </div>

      <div class="toolbar__group">
        <button :class="{ active: ageFilter === 'all' }" @click="ageFilter = 'all'">Всі віки</button>
        <button :class="{ active: ageFilter === 'adult' }" @click="ageFilter = 'adult'">18 +</button>
      </div>

      <div class="toolbar__group">
        <button :class="{ active: sortKey === 'nameAsc' }" @click="sortKey = 'nameAsc'">Ім'я ↑</button>
        <button :class="{ active: sortKey === 'nameDesc' }" @click="sortKey = 'nameDesc'">Ім'я ↓</button>
        <button :class="{ active: sortKey === 'ageAsc' }" @click="sortKey = 'ageAsc'">Вік ↑</button>
        <button :class="{ active: sortKey === 'ageDesc' }" @click="sortKey = 'ageDesc'">Вік ↓</button>
      </div>

      <button class="toolbar__reset" @click="resetAll">Очистити все</button>
    </div>

    <p v-if="visibleUsers.length === 0" class="users__empty">Список юзерів пустий</p>

    <div v-else class="users__list">
      <UserCard v-for="user in visibleUsers" :key="user.id" :user="user" />
    </div>
  </section>
</template>

<style scoped>
.toolbar {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
  margin-bottom: 20px;
}
.toolbar__group {
  display: flex;
  gap: 6px;
}
button.active {
  background: #42b883;
  color: #fff;
}
.users__list {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
}
.users__empty {
  font-size: 1.2rem;
}
</style>