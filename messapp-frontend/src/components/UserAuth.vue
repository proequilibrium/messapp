<template>
  <div class="container">
    <h1 class="text-center">MessAPP</h1>
    <div id="auth-container" class="row">
      <div class="col-sm-4 offset-sm-4">
        <ul class="nav nav-tabs nav-justified" id="myTab" role="tablist">
          <li class="nav-item">
            <a class="nav-link active" id="signup-tab" data-toggle="tab" href="#signup" role="tab" aria-controls="signup" aria-selected="false">Registrace</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" id="signin-tab" data-toggle="tab" href="#signin" role="tab" aria-controls="signin" aria-selected="true">Prihlaseni</a>
          </li>
        </ul>

        <div class="tab-content" id="myTabContent">

          <div class="tab-pane fade show active" id="signup" role="tabpanel" aria-labelledby="signin-tab">
            <form @submit.prevent="signUp">
              <div class="form-group">
                <input v-model="email" type="email" class="form-control" id="email" placeholder="Emailova adresa" required>
              </div>
              <div class="form-row">
                <div class="form-group col-md-6">
                  <input v-model="username" type="text" class="form-control" id="username" placeholder="prihlasovaci jmeno" required>
                </div>
                <div class="form-group col-md-6">
                  <input v-model="password" type="password" class="form-control" id="password" placeholder="heslo" required>
                </div>
              </div>
              <div class="form-group">
                <div class="form-check">
                  <input class="form-check-input" type="checkbox" id="toc" required>
                  <label class="form-check-label" for="gridCheck">
                    Accept terms and Conditions
                  </label>
                </div>
              </div>
              <button type="submit" class="btn btn-block btn-primary">Sign up</button>
            </form>
          </div>

          <div class="tab-pane fade" id="signin" role="tabpanel" aria-labelledby="signin-tab">
            <form @submit.prevent="signIn">
              <div class="form-group">
                <input v-model="username" type="text" class="form-control" id="username" placeholder="jmeno" required>
              </div>
              <div class="form-group">
                <input v-model="password" type="password" class="form-control" id="password" placeholder="heslo" required>
              </div>
              <button type="submit" class="btn btn-block btn-primary">Sign in</button>
            </form>
          </div>

        </div>
      </div>
    </div>
  </div>
</template>

<script>
const $ = window.jQuery // JQuery

export default {

  data () {
    return {
      email: '', username: '', password: ''
    }
  },

  methods: {
    signUp () {
      $.post('http://192.168.1.76:8008/auth/users/create/', this.$data, (data) => {
        alert('Your account has been created. You will be signed in automatically')
        this.signIn()
      })
        .fail((response) => {
          alert(response.responseText)
        })
    },

    signIn () {
      const credentials = {username: this.username, password: this.password}
      $.post('http://192.168.1.76:8008/auth/token/create/', credentials, (data) => {
        sessionStorage.setItem('authToken', data.auth_token)
        sessionStorage.setItem('username', this.username)
        this.$router.push('/chats')
        alert(data.auth_token)
      })
        .fail((response) => {
          alert(response.responseText)
        })
    }
  }

}

</script>

<style scoped>
  #auth-container {
    margin-top: 50px;
  }

  .tab-content {
    padding-top: 20px;
  }
</style>
