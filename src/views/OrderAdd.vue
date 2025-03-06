<template>
  <div>
    <h2>Add new order</h2>

    <form @submit.prevent="add">
      <input type="date" v-model="order.date" required />
      <input type="text" v-model="order.sender" placeholder="Sender" required />
      <input type="text" v-model="order.destination" placeholder="Destination" required />
      <input type="number" v-model="order.weight" placeholder="Weight in kg" required />

      <select v-model="order.status" required>
        <option value="Confirmé">Confirmé</option>
        <option value="En livraison">En livraison</option>
        <option value="Livré">Livré</option>
      </select>

      <p v-if="errorMessage" style="color: red">{{ errorMessage }}</p>

      <button type="submit">Save</button>
      <router-link to="/"><button type="button">Cancel</button></router-link>
    </form>
  </div>
</template>

<script>
import { nanoid } from "nanoid"

export default {
  data() {
    return {
      order: {
        date: "",
        sender: "",
        destination: "",
        weight: "",
        status: "Confirmé"
      },
      errorMessage: ""
    }
  },
  methods: {
    add() {
      if (!this.order.date || !this.order.sender || !this.order.destination || !this.order.weight) {
        this.errorMessage = "All fields must be filled"
        return
      }
      if (this.order.weight <= 0) {
        this.errorMessage = "Weight must be greater than 0"
        return
      }

      this.order.id = nanoid() // Générer un ID unique

      let orders = JSON.parse(localStorage.getItem("orders")) || []
      orders.push(this.order)
      localStorage.setItem("orders", JSON.stringify(orders))

      this.$router.push("/")
    }
  }
}
</script>
