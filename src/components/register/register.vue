<template>
  <div id="main-body">
    <img src="http://p70pqu6ys.bkt.clouddn.com/%E6%8F%92%E7%94%BB.png" alt="星空网红助手" class="shpc login-pic">
    <div class="login-box flex" id="login-box">
      <div class="login-box-title-new flex">
        <div class="login-title-new">注册</div>
        <!-- <div class="min-font flex">注册帐号开始下单</div> -->
      </div>
      <!--       <div class="input-box-new flex">
        <div class="ibn-title">用户帐号：</div>
        <div class="ibn-warp flex">
          <input type="text" placeholder="用户名" class="i-ipnput" v-model="phone" autocorrect="off" autocapitalize="off" @keyup.enter="_login" autofocus="autofocus" name="loginId">
          <i class="iconfont icon-msnui-user"></i>
        </div>
      </div> -->
      <div class="input-box-new flex">
        <div class="ibn-title">用户帐号：</div>
        <div class="ibn-warp flex">
          <input type="text" placeholder="手机号" class="i-ipnput" v-model="phone" @keyup.enter="_register" autofocus="autofocus" autocomplete='off'>
          <i class="iconfont icon-msnui-user"></i>
        </div>
      </div>
      <div class="input-box-new flex">
        <div class="ibn-title">登录密码：</div>
        <div class="ibn-warp flex">
          <input type="text" placeholder="密码" class="i-ipnput" v-model="password" @keyup.enter="_register" onfocus="this.type='password'" autocomplete='off'>
          <i class="iconfont icon-mima"></i>
        </div>
      </div>
      <div class="input-box-new flex">
        <div class="ibn-title">确认密码：</div>
        <div class="ibn-warp flex">
          <input type="text" placeholder="再次输入密码" class="i-ipnput" v-model="rePassword" @keyup.enter="_register" onfocus="this.type='password'" autocomplete='off'>
          <i class="iconfont icon-mima"></i>
        </div>
      </div>
      <label class="input-box-new flex" for='code-input'>
        <div class="ibn-title">手机验证：</div>
        <div class="ibn-warp flex">
          <input type="text" placeholder="验证码" class="i-ipnput code" id="code-input" v-model="code" @keyup.enter="_register">
          <div class="flex getcode">
            <div class="flex getcode-btn cursor" @click="_getcode">{{time}}
              <span v-if="time > 0">s后重新获取</span>
            </div>
          </div>
          <i class="iconfont icon-fasongyoujian"></i>
        </div>
      </label>
      <div class="btn flex modifybtn" @click="_register">注册</div>
      <div class="flex login-item">
        <router-link tag="div" class="to-register ellipsis cursor flex" to="/login">已有帐号？立即登录</router-link>
      </div>
    </div>
  </div>
</template>
<script type="text/javascript">
import { sendVerify, register } from 'api/login'
import { normalMixin } from 'common/js/mixin'
import { SUCCESS_CODE } from 'api/config'
// import { Judge } from 'common/js/judge'
export default {
  mixins: [normalMixin],
  data() {
    return {
      time: '获取验证码',
      phone: '',
      rePassword: '',
      password: '',
      code: ''
    }
  },
  created() {
    this._setTime()
    this.$root.eventHub.$emit('canvas')
  },
  methods: {
    _register() {
      if (!this._verifyPhone(this.phone) || !this._verifyPassword(this.password) || !this._verifyRePassword(this.password, this.rePassword) || !this._verifyCode(this.code)) {
        return false
      }
      register(this.phone, this.password, this.rePassword, this.code).then((res) => {
        if (res.data.err_code === SUCCESS_CODE) {
          // this.$message({
          //   showClose: true,
          //   message: '注册成功',
          //   type: 'success'
          // })
          this.$parent._open('注册成功')
          this.$router.replace({
            path: '/login',
            query: {
              phone: this.phone
            }
          })
          // window.location.href = window.location.origin + '/agent.html#/login?phone=' + this.phone
          return true
        } else {
          if (res.data.err_msg) {
            this.code = ''
            this.$parent._open(this.$root.errorCode[res.data.err_code])
          } else {
            this.password = ''
            this.rePassword = ''
            this.code = ''
            this.$parent._open('似乎出错了')
          }
        }
      })
    },
    _setTime() {
      let nowTime = +new Date()
      let lastTime = localStorage.getItem('codeTime') || 0
      console.log(nowTime)
      console.log(lastTime)
      if (nowTime < lastTime) {
        this._countdown(parseInt((lastTime - nowTime) / 1000))
      }
    },
    _getcode() {
      if (typeof this.time === 'string') {
        if (!this._verifyPhone(this.phone)) {
          return false
        }
        let lastTime = +new Date() + 60 * 1000
        localStorage.setItem('codeTime', lastTime)
        this._countdown(60)
        this.netSendCode()
      } else {
        this.$parent._open('请稍后再试哦')
      }
    },
    _countdown(time) {
      this.time = time
      let timer = setInterval(() => {
        if (this.time > 1) {
          this.time = this.time - 1
        } else {
          this.time = '重新获取验证码'
          clearInterval(timer)
          timer = null
        }
      }, 1000)
    },
    netSendCode() {
      sendVerify(this.phone).then((res) => {
        if (res.data.err_code === SUCCESS_CODE) {
          this.$parent._open('验证码已发送')
        } else {
          if (res.data.err_msg) {
            this.$parent._open(this.$root.errorCode[res.data.err_code])
          } else {
            this.$parent._open('似乎出错了')
          }
        }
      })
    }
  },
  components: {}
}

</script>
<style type="text/css" scoped>
.code {
  width: 40%;
}

.getcode {
  width: auto;
  min-width: 25%;
  height: 100%;
  justify-content: flex-start;
  overflow: hidden;
}

.getcode-btn {
  width: auto;
  height: 100%;
  margin-right: 5px;
  color: #ff9430;
}

.to-register {
  flex-grow: 1;
  flex-shrink: 0;
  min-width: 30%;
  color: #ff9430;
  margin: 0 20px;
}

.iconfont {
  font-size: 20px;
  margin: 0 10px;
}

.input-box {
  margin-bottom: 20px !important;
}

</style>
