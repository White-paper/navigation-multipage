<template>
  <div>
    <header class="header flex">
      <nav class="phone-nav flex" id="phone-nav" @click="_showSidebar">
        <i class="iconfont icon-gengduo"></i>
      </nav>
      <div class="logo flex">
        <img src="http://p8sxtcg6t.bkt.clouddn.com/defual.png" class="logo-class" ref='logo'>
      </div>
      <nav class="pc-nav flex" id="pc-nav">
        <div class="nav-ul flex">
          <!-- window.location.pathname + window.location.hash -->
          <el-menu :default-active="_windowLoction" class="el-menu-demo" mode="horizontal" @select="handleSelect" text-color="#000" active-text-color="#ff9430">
            <el-menu-item index="/">主页</el-menu-item>
            <el-menu-item index="/qzone.html#/download/0">空间刷赞</el-menu-item>
            <el-menu-item index="/ks.html#/download/1">快手刷粉</el-menu-item>
            <el-menu-item index="/dy.html#/download/2">抖音刷粉</el-menu-item>
            <el-menu-item index="/none/1" disabled>
              <div @click="_windowopen('http://xkfans.com')" class="log-out">网红代刷</div>
            </el-menu-item>
            <el-menu-item index="/agent.html">
              申请代理
              <!-- <div @click="_windowopen('http://xkfans.com/#/agent')" class="log-out">申请代理</div> -->
            </el-menu-item>
            <el-menu-item index="/cantact.html">联系我们</el-menu-item>
            <el-submenu index="/none" v-show="user">
              <template slot="title">我的账户</template>
              <el-menu-item index="/none" class='phone-item disable flex agent-item' v-show="proxyRank && user" disabled>
                <!-- {{proxyRank}} -->
                <div class="agent-ul-warp flex cursor">
                  代理等级: {{proxyRank}}
                  <img :src="proxyIcon" v-if="proxyIcon" class="proxy-icon">
                </div>
                <div class="agent-border-bootom cursor"></div>
              </el-menu-item>
              <el-menu-item index="/none" class='phone-item flex' v-show="user" disabled>
                <div class="agent-ul-li flex cursor">
                  <div class="agent-ul-li-left flex ellipsis">我的账户</div>
                  <div class="agent-ul-li-right flex ellipsis">{{userPhone}}</div>
                </div>
              </el-menu-item>
              <el-menu-item index="/none" v-show="user.is_agency && user.agency_level && !user.agency.sub_domain && !FENZAN" class='flex' disabled>
                <div class="agent-ul-li flex cursor" @click="_tofenzan('http://xkfans.com/#/index')">
                  <div class="agent-ul-li-left flex ellipsis">创建分站</div>
                  <div class="agent-ul-li-right flex ellipsis"></div>
                </div>
              </el-menu-item>
              <el-menu-item index="/none" v-show="user.agency && user.agency.sub_domain && user.agency.sub_site && !FENZAN" class='flex'>
                <div class="agent-ul-li flex cursor" @click="_tofenzan('http://xkfans.com/#/index')">
                  <div class="agent-ul-li-left flex ellipsis">分站管理</div>
                  <div class="agent-ul-li-right flex ellipsis"></div>
                </div>
              </el-menu-item>
              <el-menu-item index="/login.html#/modify-password" v-show="user" class='flex'>
                <div class="agent-ul-li flex cursor">
                  <div class="agent-ul-li-left flex ellipsis">修改密码</div>
                  <div class="agent-ul-li-right flex ellipsis"></div>
                </div>
              </el-menu-item>
              <el-menu-item index="/none" class=' flex' v-show="user" disabled>
                <!-- <div class="log-out-min log-out" @click="_logout($event)">注销</div> -->
                <div class="agent-ul-li flex cursor" @click="_logout($event)">
                  <div class="agent-ul-li-left flex ellipsis">注销</div>
                  <div class="agent-ul-li-right flex ellipsis"></div>
                </div>
              </el-menu-item>
            </el-submenu>
            <el-menu-item index="/login.html#/login" v-show="!user">登录帐号</el-menu-item>
          </el-menu>
        </div>
      </nav>
    </header>
    <sidebar ref="sidebar">
      <el-row class="tac">
        <el-col>
          <el-menu :default-active="_windowLoction" class="el-menu-vertical-demo" @select="handleSelect" text-color="#000" active-text-color="#ff9430">
            <el-menu-item index="/">
              主页
            </el-menu-item>
            <el-menu-item index="/qzone.html#/download/0">
              空间刷赞
            </el-menu-item>
            <el-menu-item index="/ks.html#/download/1">
              快手刷粉
            </el-menu-item>
            <el-menu-item index="/dy.html#/download/2">
              抖音刷粉
            </el-menu-item>
            <el-menu-item index="/none/1" disabled>
              <div @click="_windowopen('http://xkfans.com')" class="log-out">网红代刷</div>
            </el-menu-item>
            <el-menu-item index="/agent.html">
              申请代理
              <!-- <div @click="_windowopen('http://xkfans.com/#/agent')" class="log-out">申请代理</div> -->
            </el-menu-item>
            <el-menu-item index="/cantact.html">
              联系我们
            </el-menu-item>
          </el-menu>
        </el-col>
      </el-row>
    </sidebar>
    <!-- 2018.04.21-申请代理 -->
    <popup ref='agent' :protocol='true'>
      <div v-show='!hadAgree'>
        <div class="recharge-box-title-agent flex">代理合作协议</div>
        <div class="agreement-content">
          <p>尊敬的用户您好，感谢您使用“星空网红助手平台”网站及相关服务！ 根据《中华人民共和国合同法》及相关法律法规的规定，用户和星空网红助手平台（以下简称我平台）双方本着自愿、平等、诚信的原则，就用户购买星空网红助手平台相关服务及有关事宜达成如下条款:</p>
          <div class="agreement-content-title flex">第一条 代理服务内容</div>
          <p>1、我平台同意该用户代理销售相关互联网在线服务，我平台将全力支持配合用户的销售行为。</p>
          <p> 2、用户的具体可销售服务内容和产品价格，以网站当前实际报价为准。</p>
          <div class="agreement-content-title flex">第二条 代理费用及支付</div>
          <p>1、用户需缴纳代理费用方可成为我平台代理，获取代理服务销售价格和相关支持。代理费用为年费，仅为一年的使用费用。</p>
          <p>2、我平台给用户提供的服务费用，采取预付费的形式进行结算，以用户使用服务等功能的实际价格进行收取。</p>
          <p>3、用户的实际购买价格折扣，根据用户的代理等级有不同折扣，实际折扣以当前实际情况为准。</p>
          <div class="agreement-content-title flex">第三条 用户的权利及义务</div>
          <p>1、用户按本协议约定享有我平台产品服务的使用权、产品升级服务及客户服务。</p>
          <p>2、用户购买我平台产品服务，应按本协议的约定按时向我平台支付相应的服务费用。</p>
          <p>3、用户必须遵守国家有关法律、法规的规定，不得利用我平台服务从事国家法律法规所禁止的活动，并对利用我平台服务使用行为及数据内容的合法性承担完全责任。</p>
          <p>4、如我平台服务与本协议约定及官方公布的条款或内容不符，用户有权单方提前终止我平台服务的使用，并可申请退还截至终止之日至尚未抵扣的费用。</p>
          <div class="agreement-content-title flex">第四条 “星空网红助手平台”的权利及义务 </div>
          <p>1、我平台按本协议约定向用户提供技术服务及本协议约定的产品升级服务。</p>
          <p> 2、我平台将持续对我平台的服务平台提供升级服务，包括且不限于：功能优化、功能新增、第三方集成插件等功能和服务，相关升级服务将会以协议约定的电话、邮件或提前在我平台网站显著位置公示等有效方式通知用户。 </p>
          <p>3、我平台将基于我平台服务质量提升的考虑，可能调整或优化服务内容或服务方式，所有调整或优化的服务内容和服务方式将会以协议约定的电话、邮件或提前在我平台网站显著位置公示等有效的方式通知用户。</p>
          <p>4、我平台服务平台在协议约定版本内的我平台自行开发或拥有知识产权的功能优化、功能新增等升级服务将免费为用户提供。</p>
          <p>5、我平台对用户使用我平台服务的数据实行 7*24 小时技术支持。如果发现用户出现非法行为，我平台有权冻结用户账号直至用户查清停止非法行为为止，同时用户应积极配合自查非法行为，将信息准确提供给我平台以备国家相关监管部门审查。 </p>
          <div class="agreement-content-title flex">第五条 费用续费和协议延续</div>
          <p>1、用户需在其已经确认的订单的服务期满前 30 日内确认是否续费，用户已支付的订单服务期满且未在 30 日内完成续费订单确认及费用支付的，我平台有权在用户确认的订单服务期满后中止向用户服务，但我平台产品服务平台会将用户账号中的数据自到期之日起保存30 天，此期间称为保留期。 </p>
          <p>2、如用户在保留期内完成续费订单确认及费用支付的，我平台将恢复用户的服务及账号中的数据，在保留期内未及时完成续费订单确认及费用支付的，我平台将在保留期结束后不再保存用户账号中的数据，由此造成的一切后果由用户自行承担。 </p>
          <p>3、保留期满后用户仍未完成续费订单确认及费用支付的，本协议自动终止。如用户仍有预付款未抵扣的，视为用户放弃上述预付款 </p>
          <div class="agreement-content-title flex">第六条 保密条款 </div>
          <p>1、保密信息指双方合作过程中由披露方向接收方披露的所有保密信息，包括但不限于商业计划、客户信息、服务协议、技术数据、产品构思、产品价格、职员名单、操作手册等。</p>
          <p>2、保密信息还包括用户使用我平台产生的数据及相关内容，以及涉及我平台与本协议有关的所有信息，包括但不限于我平台的服务内容、优惠费率等。 </p>
          <p>3、用户和我平台双方不得将保密信息向第三方或者公众透露，否则承担由此给保密信息所有方造成的一切损失。 </p>
          <p>4、保密责任不因本协议的终止而终止。 </p>
          <div class="agreement-content-title flex">第七条 知识产权 </div>
          <p>1、 我平台完全拥有我平台服务及产品的全部知识产权（包括但不限于版权、商标权、专利权及商业秘密等），用户签署本协议后并未拥有产品的所有权。我平台的名字、标志、LOGO、产品名字等属于我平台，未经我平台授权，其他方不得使用。 </p>
          <p>2、用户完全拥有在使用我平台服务过程中产生的数据及相关内容的所有权，除法律允许的情况下，我平台不得使用用户的文件数据及相关内容。 </p>
          <div class="agreement-content-title flex">第八条 不可抗力 </div>
          <p>1、因不可抗力事件，致使一方无法履行本协议，发生不可抗力的一方有权提前终止本协议，在不可抗力的影响范围内得以免除责任，但应在上述情况发生后 5 个工作日内书面通知并提供有效证明。</p>
          <p>2、不可抗力事件包括但不限于：国家政策调整、机房中继线路调整、电信运营商机房维护检修、网络互联互通链接不通畅、电信运营商政策调整与影响。</p>
          <p>3、鉴于我平台为基于互联网应用的产品服务平台的特殊性，因黑客、病毒、电信部门技术调整等引起的事件，用户亦认同不属于我平台违约。但我平台应尽最大努力保护用户权益并在事件结束后立即恢复服务。</p>
          <div class="agreement-content-title flex">第九条 违约责任 </div>
          <p> 1、违约金条款适用于正式的付费服务周期。</p>
          <p> 2、除本协议另有约定外，任何一方违反本协议而使另一方发生任何费用或开支或额外责任或遭受损失的，违约方应在收到非违约方书面通知之日起十个工作日内就该费用、开支、责任或损失，给予非违约方赔偿。 </p>
          <p>3、如用户违反本协议及附件相关约定，我平台有权提前解除服务协议，并不退还用户所支付的服务费用；如给我平台造成损失的，用户应当赔偿我平台实际发生的损失。</p>
          <p>4、如我平台所提供的服务和本协议及附件相关约定不符，用户有权提前解除协议，我平台退还用户所支付的服务费用</p>
          <div class="agreement-content-title flex">第十条 争议及解决 </div>
          <p>用户及我平台就本协议的任何争议须友好协商。如协商不成，用户及我平台均可向被告方所在地人民法院起诉。本协议之签署、效力、解释和执行以及本协议项下争议之解决均应适用中国法律</p>
          <div class="agreement-content-title flex">第十一条 其他 </div>
          <p>本协议的版权为我平台所有，我平台保留一切解释和修改的权利，并在修改后通知用户。</p>
          <p class="font-weight-agent">对于以上《代理合作协议》的各项内容，用户已全文阅读并完全理解。</p>
        </div>
        <div class="recharge-btn-box-after flex bottom">
          <div class="recharge-btn-sure-after flex sure-agent cursor" @click='_agree'>同意并继续</div>
          <div class="recharge-btn-sure-after flex cancel cursor" @click="_hiddenAgent">我再考虑考虑</div>
        </div>
      </div>
      <div v-show='hadAgree'>
        <div class="recharge-box-title-agent flex">提交代理申请</div>
        <div class="agreement-content overHiden">
          <div class="flex agree-input-box">
            <div class="agree-label flex ellipsis">公司简称<span class="must">*</span></div>
            <div class="flex input-defult">
              <input type="text" placeholder="请填写公司简称" class="i-ipnput" v-model="companyName">
            </div>
          </div>
          <div class="flex agree-input-box">
            <div class="agree-label flex ellipsis">申请人<span class="must">*</span></div>
            <div class="flex input-defult">
              <input type="text" placeholder="请填写申请人" class="i-ipnput" v-model="applicant">
            </div>
          </div>
          <div class="flex agree-input-box">
            <div class="agree-label flex ellipsis">联系电话<span class="must">*</span></div>
            <div class="flex input-defult">
              <input type="text" placeholder="请填写联系电话" class="i-ipnput" v-model="telephone">
            </div>
          </div>
          <div class="flex agree-input-box">
            <div class="agree-label flex ellipsis">电子邮箱<span class="must">*</span></div>
            <div class="flex input-defult">
              <input type="text" placeholder="请填写电子邮箱" class="i-ipnput" v-model="mailBox">
            </div>
          </div>
          <div class="flex agree-input-box">
            <div class="agree-label flex ellipsis">申请需求<span class="not-must">(非必填)</span></div>
            <div class="flex input-defult">
              <input type="text" placeholder="请简要描述您的需求" class="i-ipnput" v-model="demand">
            </div>
          </div>
        </div>
        <div class="recharge-btn-box-after flex bottom">
          <div class="recharge-btn-sure-after flex sure-agent cursor" @click="_sublimeAgree">提交申请</div>
          <div class="recharge-btn-sure-after flex cancel cursor" @click="_hiddenAgent">我再考虑考虑</div>
        </div>
      </div>
    </popup>
    <el-dialog :title="dialogTitle" :visible.sync="centerDialogVisible" width="30vw" center :show-close="false" top="35vh">
      <div class="dialog-min-text flex">{{dialogText}}</div>
      <div class="dialog-min-btn flex cursor" @click="centerDialogVisible = false">知道了</div>
    </el-dialog>
    <interlayer ref="interlayer" @close='_interlayerHide'></interlayer>
  </div>
