<template>
  <form @submit.prevent="convert">
    <div style="display:flex; gap:12px; margin-bottom:12px; flex-wrap:wrap">
      <input v-model.trim="from" maxlength="3" placeholder="From (EUR)"/>
      <input v-model.trim="to" maxlength="3" placeholder="To (USD)"/>
      <input v-model.number="amount" type="number" min="0" step="1" placeholder="Amount (1)"/>
      <button class="btn" :disabled="loading" type="submit">
        {{ loading ? "Convirtiendo…" : "Convertir" }}
      </button>
    </div>

    <p v-if="error" class="error">{{ error }}</p>

    <div v-if="hasResult" class="badge-result">
      <span>{{ prettyFrom }}</span>
      <span>=</span>
      <span>{{ prettyResult }}</span>
    </div>

    <pre v-if="hasResult">{{ resultJson }}</pre>
  </form>
</template>

<script setup>
import {computed, ref} from "vue";
import {api} from "../services/api";

const from = ref("EUR");
const to = ref("USD");
const amount = ref(1);
const loading = ref(false);
const error = ref("");
const payload = ref(null);

const up = (s) => String(s || "").trim().toUpperCase();

const convert = async () => {
  loading.value = true;
  error.value = "";
  payload.value = null;
  try {
    const {data} = await api.get("/currencies/rate-conversion", {
      params: {from: up(from.value), to: up(to.value), amount: Number(amount.value) || 0},
    });
    payload.value = {
      data: {
        from: String(data?.data?.from ?? up(from.value)),
        to: String(data?.data?.to ?? up(to.value)),
        amount: Number(data?.data?.amount ?? amount.value),
        result: Number(data?.data?.result ?? 0),
      },
    };
  } catch (e) {
    error.value = e.message;
  } finally {
    loading.value = false;
  }
};

const hasResult = computed(() => !!payload.value);
const resultJson = computed(() => (payload.value ? JSON.stringify(payload.value, null, 2) : ""));
const prettyFrom = computed(() => `${Number(payload.value?.data?.amount ?? amount.value)} ${up(from.value)}`);
const prettyResult = computed(() => `${(payload.value?.data?.result ?? 0).toFixed(2)} ${up(to.value)}`);
</script>
