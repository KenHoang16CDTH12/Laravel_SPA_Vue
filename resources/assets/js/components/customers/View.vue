<template>
  <div class="customer-view" v-if="customer">
    <div class="user-img">
      <img src="https://cdn.dribbble.com/users/1126660/screenshots/2667470/avatar_1x.png" alt="">
    </div>
    <div class="user-infor">
      <table class="table">
        <tr>
          <th>ID</th>
          <td>{{ customer.id }}</td>
        </tr>
        <tr>
          <th>Name</th>
          <td>{{ customer.name }}</td>
        </tr>
        <tr>
          <th>Email</th>
          <td>{{ customer.email }}</td>
        </tr>
        <tr>
          <th>Phone</th>
          <td>{{ customer.phone }}</td>
        </tr>
        <tr>
          <th>Website</th>
          <td>{{ customer.website }}</td>
        </tr>
      </table>
      <router-link to="/customers">Back to all customers</router-link>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'view',
    data() {
      return {
        customer: null
      }
    },
    created() {
      if (this.customers.length) {
        this.customer = this.customers.find((customer) => customer.id == this.$route.params.id);
      } else {
        axios.get(`/api/customers/${this.$route.params.id}`, {
          headers: {
            "Authorization": `Bearer ${this.currentUser.token}`
          }
        })
        .then((response) => {
          this.customer = response.data.customer
        });
      }
    },
    computed: {
      currentUser() {
        return this.$store.getters.currentUser;
      },
      customers() {
        return this.$store.getters.currentUser;
      }
    }
  }
</script>

<style scoped>
.customer-view {
  display: flex;
  align-items: center;
}
.user-img {
  flex: 1;
}
.user-img img {
  max-width: 160px;
}
.user-infor {
  flex: 3;
  overflow-x: scroll;
}
</style>
