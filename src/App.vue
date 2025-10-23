<template>
  <div>
    <header>
      <div class="logo-area">
        <img src="./assets/logo.png" alt="Hoy-voy"/>
      </div>

      <nav>
        <a href="https://fixer.io/">¿Dónde conseguimos los datos?</a>
      </nav>
    </header>

    <main class="container">
      <h1> Currencies Playground</h1>
      <p class="subtitle">API: {{ baseUrl }}</p>

      <div class="grid">
        <section class="card">
          <h2>Listado de divisas</h2>
          <CurrenciesTable/>
        </section>

        <section class="card">
          <h2>Conversión</h2>
          <RateConversionForm/>
        </section>
      </div>
    </main>
  </div>
</template>

<script setup>
import {computed} from "vue";
import CurrenciesTable from "./components/CurrenciesTable.vue";
import RateConversionForm from "./components/RateConversionForm.vue";

const baseUrl = computed(() => import.meta.env.VITE_API_BASE_URL || "http://localhost/api");
</script>
