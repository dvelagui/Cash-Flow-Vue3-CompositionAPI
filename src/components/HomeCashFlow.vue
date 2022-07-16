<template>
  <LayoutCashFlow>
    <template #header>
      <HeaderCashFlow />
    </template>
    <template #resume>
      <Resume :label="selectDate" :amount="amount" :amount-total="totalAmount">
        <template #graphic>
          <GraphicCash :amounts="amounts" />
        </template>
        <template #action>
          <ActionCashFlow @create="create" />
        </template>
      </Resume>
    </template>
    <template #movements>
      <IndexMovements :movements="movements" @remove="remove" />
    </template>
  </LayoutCashFlow>
</template>

<script setup>
import { computed } from "vue";
import LayoutCashFlow from "@/components/LayoutCashFlow.vue";
import HeaderCashFlow from "@/components/HeaderCashFlow.vue";
import Resume from "@/components/Resume/IndexCashFlow.vue";
import IndexMovements from "@/components/Movements/IndexMovements.vue";
import ActionCashFlow from "./ActionCashFlow.vue";
import GraphicCash from "./Resume/GraphicCash.vue";

const movements = [
  {
    id: 0,
    title: "movimiento 1",
    description: "lorem ipsum lo que sea del 1",
    amount: 100,
    time: new Date("06-21-2022"),
  },
  {
    id: 1,
    title: "movimiento 2",
    description: "lorem ipsum lo que sea",
    amount: 200,
    time: new Date("06-21-2022"),
  },
  {
    id: 2,
    title: "movimiento 3",
    description: "lorem ipsum lo que sea del 3",
    amount: 600,
    time: new Date("06-21-2022"),
  },
  {
    id: 4,
    title: "movimiento 5",
    description: "lorem ipsum lo que sea",
    amount: -600,
    time: new Date("06-21-2022"),
  },
  {
    id: 5,
    title: "movimiento 6",
    description: "lorem ipsum lo que sea",
    amount: -500,
    time: new Date("06-21-2022"),
  },
  {
    id: 6,
    title: "movimiento 7",
    description: "lorem ipsum lo que sea",
    amount: 0,
    time: new Date("06-21-2022"),
  },
  {
    id: 7,
    title: "movimiento 8",
    description: "lorem ipsum lo que sea",
    amount: 500,
    time: new Date("06-20-2022"),
  },
  {
    id: 8,
    title: "movimiento 9",
    description: "lorem ipsum lo que sea",
    amount: 200,
    time: new Date("05-20-2022"),
  },
  {
    id: 9,
    title: "movimiento 10",
    description: "lorem ipsum lo que sea",
    amount: -100,
    time: new Date("05-20-2022"),
  },
  {
    id: 67,
    title: "movimiento 11",
    description: "lorema",
    amount: -200,
    time: new Date(),
  },
];

const totalAmount = computed(() => {
  return movements.reduce((suma, move) => {
    return suma + move.amount;
  }, 0)
})

const amounts = computed(() => {
  const lastDays = movements
    .filter((m) => {
      const today = new Date();
      const oldDate = today.setDate(today.getDate() - 30);
      return m.time > oldDate;
    })
    .map((m) => m.amount);

  return lastDays.map((m, i) => {
    const lastMovements = lastDays.slice(0, i + 1);
    return lastMovements.reduce((suma, movement) => {
      return suma + movement;
    }, 0);
  });
});

const create = (movement) => {
  console.log(movement);
  movements.push(movement);
  console.log(movements);
};

const remove = (id) => {
  const index = movements.findIndex(m => m.id === id);
  movements.splice(index, 1);
};

const amount = null;
const selectDate = new Date().toLocaleDateString();
</script>
