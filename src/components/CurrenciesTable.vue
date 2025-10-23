<template>
  <div>
    <div style="display:flex; gap:12px; margin-bottom:12px">
      <input v-model.trim="q" placeholder="Filtrar por código o nombre…"/>
      <button class="btn" @click="fetchCurrencies" :disabled="loading">
        {{ loading ? "Cargando…" : "Recargar" }}
      </button>
    </div>

    <p v-if="error" class="error">{{ error }}</p>

    <div class="table-wrap">
      <table>
        <thead>
        <tr>
          <th style="width:110px">Código</th>
          <th>Nombre</th>
          <th style="width:140px">rate_USD</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="r in paged" :key="r.code">
          <td><strong>{{ r.code }}</strong></td>
          <td>{{ r.name }}</td>
          <td>{{ r.rate_USD }}</td>
        </tr>
        <tr v-if="!loading && paged.length === 0">
          <td colspan="3" style="opacity:.7">Sin resultados</td>
        </tr>
        </tbody>
      </table>
    </div>

    <div class="pager" v-if="pages > 1">
      <button class="btn" :disabled="page===1" @click="page--">‹</button>
      <span>Página {{ page }} / {{ pages }}</span>
      <button class="btn" :disabled="page===pages" @click="page++">›</button>
    </div>
  </div>
</template>

<script setup>
import {computed, onMounted, ref, watch} from "vue";
import {api} from "../services/api";

const loading = ref(false);
const error = ref("");
const q = ref("");
const rows = ref([]);
const page = ref(1);
const pageSize = 5;

const fetchCurrencies = async () => {
  loading.value = true;
  error.value = "";
  try {
    const {data} = await api.get("/currencies");
    rows.value = Array.isArray(data?.data) ? data.data : [];
    page.value = 1;
  } catch (e) {
    error.value = e.message;
  } finally {
    loading.value = false;
  }
};

const filtered = computed(() => {
  const k = q.value.toLowerCase();
  if (!k) return rows.value;
  return rows.value.filter(
      (r) => r.code.toLowerCase().includes(k) || r.name.toLowerCase().includes(k)
  );
});
const pages = computed(() => Math.max(1, Math.ceil(filtered.value.length / pageSize)));
const paged = computed(() => {
  const start = (page.value - 1) * pageSize;
  return filtered.value.slice(start, start + pageSize);
});
watch(filtered, () => {
  if (page.value > pages.value) page.value = pages.value;
});

onMounted(fetchCurrencies);
</script>
