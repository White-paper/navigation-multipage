<template>
  <div id="main-body">
    <img src="http://p70pqu6ys.bkt.clouddn.com/%E6%8F%92%E7%94%BB.png" alt="星空网红助手" class="shpc login-pic">
    <div class="login-box" id="login-box">
      <div class="login-box-title-new flex">
        <div class="login-title-new">修改密码</div>
        <!--         <div class="min-font flex">修改当前密码</div> -->
      </div>
      <div class="input-box-new flex">
        <div class="ibn-title">旧的密码：</div>
        <div class="ibn-warp flex">
          <input type="password" placeholder="旧的密码" class="i-ipnput" v-model="password" readonly onfocus="this.removeAttribute('readonly');" autocomplete='off' name='old-password'>
          <i class="iconfont icon-mima"></i>
        </div>
      </div>
      <div class="input-box-new flex">
        <div class="ibn-title">新的密码：</div>
        <div class="ibn-warp flex">
          <input type="password" placeholder="设置新的密码" class="i-ipnput" v-model="newPassword" readonly onfocus="this.removeAttribute('readonly');" autocomplete='off' name='new-password'>
          <i class="iconfont icon-mima"></i>
        </div>
      </div>
      <div class="input-box-new flex">
        <div class="ibn-title">确认密码：</div>
        <div class="ibn-warp flex">
          <input type="password" placeholder="再次输入新的密码" class="i-ipnput" v-model="reNewPassword" readonly onfocus="this.removeAttribute('readonly');" autocomplete='off' name='new-repassword'>
          <i class="iconfont icon-mima"></i>
        </div>
      </div>
      <div class="btn flex modifybtn" @click="_sublime">修改密码</div>
    </div>
  </div>
</template>
<script type="text/javascript">
import { updatePassword } from 'api/login'
import { normalMixin } from 'common/js/mixin'
import { testToken } from 'common/js/util'
import { mapGetters, mapMutations } from 'vuex'
import { SUCCESS_CODE } from 'api/config'
// import { Judge } from 'common/js/judge'
export default {
  mixins: [normalMixin],
  data() {
    return {
      password: '',
      newPassword: '',
      reNewPassword: ''
    }
  },
  created() {
    // this.$root.eventHub.$emit('canvas', true)
  },
  computed: {
    ...mapGetters([
      'user',
      'token',
      'tokenTime'
    ])
  },
  methods: {
    checkTock() {
      if (!this.user) {
        this.$message({
          showClose: true,
          message: '请登录',
          type: 'warning'
        })
        window.location.href = window.location.origin + '/login.html#/login'
        return false
      }
      if (!testToken(this.tokenTime)) {
        this.setUser(false)
        this.setToken(false)
        this.setTokenTime(false)
        this.$message({
          showClose: true,
          message: '登录已失效',
          type: 'warning'
        })
        window.location.href = window.location.origin + '/login.html#/login'
        return false
      }
      return true
    },
    _sublime() {
      if (!this.checkTock()) {
        return false
      }
      if (!this._verifyOldPassword(this.password) || !this._verifyPassword(this.newPassword) || !this._verifyRePassword(this.newPassword, this.reNewPassword)) {
        return false
      }
      updatePassword(this.password, this.newPassword, this.reNewPassword, this.token).then((res) => {
        if (res.data.err_code === SUCCESS_CODE) {
          this.setUser(false)
          this.setToken(false)
          this.$parent._open('设置成功，请重新登录')
          this.password = ''
          window.location.href = window.location.origin + '/login.html#/login'
        } else {
          this.password = ''
          if (res.data.err_msg) {
            this.$parent._open(this.$root.errorCode[res.data.err_code])
          } else {
            this.$parent._open('似乎出错了')
          }
        }
      })
    },
    ...mapMutations({
      setToken: 'SET_TOKEN',
      setUser: 'SET_USER',
      setScorerate: 'SET_SCORERATE',
      setTokenTime: 'SET_TOKENTIME'
    })
  },
  components: {},
  watch: {
    password(val, oldval) {
      if (val.length >= oldval.length + 6) {
        this.password = ''
        this.$parent._open('请手动输入您的旧密码')
      }
    }
  }
}

</script>
<style type="text/css" scoped>
.login-box-title {
  width: 100%;
  height: 105px;
  font-size: 18px;
  flex-wrap: wrap;
  align-content: center;
}

.iconfont {
  font-size: 20px;
  margin: 0 10px;
}

.modifybtn {
  margin: 0 auto 30px;
}

.input-box {
  margin-bottom: 20px !important;
}

</style>
