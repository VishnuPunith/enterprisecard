<template>
  <div class="plans-table">
    <h3 class="table-title">
      Customised Solutions for the School's Specific needs
    </h3>
    <div class="plans-container">
      <el-table
        v-if="plans.length > 0"
        :data="featureKeys.map((key) => ({ feature: key }))"
        class="custom-table"
      >
        <el-table-column
          prop="feature"
          label="Features"
          align="center"
        ></el-table-column>

        <!-- Column for Student Plan -->
        <el-table-column>
          <template #header>
            <div class="header-content">
              <span>Student Plan</span>
              <div class="recommended-label">Recommended</div>
            </div>
          </template>
          <template v-slot="scope">
            <transition name="fade">
              <el-icon
                v-if="plans[0].features[scope.row.feature]"
                class="el-icon-check"
              ></el-icon>
              <el-icon v-else class="el-icon-close"></el-icon>
            </transition>
          </template>
        </el-table-column>

        <!-- Column for Enterprise Plan -->
        <el-table-column label="Enterprise Plan" align="center">
          <template v-slot="scope">
            <transition name="fade">
              <el-icon
                v-if="plans[1].features[scope.row.feature]"
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
    };
  },
  async mounted() {
    try {
      const response = await axios.get("/plans.json");
      this.plans = response.data;
      const allFeatures = new Set();
      this.plans.forEach((plan) => {
        Object.keys(plan.features).forEach((feature) =>
          allFeatures.add(feature)
        );
      });
      this.featureKeys = Array.from(allFeatures);
    } catch (error) {
      console.error("Error fetching plans:", error);
    }
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

.custom-table ::v-deep th,
.custom-table ::v-deep td {
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
  font-size: 7px;
  padding: 0 6px;
  border-radius: 5px;
  margin-bottom: 3px;
}

.el-table-column__header {
  display: flex;
  align-items: center;
}

.el-table {
  font-size: 12px;
  height: auto;
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

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}

/* Mobile responsiveness */
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
    font-size: 5px;
    padding: 3px 5px;
    margin-bottom: 2px;
  }

  .el-table {
    font-size: 10px;
  }

  .el-icon-check,
  .el-icon-close {
    font-size: 16px;
  }

  .custom-table ::v-deep th,
  .custom-table ::v-deep td {
    padding: 5px 2px;
  }
}

@media (max-width: 480px) {
  .header-content {
    font-size: 12px;
  }

  .recommended-label {
    font-size: 4px;
    padding: 2px 4px;
  }

  .el-table {
    font-size: 9px;
  }

  .el-icon-check,
  .el-icon-close {
    font-size: 14px;
  }

  .custom-table ::v-deep th,
  .custom-table ::v-deep td {
    padding: 4px 2px;
  }
}
</style>