</template>
<script type="text/javascript" scoped>
import sidebar from 'components/sidebar/sidebar'
import interlayer from 'base/interlayer/interlayer'
import { mapGetters, mapMutations } from 'vuex'
import { agency } from 'api/header'
import popup from 'base/popup/popup'
import { testToken } from 'common/js/util'
import { getUserInfo } from 'api/index'
import { SUCCESS_CODE } from 'api/config'
const LOGIN = '/login.html#/login'
export default {
  data() {
    return {
      companyName: '',
      applicant: '',
      telephone: '',
      mailBox: '',
      demand: '',
      centerDialogVisible: false,
      sidebar: false,
      popup: false,
      agent: false,
      FENZAN: null,
      dialogText: '',
      dialogTitle: '',
      hadAgree: false,
      rank: ['青铜代理', '白银代理', '黄金代理', '王者代理'],
      iconList: ['http://p70pqu6ys.bkt.clouddn.com/%E7%AD%89%E8%AE%B01.png', 'http://p70pqu6ys.bkt.clouddn.com/%E7%AD%89%E7%BA%A72.png', 'http://p70pqu6ys.bkt.clouddn.com/%E7%AD%89%E7%BA%A73@2x.png']
    }
  },
  created() {
    this.$root.eventHub.$on('user', (location) => {
      this._updataUser(location)
    })
    this.$root.eventHub.$on('agent', () => {
      this._toShowAgent()
    })
    this.telephone = this.user.phone || ''
  },
  computed: {
    _windowLoction() {
      if (window.location.hash === '#/') {
        return window.location.pathname
      } else {
        return window.location.pathname + window.location.hash
      }
    },
    proxyRank() {
      if (this.user && this.user.agency && this.user.agency.level > 0) {
        return this.rank[this.user.agency.level - 1]
      } else {
        return '普通用户'
      }
    },
    proxyIcon() {
      if (this.user && this.user.agency && this.user.agency.level > 0) {
        return this.iconList[this.user.agency.level - 1]
      } else {
        return false
      }
    },
    userPhone() {
      if (this.user) {
        return this._formatUserPhone(this.user.phone)
      } else {
        return '未登录'
      }
    },
    ...mapGetters([
      'user',
      'token',
      'tokenTime'
    ])
  },
  methods: {
    _updataUser(location) {
      if (!this.checkTock()) {
        return false
      }
      this.money = ''
      getUserInfo(this.token).then((res) => {
        if (res.data.err_code === SUCCESS_CODE) {
          this.setUser(res.data.data)
          if (location) {
            this.$router.replace({
              path: location
            })
          }
        } else {
          if (res.data.err_msg) {
            this.$parent._open(this.$root.errorCode[res.data.err_code])
          } else {
            this.$parent._open('似乎出错了')
          }
        }
      })
    },
    _toShowAgent() {
      if (!this.user) {
        return false
      }
      if (this.user.is_agency) {
        this.dialogTitle = '您已提交过申请'
        this.dialogText = '无需重复申请, 我们的商务人员将在24小时内进行联系您! 请您耐心等待!'
        this.centerDialogVisible = true
        return false
      }
      this.$nextTick(() => {
        this.agent = true
        this.$refs.agent._showPopup()
        this.$refs.interlayer._setZIndex(9999)
        this.$refs.interlayer._showLayer()
      })
    },
    checkTock() {
      if (!this.user) {
        this.$parent._open('请登录')
        // this.$router.replace({
        //   path: '/login'
        // })
        window.location.href = window.location.origin + LOGIN
        return false
      }
      if (!testToken(this.tokenTime)) {
        // console.log('登录已失效 checkTock')
        this.setUser(false)
        this.setToken(false)
        this.setTokenTime(false)
        this.$parent._open('登录已失效')
        // this.$router.replace({
        //   path: '/login'
        // })
        window.location.href = window.location.origin + LOGIN
        return false
      }
      return true
    },
    _tofenzan(url) {
      window.location.href = url
    },
    _agree() {
      this.hadAgree = true
    },
    _hiddenAgent() {
      // clearInterval(this.timer)
      // this.timer = null
      // this.time = false
      this.hadAgree = false
      if (this.agent && !this.sidebar) {
        this.agent = false
        this.$refs.agent._hiddenPopup()
        this.$refs.interlayer._hiddenLayer()
      }
      if (this.agent && this.sidebar) {
        this.agent = false
        this.$refs.agent._hiddenPopup()
        this.$refs.interlayer._setZIndex(1500)
      }
    },
    _interlayerHide() {
      if (this.sidebar && !this.popup) {
        this.sidebar = false
        this.$refs.sidebar._hiddenSidebar()
        this.$refs.interlayer._hiddenLayer()
      }
    },
    _formatUserPhone(phone) {
      const start = phone.slice(0, 3)
      const end = phone.slice(-4)
      return start + '****' + end
    },
    _showSidebar() {
      this.sidebar = true
      this.$nextTick(() => {
        this.$refs.sidebar._showSidebar()
        this.$refs.interlayer._setZIndex(1500)
        this.$refs.interlayer._showLayer()
      })
    },
    // _toIndex() {
    //   this.$router.replace({
    //     path: '/'
    //   })
    // },
    _windowopen(src) {
      window.open(src)
    },
    _sublimeAgree() {
      if (!this.companyName) {
        this.$parent._open('请填写公司简称')
        return false
      }
      if (!this.applicant) {
        this.$parent._open('请填写申请人')
        return false
      }
      if (!this.telephone) {
        this.$parent._open('请填写联系电话')
        return false
      }
      if (!this._checkPhone(this.telephone)) {
        this.$parent._open('请正确填写联系电话')
        return false
      }
      if (!this.mailBox) {
        this.$parent._open('请填写您的电子邮箱')
        return false
      }
      if (!this._checkMail(this.mailBox)) {
        this.$parent._open('请正确填写您的电子邮箱')
        return false
      }
      if (!testToken(this.tokenTime)) {
        this.setUser(false)
        this.setToken(false)
        this.setTokenTime(false)
        this.$parent._open('登录已失效')
        // this.$router.replace({
        //   path: '/login'
        // })
        window.location.href = window.location.origin + LOGIN
        this._hiddenAgent()
        return false
      }
      agency(this.token, this.companyName, this.applicant, this.telephone, this.mailBox, this.demand).then((res) => {
        if (res.data.err_code === SUCCESS_CODE) {
          this.companyName = ''
          this.$root.eventHub.$emit('user')
          this.dialogTitle = '你的代理申请已成功提交！'
          this.dialogText = '我们的商务人员将在24小时内进行联系您! 请您耐心等待!'
          this.centerDialogVisible = true
          this._hiddenAgent()
        } else {
          if (res.data.err_msg) {
            this.telephone = ''
            this.mailBox = ''
            this.$parent._open(this.$root.errorCode[res.data.err_code])
          } else {
            this.telephone = ''
            this.mailBox = ''
            this.$parent._open('似乎出错了')
          }
        }
      })
    },
    handleSelect(key, keyPath) {
      // console.log(key)
      // console.log(window.location.origin)
      if (key.indexOf('/none') > -1) {
        return false
      }
      window.location.href = window.location.origin + key
    },
    _logout(e) {
      e.stopPropagation()
      this.setUser(false)
      this.setToken(false)
      this.setTokenTime(false)
      this.$parent._open('已注销')
      // this.$router.replace({
      //   path: '/login'
      // })
      window.location.href = window.location.origin + '/'
      // this.$root.eventHub.$emit('canvas', true)
    },
    _hiddenSidebar() {
      if (this.sidebar && !this.popup) {
        this.sidebar = false
        this.$refs.sidebar._hiddenSidebar()
        this.$refs.interlayer._hiddenLayer()
      }
      if (this.popup && !this.sidebar) {
        this.popup = false
        this.$refs.popup._hiddenPopup()
        this.$refs.interlayer._hiddenLayer()
      }
      if (this.popup && this.sidebar) {
        this.popup = false
        this.$refs.popup._hiddenPopup()
        this.$refs.interlayer._setZIndex(1500)
      }
    },
    ...mapMutations({
      setToken: 'SET_TOKEN',
      setUser: 'SET_USER',
      setTokenTime: 'SET_TOKENTIME'
    })
  },
  watch: {},
  components: {
    sidebar,
    interlayer,
    popup
  }
}

