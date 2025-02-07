<template>
  <div class="plans-table">
    <h3 class="table-title">
      Customised Solutions for the School's Specific Needs
    </h3>
    <div class="plans-container">
      <!-- Skeleton loader (while loading data) -->
      <el-skeleton v-if="loading" :rows="5" class="skeleton-loader">
        <template #default>
          <el-table class="custom-table" :data="[]">
            <el-table-column label="Features" align="center"></el-table-column>
            <el-table-column
              v-for="(plan, index) in placeholderPlans"
              :key="index"
              :label="plan.label"
              align="center"
            >
              <template v-slot="scope">
                <el-skeleton :loading="loading" />
              </template>
            </el-table-column>
          </el-table>
        </template>
      </el-skeleton>

      <!-- Actual table (once data is loaded) -->
      <el-table
        v-if="!loading && plans.length > 0"
        :data="featureKeys.map((key) => ({ feature: key }))"
        class="custom-table"
      >
        <!-- Column for Features -->
        <el-table-column
          prop="feature"
          label="Features"
          align="center"
        ></el-table-column>

        <!-- Loop through plans dynamically to create a column for each -->
        <el-table-column
          v-for="(plan, index) in plans"
          :key="plan.plan"
          :label="plan.plan"
          align="center"
        >
          <template #header>
            <div class="plan-header">
              <span>{{ plan.plan }}</span>
              <!-- Display 'Recommended' label if applicable -->
              <div v-if="plan.recommended" class="recommended-label">
                Recommended
              </div>
            </div>
          </template>

          <template v-slot="scope">
            <transition name="fade">
              <el-icon
                v-if="plan.features[scope.row.feature]"
                class="el-icon-check"
              ></el-icon>
              <el-icon v-else class="el-icon-close"></el-icon>
            </transition>
          </template>
        </el-table-column>
      </el-table>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      plans: [],
      featureKeys: [],
      loading: true, // Track loading state
      placeholderPlans: [
        // Placeholder for skeleton columns
        { label: "Student Plan" },
        { label: "Enterprise Plan" },
      ],
    };
  },
  async mounted() {
    try {
      const response = await axios.get("/plans.json");
      this.plans = response.data;
      this.featureKeys = this.getFeatureKeys();
      this.loading = false; // Data has loaded, hide skeleton
    } catch (error) {
      console.error("Error fetching plans:", error);
      this.loading = false; // Hide skeleton even if there's an error
    }
  },
  methods: {
    getFeatureKeys() {
      const allFeatures = new Set();
      this.plans.forEach((plan) => {
        Object.keys(plan.features).forEach((feature) =>
          allFeatures.add(feature)
        );
      });
      return Array.from(allFeatures); // Convert the set into an array
    },
  },
};
</script>

<style scoped>
.plans-table {
  width: 100vw;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.plans-container {
  width: 90%;
  height: auto;
  padding: 20px;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 10px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.2);
}

.custom-table /deep/ th,
.custom-table /deep/ td {
  text-align: center !important;
  font-weight: bold;
  padding: 6px 0;
}

.header-content {
  display: flex;
  justify-content: center;
  gap: 3px;
}

.recommended-label {
  background: gold;
  color: black;
  font-weight: bold;
  font-size: 8px;
  padding: 2px 6px;
  border-radius: 5px;
  margin-bottom: 3px;
  width: fit-content;
}

.plan-header {
  display: flex;
  align-items: center;
  justify-content: center;
}

.skeleton-loader .custom-table {
  background: #f5f5f5;
  border-radius: 10px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.2);
}

.el-icon-check {
  color: #00ff00;
  font-size: 20px;
  transition: transform 0.3s ease-in-out;
}

.el-icon-check:hover {
  transform: scale(1.2);
}

.el-icon-close {
  color: #ff0000;
  font-size: 20px;
  transition: transform 0.3s ease-in-out;
}

.el-icon-close:hover {
  transform: scale(1.2);
}

/* Fix Element UI Table styles from affecting other components */
.custom-table {
  font-size: 12px;
  height: auto;
}

.custom-table /deep/ th,
.custom-table /deep/ td {
  padding: 6px 8px;
}

/* Responsive styles */
@media (max-width: 768px) {
  .plans-container {
    width: 85%;
    padding: 15px;
  }
  .table-title {
    font-size: smaller;
  }
  .header-content {
    flex-direction: column;
    align-items: center;
  }

  .recommended-label {
    font-size: 7px;
    padding: 1px 4px;
  }

  .custom-table {
    font-size: 10px;
  }

  .el-icon-check,
  .el-icon-close {
    font-size: 16px;
  }

  .custom-table /deep/ th,
  .custom-table /deep/ td {
    padding: 5px 2px;
  }
}

@media (max-width: 480px) {
  .header-content {
    font-size: 12px;
  }

  .recommended-label {
    font-size: 6px;
    padding: 1px 3px;
  }

  .custom-table {
    font-size: 9px;
  }

  .el-icon-check,
  .el-icon-close {
    font-size: 14px;
  }

  .custom-table /deep/ th,
  .custom-table /deep/ td {
    padding: 4px 2px;
  }
}
</style>
