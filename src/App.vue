<script setup lang="ts">
import { ref, reactive, computed } from 'vue'
import ModalView from './components/ModalView.vue'

const showModal = ref(false)

type Product = { id: number; name: string; price: number }
const products = reactive<Product[]>([
  { id: 1, name: 'Produk A', price: 50000 },
  { id: 2, name: 'Produk B', price: 75000 }
])

let lastId = products.length ? Math.max(...products.map(p => p.id)) : 0

function addProduct(payload: { name: string; price: number }) {
  lastId++
  products.push({ id: lastId, name: payload.name, price: payload.price })
  showModal.value = false
}

function deleteProduct(id: number) {
  const idx = products.findIndex(p => p.id === id)
  if (idx !== -1) products.splice(idx, 1)
}

const totalProducts = computed(() => products.length)
const totalValue = computed(() => products.reduce((s, p) => s + p.price, 0))
</script>

<template>
  <div class="page">

    <header class="header">
      <h1 class="title">Product Manager</h1>

      <div class="meta">
        <span>Items: <strong>{{ totalProducts }}</strong></span>
        <span>Total value:  
          <strong>Rp {{ totalValue.toLocaleString() }}</strong>
        </span>
      </div>
    </header>

    <main class="main">
      <div class="action-row">
        <button class="btn add" @click="showModal = true">+ Tambah Produk</button>
      </div>

      <section class="grid">
        <div v-if="products.length === 0" class="empty">Belum ada produk.</div>

        <article v-for="p in products" :key="p.id" class="card">
          <div class="card-left">
            <div class="name">{{ p.name }}</div>
            <div class="price">Rp {{ p.price.toLocaleString() }}</div>
          </div>
          <button class="btn danger" @click="deleteProduct(p.id)">Hapus</button>
        </article>
      </section>
    </main>

    <ModalView :show="showModal" @close="showModal = false" @save="addProduct" />
  </div>
</template>

<style scoped>
/* ----------- Layout ---------- */
.page {
  background: #f7f9fc;
  min-height: 100vh;
  max-width: 860px;
  margin: 32px auto;
  padding: 0 20px;
  font-family: "Inter", sans-serif;
  color: #1f2937;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.title {
  font-size: 28px;
  font-weight: 700;
  color: #1e3a8a;
}

.meta {
  display: flex;
  gap: 14px;
  font-size: 15px;
  color: #4b5563;
}

.meta strong {
  color: #111827;
}

/* ---------- Buttons ---------- */
.btn {
  padding: 9px 16px;
  border-radius: 10px;
  border: none;
  cursor: pointer;
  font-size: 14px;
  transition: 0.2s ease;
}

.btn.add {
  background: #2563eb;
  color: white;
}

.btn.add:hover {
  background: #1d4ed8;
}

.btn.danger {
  background: #ef4444;
  color: white;
}

.btn.danger:hover {
  background: #dc2626;
}

/* ---------- Cards ---------- */
.main .grid {
  margin-top: 14px;
  display: grid;
  gap: 14px;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
}

.card {
  background: #ffffff;
  border: 1px solid #e5e7eb;
  padding: 14px 16px;
  border-radius: 14px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: 0.2s ease;
}

.card:hover {
  box-shadow: 0 4px 12px rgba(0,0,0,0.06);
}

.card-left .name {
  font-weight: 600;
  margin-bottom: 4px;
}

.card-left .price {
  color: #6b7280;
  font-size: 14px;
}

.empty {
  text-align: center;
  color: #6b7280;
  padding: 30px;
  grid-column: 1 / -1;
}
</style>
