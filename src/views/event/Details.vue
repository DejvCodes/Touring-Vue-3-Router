<script setup>
  import { defineProps } from 'vue';

  const props = defineProps(['event']);
</script>

<template>
  <p>{{ event.time }} on {{ event.date }} @ {{ event.location }}</p>
  <p>{{ event.description }}</p>
</template>