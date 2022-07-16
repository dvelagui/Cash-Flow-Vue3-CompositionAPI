<template>
  <Suspense>
    <template #default>
      <HomeCashFlow />
    </template>
    <template #fallback>
      <SplashScreen />
    </template>
  </Suspense>
</template>

<script setup>
import SplashScreen from "@/components/SplashScreen.vue";
import { defineAsyncComponent } from "vue";

const HomeCashFlow = defineAsyncComponent(
  () =>
    new Promise((resolve) => {
      setTimeout(() => {
        resolve(import("./components/HomeCashFlow.vue"));
      }, 2500);
    })
);
</script>

<style>
html,
body,
.app {
  min-height: 100vh;
  margin: 0;
  font-family: Arial, Helvetica, sans-serif;
}
* {
  --brand-green: #04b500;
  --brand-blue: #0689b0;
}
</style>
