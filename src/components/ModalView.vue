<template>
  <div v-if="show" class="modal-overlay" @click.self="closeModal">
    <div class="modal-card" role="dialog" aria-modal="true" aria-labelledby="modal-title">
      <h2 id="modal-title" class="modal-title">Tambah Produk</h2>

      <div class="modal-content">
        <div class="form-group">
          <label for="name">Nama Produk</label>
          <input
            id="name"
            type="text"
            v-model="product.name"
            placeholder="Masukkan nama produk"
          />
        </div>

        <div class="form-group">
          <label for="price">Harga (Rp)</label>
          <input
            id="price"
            type="number"
            v-model.number="product.price"
            placeholder="Masukkan harga"
            min="0"
          />
        </div>
      </div>

      <div class="modal-actions">
        <button class="btn cancel" @click="closeModal">Batal</button>
        <button class="btn save" @click="saveProduct" :disabled="!canSave">Simpan</button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive, computed } from 'vue'

defineProps<{ show: boolean }>()
const emit = defineEmits(["close", "save"])

const product = reactive<{ name: string; price: number | null }>({
  name: "",
  price: null
})

const canSave = computed(() => {
  return product.name.trim().length > 0 && product.price !== null && product.price >= 0
})

function closeModal() {
  product.name = ""
  product.price = null
  emit("close")
}

function saveProduct() {
  if (!canSave.value) return
  emit("save", { name: product.name.trim(), price: product.price ?? 0 })
  product.name = ""
  product.price = null
}
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.35);
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}

.modal-card {
  background: #ffffff;
  width: 380px;
  border-radius: 16px;
  padding: 20px;
  box-shadow: 0 14px 36px rgba(0,0,0,0.15);
}

.modal-title {
  font-size: 20px;
  font-weight: 600;
  margin-bottom: 16px;
  text-align: center;
  color: #1e3a8a;
}

.form-group {
  margin-bottom: 16px;
  display: flex;
  flex-direction: column;
}

label {
  font-size: 14px;
  color: #374151;
  margin-bottom: 6px;
}

input {
  padding: 10px 12px;
  border-radius: 8px;
  border: 1px solid #d1d5db;
  font-size: 14px;
}

input:focus {
  border-color: #2563eb;
  box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.18);
  outline: none;
}

.modal-actions {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
  margin-top: 10px;
}

.btn {
  padding: 8px 16px;
  border-radius: 8px;
  border: none;
  cursor: pointer;
  font-size: 14px;
  transition: 0.2s ease;
}

.cancel {
  background: #e5e7eb;
  color: #111827;
}

.cancel:hover {
  background: #d1d5db;
}

.save {
  background: #2563eb;
  color: white;
}

.save:hover {
  background: #1d4ed8;
}

.save:disabled {
  background: #93c5fd;
}
</style>
