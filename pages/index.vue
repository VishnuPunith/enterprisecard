<template>
  <div class="cards-list">
    <DynamicCard v-for="(card, index) in cards" :key="index" :card="card" />
  </div>
</template>

<script>
import DynamicCard from '@/components/DynamicCard.vue'

export default {
  components: {
    DynamicCard,
  },
  async asyncData({ $axios }) {
    try {
      const cards = await $axios.$get('/cards.json')
      return { cards }
    } catch (error) {
      console.error('Error loading cards:', error)
      return { cards: [] }
    }
  },
}
</script>

<style scoped>
.cards-list {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
}
</style>