</script>
<style type="text/css" scoped>
.input-defult:hover {
  animation: gradientIndex .5s ease 1 forwards;
}

@keyframes gradientIndex {
  0% {
    border-bottom: 1px solid rgba(0, 0, 0, .1);
  }
  100% {
    border-bottom: 1px solid #ff9430;
  }
}

.iconfont {
  font-size: 20px;
  margin: 0 10px 0 0;
}

.header {
  height: auto;
  min-height: 65px;
  width: 100%;
  flex-wrap: wrap;
  background: #fff;
  box-shadow: 0px 1px 10px rgba(0, 0, 0, .1);
  overflow: hidden;
}

.phone-nav {
  width: 50px;
  height: 65px;
  flex-shrink: 0;
}

.pc-nav {
  height: 65px;
  flex-grow: 1;
  min-width: 30%;
  max-width: 100%;
  flex-shrink: 0;
}

.logo {
  height: 65px;
  max-width: 100%;
  min-width: 240px;
  flex-shrink: 0;
  flex-grow: 1;
}

.disable:hover {
  pointer-events: none;
}





















/*start ---改写我的账户下拉窗 2018.04.27*/

.phone-item {
  color: #000 !important;
  font-size: 13px;
  height: 50px;
}

.phone-item:hover {
  color: #000 !important;
}

