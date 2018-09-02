<template>
  <div class="customer-new">
    <form @submit.prevent="add" class="form-horizontal">
       <div class="form-group">
            <label for="name" class="col-md-4 control-label">Name</label>

            <div class="col-md-6">
                <input id="name" type="text" class="form-control" name="name" placeholder="Name" required autofocus  v-model="customer.name">
            </div>
        </div>

        <div class="form-group">
            <label for="email" class="col-md-4 control-label">E-Mail Address</label>

            <div class="col-md-6">
                <input id="email" type="email" class="form-control" name="email" placeholder="Email Address" required autofocus  v-model="customer.email">
            </div>
        </div>

       <div class="form-group">
            <label for="phone" class="col-md-4 control-label">Phone</label>

            <div class="col-md-6">
                <input id="phone" type="text" class="form-control" name="phone" placeholder="Phone" v-model="customer.phone">
            </div>
        </div>

       <div class="form-group">
            <label for="website" class="col-md-4 control-label">Website</label>

            <div class="col-md-6">
                <input id="website" type="text" class="form-control" name="website" placeholder="Website" v-model="customer.website">
            </div>
        </div>
        <div class="form-group">
            <div class="col-md-8 col-md-offset-4">
                <button type="submit" class="btn btn-primary">
                    Create
                </button>
                <button type="button" class="btn btn-danger">
                    Cancel
                </button>
            </div>
        </div>
    </form>

    <div class="errors" v-if="errors">
      <ul>
        <li v-for="(fieldError, fieldName) in errors" :key="fieldName">
          <strong>{{ fieldName }}</strong> {{ fieldError.join('\n') }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>

  import validate from 'validate.js'

  export default {
    name: 'new',
    data() {
      return {
        customer: {
          name: '',
          email: '',
          phone: '',
          websites: ''
        },
        errors: null
      };
    },
    computed: {
      currentUser() {
        return this.$store.getters.currentUser;
      }
    },
    methods: {
      add() {
        this.errors = null;
        const constraints = this.getConstrains();
        const errors = validate(this.$data.customer, constraints);
        if (errors) {
          this.errors = errors;
          return;
        }

        //send the customers data to the BE api
        axios.post('/api/customers/new', this.$data.customer, {
          headers: {
            "Authorization": `Bearer ${this.currentUser.token}`
          }
        })
        .then((response) => {
          this.$router.push('/customers')
        });
      },
      getConstrains() {
        return {
          name: {
            presence: true,
            length: {
              minimum: 3,
              message: 'Must be at least 3 characters long'
            }
          },
          email: {
            presence: true,
            email: true
          },
          phone: {
            presence: true,
            numericality: true,
            length: {
              minimum: 10,
              message: 'Must be at least 10 digits long'
            }
          },
          website: {
            presence: true,
            url: true
          },
        };
      }
    }
  }
</script>

<style scoped>
.errors {
  background: lightcoral;
  border-radius: 5px;
  padding: 21px 0 2px 0;
}
</style>
