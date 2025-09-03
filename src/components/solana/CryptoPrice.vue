<template>
  <VaCard>
    <VaCardContent>
      <h3>Solana Price</h3>
      <p>
        {{ price ? `$${price}` : error ? error : 'Loading...' }}
        <VaIcon
          v-if="trend"
          :name="trend === 'up' ? 'va-arrow-up' : 'va-arrow-down'"
          :color="trend === 'up' ? 'success' : 'danger'"
          size="small"
        />
      </p>
    </VaCardContent>
  </VaCard>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const price = ref(null)
const prevPrice = ref(null)
const trend = ref(null) // 'up', 'down', or null (no trend yet)
const error = ref(null)
let intervalId = null

const fetchPrice = async () => {
  try {
    const response = await fetch('https://api.coingecko.com/api/v3/simple/price?ids=solana&vs_currencies=usd')
    if (!response.ok) {
      throw new Error(`HTTP error! Status: ${response.status}`)
    }
    const data = await response.json()
    console.log('API Response:', data) // Debug (remove in production)
    if (data?.solana?.usd) {
      const newPrice = data.solana.usd
      if (price.value !== null) {
        // Compare with previous price
        trend.value = newPrice > price.value ? 'up' : newPrice < price.value ? 'down' : null
        prevPrice.value = price.value // Store current price as previous
      }
      price.value = newPrice // Update current price
      error.value = null
    } else {
      error.value = 'No price data available'
    }
  } catch (err) {
    console.error('Fetch Error:', err.message)
    error.value = 'Error fetching Solana price'
  }
}

onMounted(() => {
  fetchPrice() // Initial fetch
  intervalId = setInterval(fetchPrice, 60000) // Update every 60 seconds
})

onUnmounted(() => {
  if (intervalId) clearInterval(intervalId) // Clean up interval
})
</script>

<style scoped>
.price-container {
  display: flex;
  align-items: center;
  gap: 8px; /* Space between price and arrow */
}
</style>
