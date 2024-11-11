<template>
  <div>
    <h3>Transaction History</h3>
    <table>
      <thead>
        <tr>
          <th>Date</th>
          <th>Type</th>
          <th>Symbol</th>
          <th>Quantity</th>
          <th>Amount</th>
          <th>Brokerage Fee</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="transaction in transactions" :key="transaction.date">
          <td>{{ transaction.date }}</td>
          <td>{{ transaction.type }}</td>
          <td>{{ transaction.symbol }}</td>
          <td>{{ transaction.quantity }}</td>
          <td>${{ transaction.amount }}</td>
          <td v-if="transaction.brokerageFee">${{ transaction.brokerageFee }}</td>
          <td v-else>-</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  props: {
    transactions: {
      type: Array,
      required: true
    }
  }
};
</script>

<style scoped>
table {
  width: 100%;
  border-collapse: collapse;
}
th, td {
  padding: 10px;
  text-align: center;
  border: 1px solid #ddd;
}
</style>
