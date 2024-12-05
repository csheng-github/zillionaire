<script setup lang="ts">
import { ref, computed } from "vue";
import Zillionaire, { RoadMap } from "../components/zillionaire/Zillionaire.vue";

const { modelValue } = defineModels<{
  modelValue: number;
}>();

const getComputeSite = (rowCount: number, colCount: number, space = 0) => {
  const widthP = (100 - (colCount + 1) * space) / colCount;
  const heightP = (100 - (rowCount + 1) * space) / rowCount;
  const hw = widthP / 2;
  const hh = heightP / 2;
  const hp = space / 2;
  return (rowIndex: number, colIndex: number) => {
    const l = colIndex * (widthP + space) + hw + hp;
    const t = rowIndex * (heightP + space) + hh + hp;
    return { width: `${widthP}%`, height: `${heightP}%`, left: `${l}%`, top: `${t}%`, rowIndex, colIndex };
  };
};
const computeSite = getComputeSite(10, 6, 4);

const roadMap: RoadMap = (
  [
    [0, 5],
    [1, 5],
    [2, 5],
    [3, 5],
    [4, 5],
    [5, 5],
    [6, 5],
    [7, 5],
    [8, 5],
    [9, 5],
    [9, 4],
    [9, 3],
    [9, 2],
    [9, 1],
    [9, 0],
    [8, 0],
    [7, 0],
    [6, 0],
    [5, 0],
    [4, 0],
    [3, 0],
    [2, 0],
    [1, 0],
    [0, 0],
    [0, 1],
    [0, 2],
    [0, 3],
    [0, 4],
  ] as Array<[number, number]>
).map((args) => computeSite(...args));

const pointerIndex = computed({
  get: () => modelValue.value,
  set: (_v) => (modelValue.value = _v),
});

const zillionaireRef = ref<InstanceType<typeof Zillionaire>>();
const zillionaireBoxRef = ref<HTMLDivElement>();
const onClickOrigin = () => {
  zillionaireRef.value!.resetOrigin();
};
</script>

<template>
  <div class="relative w-full overflow-hidden overflow-x-auto" ref="zillionaireBoxRef">
    <Zillionaire ref="zillionaireRef" v-model="pointerIndex" :roadMap="roadMap" :jumpInterval="500" />
    <PrimaryButton class="absolute right-20px top-20px z-999" @click="onClickOrigin">回到原点</PrimaryButton>
  </div>
</template>
