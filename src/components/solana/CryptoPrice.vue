<template>
  <VaCard>
    <VaCardContent>
      <h3>Solana Price</h3>
      <p>{{ price ? `$${price}` : 'Loading...' }}</p>
    </VaCardContent>
  </VaCard>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const price = ref(null)
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
      price.value = data.solana.usd
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
