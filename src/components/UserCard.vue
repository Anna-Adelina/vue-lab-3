<script setup lang="ts">
import { computed, ref } from 'vue'
import type { User } from '../types/user'

const props = defineProps<{
  user: User
}>()

const showDetails = ref(false)

const fullName = computed(() => `${props.user.name.first} ${props.user.name.last}`)

const birthDate = computed(() =>
  new Date(props.user.dob.date).toLocaleDateString('uk-UA'),
)

const ageGroup = computed(() => {
  const age = props.user.dob.age
  if (age < 18) return 'minor'
  if (age <= 30) return 'young'
  if (age <= 50) return 'adult'
  return 'senior'
})
</script>

<template>
  <article
    class="user-card"
    :class="{
      'user-card--minor': ageGroup === 'minor',
      'user-card--young': ageGroup === 'young',
      'user-card--adult': ageGroup === 'adult',
      'user-card--senior': ageGroup === 'senior',
    }"
  >
    <img
      class="user-card__photo"
      :src="user.picture"
      :alt="`Фото: ${fullName}`"
    />

    <h2 class="user-card__name">{{ user.name.title }} {{ fullName }}</h2>

    <ul class="user-card__info">
      <li>Стать: {{ user.gender === 'male' ? 'чоловік' : 'жінка' }}</li>
      <li>Місто: {{ user.location.city }}, {{ user.location.country }}</li>
      <li>Email: {{ user.email }}</li>
      <li>Телефон: {{ user.phone }}</li>
      <li>Дата народження: {{ birthDate }}</li>
      <li v-if="user.dob.age > 18">Вік: {{ user.dob.age }}</li>
    </ul>

    <h3 class="user-card__subtitle">Хобі</h3>
    <ul class="user-card__hobbies">
      <li v-for="hobby in user.hobbies" :key="hobby">{{ hobby }}</li>
    </ul>

    <button type="button" @click="showDetails = !showDetails">
      {{ showDetails ? 'Сховати деталі' : 'Показати деталі' }}
    </button>
    <p v-show="showDetails" class="user-card__details">{{ user.details }}</p>
  </article>
</template>

<style scoped>
.user-card {
  box-sizing: border-box;
  width: 340px;
  padding: 16px;
  border: 2px solid #ccc;
  border-radius: 12px;
  background: #fff;
}

.user-card__photo {
  width: 100%;
  height: 280px;
  object-fit: cover;
  border-radius: 8px;
  margin: 0 auto;
  object-position: 50% 20%;
}

.user-card__name {
  margin: 12px 0 8px;
  font-size: 1.2rem;
}

.user-card__subtitle {
  margin: 12px 0 4px;
  font-size: 1rem;
}

.user-card__info,
.user-card__hobbies {
  margin: 0;
  padding-left: 18px;
}

.user-card__details {
  margin-top: 10px;
}

/* кольори за віком: можна змінити на свої */
.user-card--minor  { background: #e3f2fd; border-color: #64b5f6; }
.user-card--young  { background: #e8f5e9; border-color: #81c784; }
.user-card--adult  { background: #fff8e1; border-color: #ffd54f; }
.user-card--senior { background: #fce4ec; border-color: #f06292; }
</style>