.agent-item {
  height: 70px !important;
  width: 100%;
  flex-wrap: wrap;
  /*border-bottom: 1px solid red;*/
  position: relative;
}

.agent-ul-warp {
  width: 86%;
  height: 55%;
  border-radius: 5px;
  background: #FFD236;
  color: #000;
  font-size: 14px;
  transform: translate(0, -15%);
}

.agent-border-bootom {
  position: absolute;
  bottom: 0;
  left: 0;
  height: 1px;
  width: 100%;
  background: rgba(0, 0, 0, .1);
}

.agent-ul-li {
  box-sizing: content-box;
  min-width: 100%;
  height: 100%;
  font-size: 14px;
}

.agent-ul-li:hover {
  background: #FFD236 !important;
}

.agent-ul-li-left {
  width: 35%;
  height: 100%;
  justify-content: flex-start;
  padding-left: 5%;
  /*flex-grow: 1;*/
}

.agent-ul-li-right {
  width: 55%;
  height: 100%;
  justify-content: flex-end;
  padding-right: 5%;
}









/*end ---改写我的账户下拉窗*/

.icon-jifen {
  color: #777 !important;
}


.iconfont {
  color: #000;
}

.log-out {
  display: inline-block;
  height: 100%;
  width: 100%;
  line-height: 60px;
  pointer-events: auto;
  cursor: pointer;
  zoom: 1;
  margin-top: -3px;
  font-size: 15px;
}

