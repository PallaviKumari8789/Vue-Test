<template>
  <div id="app">
    <h1 class="classy-title">Stockify</h1>
    <UserWallet :balance="walletBalance" />
    
    <!-- Stock List with Buy and Sell Actions -->
    <StockList 
      :stocks="stocks" 
      :transactionHistory="transactionHistory"
      @buyStock="buyStock" 
      @sellStock="sellStock" 
    />
    
    <!-- Transaction History -->
    <TransactionHistory :transactions="transactionHistory" />
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import UserWallet from './components/UserWallet.vue';
import StockList from './components/StockList.vue';
import TransactionHistory from './components/TransactionHistory.vue';

export default {
  components: {
    UserWallet,
    StockList,
    TransactionHistory
  },
  
  setup() {
    const walletBalance = ref(1000);  // Starting wallet balance
    const stocks = ref([]);           // List of stocks
    const transactionHistory = ref([]);  // Transaction history

    // Fetch stocks data from an API (or mock data for now)
    onMounted(async () => {
      try {
        const response = await axios.get('http://localhost:3000/stock');
        stocks.value = response.data;
      } catch (error) {
        console.error('Error fetching stock data:', error);
      }
    });

    // Function to handle buying stocks
    const buyStock = (symbol, quantity) => {
      const stock = stocks.value.find(s => s.symbol === symbol);
      const totalCost = stock.price * quantity;

      // Check if the user has enough funds and stock is available
      if (walletBalance.value >= totalCost && stock.quantity >= quantity) {
        walletBalance.value -= totalCost;
        stock.quantity -= quantity;

        transactionHistory.value.push({
          type: 'Buy',
          symbol,
          quantity,
          amount: totalCost,
          date: new Date().toLocaleString()
        });
      } else {
        alert('Insufficient funds or stock quantity!');
      }
    };

    // Function to handle selling stocks
    const sellStock = (symbol, quantity) => {
      const stock = stocks.value.find(s => s.symbol === symbol);
      const totalValue = stock.price * quantity;
      const brokerageFee = totalValue * 0.01;

      walletBalance.value += totalValue - brokerageFee;
      stock.quantity += quantity;

      transactionHistory.value.push({
        type: 'Sell',
        symbol,
        quantity,
        amount: totalValue - brokerageFee,
        brokerageFee,
        date: new Date().toLocaleString()
      });
    };

    return {
      walletBalance,
      stocks,
      transactionHistory,
      buyStock,
      sellStock
    };
  }
};
</script>

<style scoped>
#app {
  position: relative;
  width: 100%;
  height: 100vh;
}

#app::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-image: url('@/assets/istockphoto2.jpg');
  background-repeat: repeat;
  background-position: center center;
  filter: blur(1px);
  z-index: -1;
  background-size: auto;
  z-index: -1;
  background-position: top left;
}

.classy-title {
  font-family: 'Georgia', serif; 
  font-weight: bold;
  text-align: center;
  font-size: 50px; 
  color: #fff; 
  text-transform: uppercase;
  letter-spacing: 4px;  /* Letter spacing for a more refined look */
  background: linear-gradient(to right, #6a5acd, #00bfff);  /* Beautiful gradient */
  -webkit-background-clip: text;  /* Apply gradient to text */
  color: transparent;  /* Make the text color transparent */
  text-shadow: 4px 4px 15px rgba(0, 0, 0, 0.5);  /* Add text shadow for depth */
  padding: 10px 0;
  transition: all 0.3s ease-in-out;
}
</style>
