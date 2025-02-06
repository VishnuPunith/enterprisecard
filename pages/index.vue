<template>
  <div class="container">
    <div class="plans-container">
      <PlanTable />
    </div>
    <div class="cards-list">
      <DynamicCard v-for="(card, index) in cards" :key="index" :card="card" />
    </div>
  </div>
</template>

<script>
import DynamicCard from "@/components/DynamicCard.vue";
import PlanTable from "@/components/PlanTable.vue";

export default {
  components: {
    DynamicCard,
    PlanTable,
  },
  async asyncData({ $axios }) {
    try {
      const cards = await $axios.$get("/cards.json");
      return { cards };
    } catch (error) {
      console.error("Error loading cards:", error);
      return { cards: [] };
    }
  },
};
</script>

<style scoped>
.container {
  overflow: hidden;
}
.cards-list {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
}
</style>
