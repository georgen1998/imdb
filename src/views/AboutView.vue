<template>
  <div class="about">
    <h1>Movies You Like </h1>
    <div v-if="$likes.length" style="padding:50px">
      <Card v-for="obj in $likes" :obj="obj" :user="$user"></Card>
    </div>
    <div else><p>none.</p></div>
  </div>
</template>

<script>
import Card from '@/components/Card.vue'

export default {
  name: "About",
  components: {
    Card
  }
}
</script>