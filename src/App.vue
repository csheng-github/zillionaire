<script setup lang="ts">
import Zillionaire from "./templates/Zillionaire.vue";
import BlindBox from "./components/blind-box/BlindBox.vue";
import { ref, watch } from "vue";

// 抽奖：0 或 1（随机）
const featchPrize = async (index: number) => {
  console.log(`目前走了${index}步`);

  localStorage.setItem("dice_number", String(index));

  return Math.floor(Math.random() * 2);
};

// 开奖：0 没中奖 / 1 中将
const showPrize = (prize: any) => {
  if (prize === 0) return console.log("未中奖");
  // blindBoxRef.value?.open(prize);
};

const currentIndex = ref(0);
watch(currentIndex, async (index) => {
  if (!index) return;
  // 抽奖
  const prize = await featchPrize(index);
  // 开奖
  showPrize(prize);
});

const blindBoxRef = ref<InstanceType<typeof BlindBox>>();
</script>

<template>
  <!-- 大富翁 -->
  <Zillionaire v-model="currentIndex" />

  <!-- 盲盒 -->
  <BlindBox ref="blindBoxRef" />
</template>
