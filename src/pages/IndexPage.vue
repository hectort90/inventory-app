<template>
  <q-page padding>
    <div v-for="(item, index) in inventoryItems" :key="index" class="q-pa-md">
      <q-card>
        <q-card-section>
          <div>
            <div class="text-h6">{{ item.code }} - {{ item.description }}</div>
            <div class="text-caption q-mt-xs"><strong>Familia:</strong> {{ item.family }}</div>
            <div class="text-caption q-mt-xs"><strong>Ubicación:</strong> {{ item.location }}</div>
          </div>
        </q-card-section>
        <q-separator />
        <q-card-actions align="right">
          <q-btn flat icon="remove" @click="decrement(item)" />
          <q-input v-model="item.quantity" type="number" dense style="width: 50px;" />
          <q-btn flat icon="add" @click="increment(item)" />
          <q-btn icon="save" color="green" @click="saveItem(item)" />
        </q-card-actions>
      </q-card>
    </div>
    <div v-if="inventoryItems.length === 0">
      No hay inventario
    </div>
  </q-page>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const inventoryItems = ref([]);
onMounted(async () => {
  try {
    console.log('Fetching inventory data...');
    const options = {
      headers: {
        'content-type': 'application/json',
      }
    };

    const response = await axios.get('http://10.0.2.2:5000/api/Products');
    console.log('Inventory data fetched:', response.data);
    inventoryItems.value = response.data;
  } catch (error) {
    console.error('Error fetching inventory data:', error);
  }
});

function increment(item) {
  item.quantity++;
}

function decrement(item) {
  if (item.quantity > 0) {
    item.quantity--;
  }
}

function saveItem(item) {
  console.log(item);
}

function goBack() {
  // Implementa la funcionalidad para volver a la página anterior
}
</script>

<style scoped>
.q-card-section {
  display: flex;
  flex-direction: column;
}
.q-card-actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
