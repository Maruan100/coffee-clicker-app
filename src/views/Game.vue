<template>
  <div class="container">
    <nav class="bar">
      <h4>{{user}}'s Café</h4>
      <router-link class="fa fa-sign-out" to="/"></router-link>
    </nav>
    <div class="box">
      <div class="info">
        <p>Counter: {{ clickerData.count }}</p>
        <p v-if="clickerData.autoClicks.amount">
          Auto coffes: {{ clickerData.autoClicks.amount }}
        </p>
      </div>
      <i class="fa fa-coffee" @click="clickerData.count++" ref="myBtn"></i>
      <button
        class="btn"
        :disabled="clickerData.count < clickerData.autoClicks.cost"
        type="button"
        @click="autoClick(false)"
      >
        Auto {{ clickerData.autoClicks.cost }}
      </button>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref, onMounted, watch } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();
let user = ref('');

let clickerData = reactive({
  count: 0,
  autoClicks: {
    amount: 0,
    baseCost: 10,
    cost: 10,
  },
});

const autoClick = (init) => {
  if (!init) {
    clickerData.autoClicks.amount++;
    clickerData.count = clickerData.count - clickerData.autoClicks.cost;
    update();
  }
  setInterval(() => {
    clickerData.count++;
  }, 100 / clickerData.autoClicks.amount);
};

const update = () => {
  const autoClickerBaseCost = clickerData.autoClicks.baseCost;
  let autoClickerCost = clickerData.autoClicks.cost;
  let numAutoClickers = clickerData.autoClicks.amount;

  autoClickerCost = autoClickerBaseCost + autoClickerBaseCost * numAutoClickers;
  clickerData.autoClicks.cost = autoClickerCost;
};

onMounted(() => {
  user.value = router.currentRoute.value.query.user;
  if(!user.value){
    router.push({ path: "/"});
  }
  const getUserData = localStorage.getItem(user.value);
  Object.assign(clickerData, JSON.parse(getUserData));

  if (clickerData.autoClicks.amount) {
    autoClick(true);
  }
});

watch(clickerData, (newVal) => {
  localStorage.setItem(user.value, JSON.stringify(newVal));
});
</script>
