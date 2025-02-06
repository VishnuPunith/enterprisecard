<template>
  <div class="card-container">
    <div class="card-main">
      <div class="">
        <div class="card-yellow"></div>
        <div class="card-blue"></div>
        <div class="card-content">
          <h2>{{ card.title }}</h2>
          <div class="badge">
            <img :src="card.image" alt="Card Image" class="card-image" />
          </div>
          <ul class="services-list">
            <li v-for="(service, index) in card.services" :key="index">
              {{ service }}
            </li>
          </ul>
          <button class="select-button">Select</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    card: Object,
  },
}
</script>

<style scoped>
.card-container {
  display: flex;
  justify-content: center;
  position: relative;
}

.card-yellow {
  width: 100%;
  height: 30%;
  background: linear-gradient(to right, #F5EA21, #F5EA21); /* Yellow gradient */
  position: absolute;
  top: 0;
  left: 0;
  z-index: -1;
  clip-path: polygon(0 0, 100% 0, 0 100%);
}

.card-blue {
  width: 100%;
  height: 30%;
  background: linear-gradient(to right, #004562, #015d7f);
  position: absolute;
  top: 0;
  left: 0;
  z-index: -1;
  clip-path: polygon(0 0, 215% 0, 0 100%);
}

.card-main {
  width: 250px;
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  text-align: center;
  position: relative;
  z-index: 2;
  margin-top: 20px;
}

.card-content {
  padding: 15px;
}

.card-image {
  width: 100%;
  border-radius: 10px 10px 0 0;
}

.card-content h2 {
  font-size: 2em;
  margin: 10px 0;
  color: white;
}

.badge {
  width: 60%;
  font-weight: bold;
  display: inline-block;
}

.services-list {
  list-style: none;
  padding: 0;
  margin:0;
  text-align: left;
  font-size: smaller;
}

.services-list li {
  padding: 5px 0;
}

.select-button {
  background: #007bff;
  color: white;
  padding: 8px 15px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 15px;
}
</style>
