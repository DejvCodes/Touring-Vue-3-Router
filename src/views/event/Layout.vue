<script setup>
  import EventService from '@/services/EventService.js';
  import { computed, onMounted, ref } from 'vue';
  import { useRouter } from 'vue-router';
  import { defineProps } from 'vue';

  const router = useRouter();
  const props = defineProps(['id']);
  const event = ref('');
  const id = computed(() => props.id);

  onMounted(() => {
    EventService.getEvent(id.value)
      .then((response) => {
        event.value = response.data;
      })
      .catch((error) => {
        // console.log(error);
        if (error.response && error.response.status == 404) {
          router.push({
            name: '404Resource',
            params: { resource: 'event' }
          })
        } else {
          router.push({ name: 'NetworkError' })
        }
      });
  });
</script>

<template>
  <div v-if='event'>
    <h1>{{ event.title }}</h1>
    <div id='nav'>
      <router-link :to="{ name: 'EventDetails' }">Details</router-link> |
      <router-link :to="{ name: 'EventRegister' }">Register</router-link> |
      <router-link :to="{ name: 'EventEdit' }">Edit</router-link>
    </div>
    <router-view :event='event'></router-view>
  </div>
</template>