.log-out-min {
  line-height: 36px;
}

.green-text {
  display: inline-block;
  font-size: 15px;
  color: #4CAF50;
  margin: -5px 10px 0;
}

.recharge-box {
  width: 100%;
  height: 100%;
  flex-wrap: wrap;
  justify-content: flex-start;
  align-content: center;
  position: relative;
}

.input-defult {
  width: 90%;
  height: 45px;
  margin: 10px auto;
  border-bottom: 1px solid rgba(0, 0, 0, .1);
}

.recharge-box-title {
  width: 90%;
  height: 70px;
  margin: 0 auto;
  font-size: 20px;
  font-weight: 600;
  color: #D94D37;
  border-bottom: 1px solid rgba(0, 0, 0, .2);
}

.recharge-box-title-agent {
  width: 100%;
  height: 70px;
  margin: 0 auto;
  font-size: 20px;
  font-weight: 600;
  color: #000;
  background: #FFD236;
}

.agreement-content {
  width: 82%;
  height: auto;
  min-height: 20px;
  /*  max-height: 500px;*/
  margin: 20px auto 100px;
  font-size: 15px;
  line-height: 26px;
  font-weight: normal;
  overflow-y: scroll;
  overflow-x: hidden;
}

.content-qr {
  width: 100%;
  height: auto;
  flex-wrap: wrap;
}

