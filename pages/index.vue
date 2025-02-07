<template>
  <div class="layout-wrapper">
    <div class="table-section">
      <PlanTable />
    </div>
    <div class="cards-wrapper">
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
.layout-wrapper {
  overflow: hidden;
}
.cards-wrapper {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
}
</style>
