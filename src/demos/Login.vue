<template>
<div id="login" class="container">
  <div class="row-fluid">
    <div class="span12">
      <div class="login well well-small">
        <div class="center">
          <img src="../assets/favicon.png" alt="logo">
        </div>
        <form action="/users/login" style="" class="login-form" id="UserLoginForm" method="post" accept-charset="utf-8">
          <div class="control-group">
            <div class="input-prepend">
              <span class="add-on"><i class="icon-user"></i></span>
              <input name="data[User][username]" required="required" placeholder="Username" maxlength="255" type="text" id="UserUsername">
            </div>
          </div>
          <div class="control-group">
            <div class="input-prepend">
              <span class="add-on"><i class="icon-lock"></i></span>
              <input name="data[User][password]" required="required" placeholder="Password" type="password" id="UserPassword">
            </div>
          </div>
          <div class="control-group">
            <label id="remember-me">
              <input type="checkbox" name="data[User][remember_me]" value="1" id="UserRememberMe"> Remember Me?</label>
          </div>
          <div class="control-group">
            <input class="btn btn-primary btn-large btn-block" type="submit" value="Sign in" @click="connect()">
          </div>
        </form>
      </div><!--/.login-->
    </div><!--/.span12-->
  </div><!--/.row-fluid-->
</div><!--/.container-->
</template>

<script>
import { Confirm, Group, XSwitch, XButton, TransferDomDirective as TransferDom } from 'vux'
export default {
  directives: {
    TransferDom
  },
  components: {
    Confirm,
    Group,
    XSwitch,
    XButton
  },
  data () {
    return {
      callee: ''
    }
  },
  methods: {
    connect () {
      this.$store.dispatch('connect', {
        username: 'oztemur@genband.com',
        password: 'Genband.1234'
      })
    }
  }
}
</script>

<style>
#login img{
  margin: 10px 0;
}
#login .center {
  text-align: center;
}

#login .login {
  max-width: 300px;
	margin: 35px auto;
}

#login .login-form{
  padding:0px 25px;
}
</style>
