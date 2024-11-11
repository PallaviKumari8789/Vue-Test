<template>
    <div>
      <h3>Available Stocks:</h3>
      <table class="styled-table">
        <thead>
          <tr>
            <th>Stock Symbol</th>
            <th>Price</th>
            <th>Available Quantity</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="stock in stocks" :key="stock.symbol">
            <td>{{ stock.symbol }}</td>
            <td>${{ stock.price }}</td>
            <td>{{ stock.quantity }}</td>
            <td>
              <button @click="buy(stock.symbol)">Buy</button>
  
              <button 
                @click="sell(stock.symbol)" 
                :disabled="!canSell(stock.symbol)">
                Sell
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      stocks: {
        type: Array,
        required: true
      },
      transactionHistory: {
        type: Array,
        required: true
      }
    },
  
    methods: {
      canSell(symbol) {
        let ownedQuantity = 0;
  
        this.transactionHistory.forEach(transaction => {
          if (transaction.symbol === symbol) {
            if (transaction.type === 'Buy') {
              ownedQuantity += transaction.quantity;
            } else if (transaction.type === 'Sell') {
              ownedQuantity -= transaction.quantity;
            }
          }
        });
  
        return ownedQuantity > 0;
      },
  
      buy(symbol) {
        const quantity = prompt(`How many ${symbol} stocks would you like to buy?`);
        if (quantity && !isNaN(quantity)) {
          this.$emit('buyStock', symbol, parseInt(quantity));
        }
      },
  
      sell(symbol) {
        const quantity = prompt(`How many ${symbol} stocks would you like to sell?`);
        if (quantity && !isNaN(quantity)) {
          this.$emit('sellStock', symbol, parseInt(quantity));
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .styled-table {
    width: 80%;
    border-collapse: separate;
    border-spacing: 2px;
    border: 5px solid #3a0202;
  }
  .styled-table th, .styled-table td {
    padding: 1px;
    text-align: center;
  }
  button {
    margin: 5px;
    background-color: #28a745;
    color: white;
    border: none;
    padding: 10px;
    cursor: pointer;
  }
  
  button:disabled {
    background-color: #d3d3d3;
    cursor: not-allowed;
  }
  
  button:hover:not(:disabled) {
    background-color: #218838;
  }
  
  button:disabled:hover {
    background-color: #d3d3d3;
  }
  </style>
  