.net-item {
  width: 40%;
  height: 40px;
  background: #eee;
  color: #333;
  margin: 20px 5% 0;
  border: 1px solid rgba(0, 0, 0, .1);
}

.net-item:hover {
  cursor: pointer;
  /* color: #ff9430;*/
}

.recharge-btn-box {
  height: 70px;
  justify-content: flex-start;
}

.recharge-btn-box-after {
  width: 100%;
  height: 70px;
  margin-bottom: 10px;
}

.recharge-btn-sure {
  width: 35%;
  height: 46px;
  border-radius: 6px;
}

.recharge-btn-sure:nth-child(1) {
  margin: 0 20% 0 5%;
}

.recharge-btn-sure-after {
  width: 180px;
  height: 46px;
  border-radius: 6px;
  margin: 0 5%;
}

.recharge-btn-sure-after:nth-child(1) {
  /*margin: 0 10% 0 5%;*/
}

.min-tips-text {
  width: 100%;
  height: 20px;
  /*text-indent: 30px;*/
  padding-left: 5%;
  font-size: 13px;
  color: #999;
  justify-content: flex-start;
}

.course-btn {
  color: #42A5F5;
  text-indent: 0px;
}

.code-div {
  width: 100%;
  height: 40px;
}

.showWX {
  background-color: #E8F8FF;
  border: 1px solid #D9F3FF;
  border-radius: 20px;
  color: #55C2F5;
  padding: 10px;
  height: 10px;
  min-width: 120px;
  margin: 10px 0 0;
}

