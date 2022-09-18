<template>
  <div>
    <Suspense>
      <AsyncViewCity />
      <template #fallback>
        <CityViewSkeleton />
      </template>
    </Suspense>
  </div>
</template>

<script setup>
import AsyncViewCity from "../components/AsyncViewCity.vue";
import CityViewSkeleton from "../components/CityViewSkeleton.vue";
</script>