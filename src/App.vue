<template>
  <div style="display: flex; flex-direction: column; align-items: center; justify-content: center; background-color: #e5e0d8; min-height: 100vh;">
    <h2 style="text-align: center; padding: 20px 0; color: var(--color-heading);">The Little Crochet Shop</h2>

    <div>
      <p v-if="showWelcome" style="color: var(--color-text);">Welcome!</p>
      <input v-model="newOrder.title" placeholder="Order Title" style="padding: 10px; font-size: 16px;" />
      <input v-model="newOrder.size" placeholder="Size" style="padding: 10px; font-size: 16px;" />
      <input v-model="newOrder.date" type="date" style="padding: 10px; font-size: 16px;" />
      <button @click="addOrder" style="background-color:#d0a77b; padding: 10px; font-size: 16px;">Add Order</button>
    </div>

    <table style="width: 100%; border-collapse: collapse; margin-top: 20px; border: 1px solid #ddd;">
      <thead>
        <tr>
          <th style="border: 1px solid #ddd; padding: 12px; text-align: left; background-color: #f2f2f2;">Order</th>
          <th style="border: 1px solid #ddd; padding: 12px; text-align: left; background-color: #f2f2f2;">Size</th>
          <th style="border: 1px solid #ddd; padding: 12px; text-align: left; background-color: #f2f2f2;">Date</th>
          <th style="border: 1px solid #ddd; padding: 12px; text-align: left; background-color: #f2f2f2;">Status</th>
          <th style="border: 1px solid #ddd; padding: 12px; text-align: left; background-color: #f2f2f2;">Actions</th>
        </tr>
      </thead>
      
      <tbody>
        <tr v-for="(order, index) in Orders" :key="index">
          <td style="border: 1px solid #ddd; padding: 12px;">{{ order.title }}</td>
          <td style="border: 1px solid #ddd; padding: 12px;">{{ order.size }}</td>
          <td style="border: 1px solid #ddd; padding: 12px;">{{ order.date }}</td>
          <td style="border: 1px solid #ddd; padding: 12px;">
            <button
              @click="toggleStatus(order, 'Complete')"
              :style="{ backgroundColor: order.status === 'Complete' ? '#D8D7B2' : 'white' }"
            >
              Complete
            </button>
            <button
              @click="toggleStatus(order, 'In Progress')"
              :style="{ backgroundColor: order.status === 'In Progress' ? '#E5ADA8' : 'white' }"
            >
              In Progress
            </button>
          </td>
          <td style="border: 1px solid #ddd; padding: 12px;">
            <button @click="deleteOrder(index)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>

    <p style="text-align: center; padding-top: 140px; font-size: 14px;">Total Orders: {{ Orders.length }}</p>
    <p style="text-align: center; font-size: 14px;">Completed Orders: {{ completedOrders.length }}</p>
    <p style="text-align: center; font-size: 14px;">In Progress Orders: {{ inProgressOrders.length }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newOrder: {
        title: '',
        size: '',
        date: '',
        status: 'In Progress',
      },
      Orders: [
        { title: 'Order 1', size: 'Medium', date: '2023-11-24', status: 'In Progress' },
      ],
      showWelcome: true,
    };
  },
  computed: {
    completedOrders() {
      return this.Orders.filter((order) => order.status === 'Complete');
    },
    inProgressOrders() {
      return this.Orders.filter((order) => order.status === 'In Progress');
    },
  },
  methods: {
    addOrder() {
      this.Orders.push({ title: this.newOrder.title, size: this.newOrder.size, date: this.newOrder.date, status: this.newOrder.status });
      this.newOrder.title = '';
      this.newOrder.size = '';
      this.newOrder.date = '';
      this.newOrder.status = 'In Progress';
    },
    deleteOrder(index) {
      this.Orders.splice(index, 1);
    },
    toggleStatus(order, newStatus) {
      order.status = newStatus;
    },
    clearCompleted() {
      this.Orders = this.Orders.filter((order) => order.status !== 'Complete');
    },
  },
};
</script>
