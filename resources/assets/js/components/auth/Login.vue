<template>
<div class="container">
    <div class="row">
        <div class="col-md-8 col-md-offset-2">
            <div class="panel panel-default">
                <div class="panel-heading">Login</div>
                <div class="panel-body">
                    <form @submit.prevent="authenticate" class="form-horizontal">
                        <div class="form-group">
                            <label for="email" class="col-md-4 control-label">E-Mail Address</label>

                            <div class="col-md-6">
                                <input id="email" type="email" class="form-control" name="email" placeholder="Email Address" required autofocus  v-model="form.email">
                            </div>
                        </div>

                        <div class="form-group">
                            <label for="password" class="col-md-4 control-label">Password</label>

                            <div class="col-md-6">
                                <input id="password" type="password" class="form-control" name="password" required placeholder="Password" v-model="form.password">
                            </div>
                        </div>
                        <div class="form-group" v-if="authError">
                          <p class="error">
                            {{ authError }}
                          </p>
                        </div>
                        <div class="form-group">
                            <div class="col-md-8 col-md-offset-4">
                                <button type="submit" class="btn btn-primary">
                                    Login
                                </button>
                            </div>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>
</template>

<script>
import {login} from '../../helpers/auth';

export default {
  name: "login",
  data() {
    return {
      form: {
        email: '',
        password: ''
      },
      error: null
    };
  },
  methods: {
    authenticate() {
      this.$store.dispatch('login');

      login(this.$data.form)
          .then((res) => {
            this.$store.commit("loginSuccess", res);
            this.$router.push({path: '/'});
          })
          .catch((error) => {
            this.$store.commit("loginFailed", {error});
          });
    }
  },
  computed: {
    authError() {
      return this.$store.getters.authError;
    }
  }
}
</script>

<style scoped>
.error {
  text-align: center;
  color: red;
}
</style>
