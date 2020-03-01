<style lang="less">
  @import './login.less';
</style>

<template>
  <div class="login">
    <div class="login-con">
      <Card icon="log-in" title="欢迎登录" :bordered="false">
        <div class="form-con">
          <login-form @on-success-valid="handleSubmit"></login-form>
          <p class="login-tip">
            <Alert type="error"
                   v-show="flag"
                   show-icon>
              {{message}}
            </Alert>
          </p>
        </div>
      </Card>
    </div>
  </div>
</template>

<script src="https://cdn.staticfile.org/axios/0.18.0/axios.min.js"></script>
<script>
import LoginForm from '_c/login-form'
import { mapActions } from 'vuex'
import axios from 'axios'
import Cookies from 'js-cookie'
var isLogin = false;
var loginMessage = '';
var code = -1;
export default {
  components: {
    LoginForm
  },
  data () {
    return {
      flag: false,
      message: ''
    }
  },
  methods: {
    ...mapActions([
      'handleLogin',
      'getUserInfo'
    ]),
    async handleSubmit ({ userName, password }) {
      var params = new URLSearchParams();
      params.append('userName', userName);
      params.append('password', password);
      await axios.post(
        'http://localhost:8081/index.php/index/index/doLogin/', params
        )
        .then(function (response) {
          code = response.data.code;
          if(code != 200){
            isLogin = true;
          }
          loginMessage = response.data.message;
          console.log(response.data);
        })
        .catch(function (error) {
          console.log(error);
          });
          if(isLogin){
            this.message = loginMessage;
            this.flag = true;
          }
          if(code == 200){
            Cookies.set('userName', userName);
            userName = 'super_admin';
            this.handleLogin({ userName, password }).then(res => {
              this.getUserInfo().then(res => {
                this.$router.push({
                  name: this.$config.homeName
                })
              })
            })
          }
    }
  }
}
</script>

<style>

</style>