.sureBtn {
  border: 1px solid #ff9430;
  border-radius: 20px;
  color: #ff9430;
  padding: 10px;
  height: 10px;
  min-width: 120px;
  margin: 0 15px 10px;
}

.colseBtn {
  border: 1px solid #999;
  border-radius: 20px;
  color: #999;
  padding: 10px;
  height: 10px;
  min-width: 120px;
  margin: 0 15px 10px;
}

.colseBtn:hover {
  background: #666;
  color: #fff;
  cursor: pointer;
}

.sureBtn:hover {
  background: #ff9430;
  color: #fff;
  cursor: pointer;
}

.qrcode-box {
  width: 100%;
  flex-shrink: 0;
  height: auto;
}

.logo-class {
  width: 155px;
  height: 47px;
  /*  margin-left: -50px;*/
}

.logo-class:hover {
  cursor: pointer;
}

.pay-img {
  width: 144px;
  height: 44px;
  margin: 0 30px;
}

.my-money {
  color: #d94d37;
  text-indent: 0px;
}

.good-item {
  box-sizing: border-box;
  min-width: 75px;
  max-width: 28.33%;
  width: 75px;
  height: 75px;
  flex-shrink: 0;
  flex-grow: 1;
  background: #FFE8D2;
  color: #D94D37;
  margin: 5px 5% 0 0;
  flex-wrap: wrap;
  align-content: center;
  border-radius: 8px;
}

.good-item-label {
  width: 100%;
  height: 40%;
}

.pay-item {
  box-sizing: border-box;
  max-width: 29.3%;
  width: 29.3%;
  min-width: 100px;
  height: 45px;
  flex-shrink: 0;
  flex-grow: 0;
  /*margin-left: 5%;*/
  margin-top: 5px;
  margin-right: 3%;
  color: #353535;
  font-size: 15px;
  border: 1px solid rgba(0, 0, 0, .2);
  border-radius: 8px;
}

