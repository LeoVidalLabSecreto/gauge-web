<script setup lang="ts">
import TheWelcome from './components/TheWelcome.vue'
import CircularProgress from './components/circularProgress/circularProgress.vue'
import DateProgress from './components/dateProgress/dateProgress.vue'
</script>

<template>
  <header>
    <gauge>
      <DateProgress
        :startProgress="new Date('2024-11-01')"
        :progress="new Date('2024-11-20')"
        :endProgress="new Date('2024-11-30')"
      />
      <CircularProgress startProgress="0" progress="9" endProgress="10" />
    </gauge>
  </header>

  <main>
    <TheWelcome />
  </main>
</template>

<style scoped>
header {
  line-height: 0.5;
}
gauge {
  width: 270px;
  height: 250px;
  background-color: white;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
