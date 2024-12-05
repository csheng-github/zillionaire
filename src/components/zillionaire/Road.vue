<script lang="ts">
export type El = HTMLDivElement & { serialId: number; rowIndex: number; colIndex: number };
export interface RoadItem {
  width: string;
  height: string;
  top: string;
  left: string;
  rowIndex: number;
  colIndex: number;
}
export type RoadMap = Array<RoadItem>;
</script>

<script setup lang="ts">
import { ref, shallowRef } from "vue";

interface Props {
  roadMap: RoadMap;
}
const props = defineProps<Props>();

const createPointer = (roadItem: RoadItem, index: number) => {
  const pointer = document.createElement("div") as El;
  pointer.style.width = roadItem.width;
  pointer.style.height = roadItem.height;
  pointer.style.top = roadItem.top;
  pointer.style.left = roadItem.left;
  pointer.style.transform = "translate(-50%, -50%)";
  if (index === 0) pointer.textContent = "开始";
  pointer.classList.add("road-holder");
  return pointer;
};

const generateRoad = (roadMap: RoadMap) => {
  const pointerMap = {} as Record<string, El>;
  roadMap.forEach((roadItem, index) => {
    const pointer = createPointer(roadItem, index);
    pointer.serialId = index;
    pointer.rowIndex = roadItem.rowIndex;
    pointer.colIndex = roadItem.colIndex;
    pointerMap[index] = pointer;
  });
  return pointerMap;
};

const roadBoxRef = ref<HTMLDivElement | null>(null);
const pointerMap = shallowRef<Record<string, El>>({});
const init = () => {
  pointerMap.value = generateRoad(props.roadMap);
  const _roadBoxRef = roadBoxRef.value!;
  Object.values(pointerMap.value).forEach((el) => _roadBoxRef.appendChild(el));
};

const getPointerFromSite = (serialId: number) => pointerMap.value[serialId];
defineExpose({ getPointerFromSite, initRoad: init });
</script>

<template>
  <div ref="roadBoxRef" class="road-box">
    <slot name="pointer"></slot>
    <slot name="sifter"></slot>
  </div>
</template>

<style lang="scss" scoped>
.road-box {
  width: 80vw;
  height: 80vh;
  position: absolute;
  bottom: 5%;
  left: 50%;
  transform: translateX(-50%);

  :deep(.road-holder) {
    position: absolute;
    border: 1px solid;
  }
}
</style>