.pay-icon {
  max-width: 30px;
  width: auto;
  max-height: 26px;
  height: auto;
  margin-right: 2%;
  flex-shrink: 1;
}


.goods-box {
  min-height: 50px;
  height: auto;
  width: 95%;
  padding-left: 5%;
  overflow: hidden;
  justify-content: flex-start;
  margin: 20px auto 5px;
  flex-wrap: wrap;
}

.active-good {
  /*color: red;*/
  color: #fff;
  background: #FF6B4E;
}

.cancel {
  box-sizing: border-box;
  color: #353535;
  background: rgba(223, 225, 229, 1);
}

.sure {
  box-sizing: border-box;
  color: #fff;
  background: #FF6B4E;
  /* box-shadow: 2px 0px 8px rgba(157, 106, 95, 1);*/
}

.sure-agent {
  box-sizing: border-box;
  color: #000;
  background: #FFD236;
}

.active-pay-type {
  border: 1px solid #FF6B4E;
  color: #FF6B4E;
}

.sure:hover {
  box-shadow: 2px 0px 8px rgba(157, 106, 95, 1);
}

.cancel:hover {
  box-shadow: 2px 0px 8px rgba(0, 0, 0, .3);
}

.disable-pay-type {
  color: rgba(0, 0, 0, .2);
  cursor: not-allowed;
}

.bottom {
  position: absolute;
  bottom: 0;
}

.agreement-content-title {
  width: 100%;
  height: 40px;
  font-size: 18px;
  color: #000;
  justify-content: flex-start;
}


.marginbottom20 {
  margin-bottom: 30px;
}

.agree-input-box {
  margin: 0 auto 20px;
  /*margin-bottom: 30px;*/
  width: 100%;
  height: 50px;
  overflow: hidden;
}

.agree-label {
  width: auto;
  height: 100%;
  min-width: 130px;
  color: #666;
  font-size: 15px;
  margin: 0 10px;
  /* justify-content: flex-start;*/
}

.input-defult {
  width: auto !important;
  flex-grow: 1;
}

.overHiden {
  overflow: hidden;
}

.must {
  font-weight: 600;
  font-size: 13px;
  color: red;
}

.not-must {
  font-size: 12px;
  color: #999;
}

.width90 {
  width: 90% !important;
}

.dialog-min-text {
  width: 100%;
  height: auto;
  line-height: 20px;
  min-height: 20px;
  font-size: 15px;
  color: #999;
}

.dialog-min-btn {
  width: 40%;
  padding: 2% 0;
  border-radius: 5px;
  background: #FFD236;
  color: #000;
  font-size: 15px;
  margin: 5% auto 0;
}

.proxy-icon {
  max-width: 20px;
  height: auto;
  margin: 0 5px;
}

.font-weight-agent {
  font-weight: 600;
}

.agreement-content {
  /*border-bottom: 1px solid rgba(0,0,0,.1);
  */
  /*  box-shadow: 0 2px 1px rgba(0,0,0,.1);*/
}

.rbta-input-warp {
  width: 80%;
  height: 50px;
  border-radius: 10px;
  background: #f4f4f4;
  color: #353535;
  border: 1px solid #eee;
  font-size: 16px;
  margin: 50px auto 10px;
}

.rbata-iw-right {
  width: 30%;
  height: 100%;
}

.rbata-iw-left {
  width: 30%;
  height: 80%;
  outline: none;
  border: none;
  margin: 0 5%;
  border-radius: 10px;
  text-align: center;
  font-size: 16px;
}

.titps-domain {
  color: #FF9100;
}

.bdd-item {
  height: 50px;
  width: 100%;
  margin: 10px auto -10px;
}

.bdd-item-left {
  width: 16%;
  justify-content: flex-end;
  padding: 0 2%;
  font-size: 17px;
  color: #FF6B4E;
}

.bdd-item-right {
  width: 72%;
  justify-content: flex-start;
  margin: 0 7% 0 1%;
  color: #353535;
  font-size: 14px;
}

.bdd-item-right-url:hover {
  color: #FF6B4E;
}

.rbta-btn {
  background: #FFD236;
  border-radius: 10px;
  height: 80%;
  width: 25%;
  margin: 0 auto;
}

.sucess-domain {
  color: #67c23a;
}

.fail-domain {
  color: #e6a23c;
}

</style>
