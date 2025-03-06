<template>
  <div class="content">
    <router-link to="add"><button>Add new order</button></router-link>

    <input type="text" v-model="searchQuery" placeholder="Rechercher..." />

    <select v-model="selectedStatus">
      <option value="">Tous les statuts</option>
      <option value="Confirmé">Confirmé</option>
      <option value="En livraison">En livraison</option>
      <option value="Livré">Livré</option>
    </select>

    <table>
      <thead>
        <tr>
          <th @click="sortBy('date')">Date</th>
          <th @click="sortBy('sender')">Expéditeur</th>
          <th @click="sortBy('destination')">Destination</th>
          <th @click="sortBy('weight')">Poids</th>
          <th @click="sortBy('status')">Statut</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(order, index) in filteredOrders" :key="order.id">
          <td>{{ order.date }}</td>
          <td>{{ order.sender }}</td>
          <td>{{ order.destination }}</td>
          <td>{{ order.weight }} kg</td>
          <td>{{ order.status }}</td>
          <td>
            <button>Next status</button>
            <button @click="deleteOrder(index)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchQuery: "",
      selectedStatus: "",
      sortColumn: "",
      sortDirection: 1,
      orders: [] // Initialisé vide, chargé depuis localStorage
    }
  },
  computed: {
    filteredOrders() {
      return this.orders
        .filter(
          (order) =>
            order.sender.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
            order.destination.toLowerCase().includes(this.searchQuery.toLowerCase())
        )
        .filter((order) => !this.selectedStatus || order.status === this.selectedStatus)
        .sort((a, b) => {
          if (!this.sortColumn) return 0
          return (a[this.sortColumn] > b[this.sortColumn] ? 1 : -1) * this.sortDirection
        })
    }
  },
  mounted() {
    this.loadOrders() // Charge les données au démarrage
  },
  methods: {
    loadOrders() {
      const storedOrders = localStorage.getItem("orders")
      if (storedOrders) {
        this.orders = JSON.parse(storedOrders)
      } else {
        this.orders = [
          {
            id: 1,
            date: "2023/03/05",
            sender: "Swayyn-Streich",
            destination: "Rohan Group",
            weight: 1,
            status: "En livraison"
          },
          {
            id: 2,
            date: "2023/03/05",
            sender: "Durgan-Koch",
            destination: "Rosenbaum, Parker and Wilms",
            weight: 4,
            status: "Confirmé"
          },
          {
            id: 3,
            date: "2023/03/06",
            sender: "Conroy-Kovacek",
            destination: "Johnston Group",
            weight: 10,
            status: "Confirmé"
          },
          {
            id: 4,
            date: "2023/03/09",
            sender: "Hamilton Inc",
            destination: "Raucker LLC",
            weight: 1,
            status: "Confirmé"
          },
          {
            id: 5,
            date: "2023/03/11",
            sender: "Von Satterfield",
            destination: "Purdy, Reichert and Ritchie",
            weight: 1,
            status: "En livraison"
          },
          {
            id: 6,
            date: "2023/03/11",
            sender: "Lowe and Sons",
            destination: "Goodwin and Sons",
            weight: 2,
            status: "En livraison"
          },
          {
            id: 7,
            date: "2023/03/11",
            sender: "Christiansen, Treutel and Kiln",
            destination: "Barrows, Glover et Dibbert",
            weight: 1,
            status: "Confirmé"
          },
          {
            id: 8,
            date: "2023/03/12",
            sender: "Gerlach-Sanford",
            destination: "Cole-Armstrong",
            weight: 8,
            status: "En livraison"
          },
          {
            id: 9,
            date: "2023/03/22",
            sender: "Ulrich-Baehnlein",
            destination: "Okuneva et Sons",
            weight: 1,
            status: "Confirmé"
          },
          {
            id: 10,
            date: "2023/03/30",
            sender: "Mills, Sporer et Kohler",
            destination: "Connelly-Stolber",
            weight: 4,
            status: "Confirmé"
          }
        ]
        this.saveOrders() // Sauvegarde initiale des données
      }
    },
    saveOrders() {
      localStorage.setItem("orders", JSON.stringify(this.orders))
    },
    deleteOrder(index) {
      this.orders.splice(index, 1)
      this.saveOrders() // Met à jour localStorage après suppression
    },
    sortBy(column) {
      if (this.sortColumn === column) {
        this.sortDirection *= -1
      } else {
        this.sortColumn = column
        this.sortDirection = 1
      }
    }
  }
}
</script>

<style scoped>
.content {
  padding: 20px;
}
table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}
th,
td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}
th {
  background-color: #f4f4f4;
  cursor: pointer;
}
input,
select {
  margin: 10px 0;
  padding: 5px;
}
button {
  margin: 2px;
  padding: 5px;
  cursor: pointer;
}
</style>
