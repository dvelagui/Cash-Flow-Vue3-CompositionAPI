<template>
  <div>
    <svg
      @touchstart="tap"
      @touchmove="tap"
      @touchend="untap"
      viewBox="0 0 300 200"
    >
      <line
        stroke="#c4c4c4"
        stroke-width="2"
        x1="0"
        :y1="zero"
        x2="300"
        :y2="zero"
      />
      <polyline
        fill="none"
        stroke="#0689b0"
        stroke-width="2"
        :points="points"
      />
      <line
        v-show="showPointer"
        stroke="#04b500"
        stroke-width="2"
        :x1="point"
        y1="0"
        :x2="point"
        y2="200"
      />
    </svg>
    <p>Últimos 30 días</p>
  </div>
</template>

<script setup>
import { toRefs, computed, ref } from "vue";
const props = defineProps({
  amounts: {
    type: Array,
    default: () => [],
  },
});
const { amounts } = toRefs(props);

const amountsToPixels = (amount) => {
  const min = Math.min(...amounts.value);
  const max = Math.max(...amounts.value);
  const amountAbs = amount + Math.abs(min);
  const minMax = Math.abs(max) + Math.abs(min);
  const moveY = 200 - ((amountAbs * 100) / minMax) * 2;
  return moveY;
};

const zero = computed(() => {
  return amountsToPixels(0);
});

const points = computed(() => {
  const total = amounts.value.length;
  return amounts.value.reduce((points, amount, i) => {
    const x = (300 / total) * (i + 1);
    const y = amountsToPixels(amount);
    return `${points} ${x},${y}`;
  }, "0,100");
});

const point = ref(0);

const showPointer = ref(false);

const emit = defineEmits(["slect"]);

const tap = ({ target, touches }) => {
  showPointer.value = true;
  const elementX = target.getBoundingClientRect().x;
  const touchX = touches[0].clientX;
  point.value = touchX - elementX;
  emit("select", amounts);
};

const untap = () => {
  showPointer.value = false;
};
</script>

<style scoped>
svg {
  width: 100%;
}
p {
  text-align: center;
}
</style>
