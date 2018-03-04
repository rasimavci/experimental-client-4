<template>
  <div style="height:100%;">
    <div v-transfer-dom>
      <loading v-model="isLoading"></loading>
    </div>
    <div v-transfer-dom>
      <actionsheet :menus="menus" v-model="showMenu" @on-click-menu="changeLocale"></actionsheet>
    </div>

    <drawer
    width="200px;"
    :show.sync="drawerVisibility"
    :show-mode="showModeValue"
    :placement="showPlacementValue"
    :drawer-style="{'background-color':'#35495e', width: '200px'}">

      <!-- drawer content -->
      <div slot="drawer">
        <group title="Profile Rasim Avci" @click.native="drawerVisibility = false" v-show="this.stateShowPlacement === 'left'">
          <cell title="Presences" link="/component/presences" value="Presences" @click.native="drawerVisibility = false" v-show="this.showPlacementValue === 'left'">
          </cell>


          <cell title="Login" link="/component/login" @click.native="drawerVisibility = false" v-show="this.stateShowPlacement === 'left'">
          </cell>
          <cell title="Call" link="/component/call" @click.native="drawerVisibility = false" v-show="this.stateShowPlacement === 'left'">
          </cell>
          <cell data-v-5d4fb59a title="   Favorites" link="/component/favorites" @click.native="drawerVisibility = false" v-show="this.stateShowPlacement === 'left'">
            <span class="demo-icon-22 vux-demo-tabbar-icon-home" slot="icon" style="position:relative;top: -2px;">&#xe606;</span>
          </cell>
          <cell title="Contacts" link="/component/contact" @click.native="drawerVisibility = false" v-show="this.stateShowPlacement === 'left'">
          </cell>
          <cell title="Chat" link="/component/messages" @click.native="drawerVisibility = false" v-show="this.stateShowPlacement === 'left'">
            <span class="demo-icon-22 vux-demo-tabbar-icon-home" slot="icon" style="position:relative;top: -2px;">&#xe608;</span>
          </cell>
          <cell title="History" link="/component/history" @click.native="drawerVisibility = false" v-show="this.stateShowPlacement === 'left'">
            <span class="demo-icon-22 vux-demo-tabbar-icon-home" slot="icon" style="position:relative;top: -2px;">&#xe619;</span>
          </cell>
          <cell title="   Dialpad" link="/component/dialpad" @click.native="drawerVisibility = false" v-show="this.stateShowPlacement === 'left'">
            <span class="demo-icon-22 vux-demo-tabbar-icon-home" slot="icon" style="position:relative;top: -2px;">&#xe661;</span>
          </cell>
          <cell title="Sessions" link="/component/sessions" @click.native="drawerVisibility = false" v-show="this.stateShowPlacement === 'left'">
          </cell>
          <cell title="Collaboration" link="/component/conference" @click.native="drawerVisibility = false" v-show="this.stateShowPlacement === 'left'">
          </cell>
        </group>
        <group title="showMode" v-show="this.stateShowPlacement === 'left'">
          <radio v-model="showMode" :options="['push', 'overlay']" @on-change="onShowModeChange"></radio>
        </group>
        <group title="placement" v-show="this.stateShowPlacement === 'left'">
          <radio v-model="showPlacement" :options="['left', 'right']" @on-change="onPlacementChange"></radio>
        </group>
        </group>
        <group title="showMode" v-show="this.stateShowPlacement === 'left'">
          <radio v-model="showMode" :options="['push', 'overlay']" @on-change="onShowModeChange"></radio>
        </group>
        <group title="Manage" v-show="this.stateShowPlacement === 'right' && this.stateCurrentPage === 'contact'">
          <radio v-model="showPlacement" :options="['Create Contact']" @on-change="onCreateContact"></radio>
          <cell title="Create Contact" @click.native="onCreateContact" v-show="this.stateShowPlacement === 'right' && this.stateCurrentPage === 'contact'">
          </cell>
        </group>

        <group title="Sort By" v-show="this.stateShowPlacement === 'right' && this.stateCurrentPage === 'contact'">
          <radio v-model="showPlacement" :options="['Last Name', 'First Name', 'Availability']" @on-change="onPlacementChange"></radio>
        </group>
        <group title="Sources" v-show="this.stateShowPlacement === 'right' && this.stateCurrentPage === 'contact'">
          <radio v-model="showPlacement" :options="['Personal Addressbook', 'Global Addressbook']" @on-change="onPlacementChange"></radio>
        </group>
        <group title="Show..." v-show="this.stateShowPlacement === 'right' && this.stateCurrentPage === 'history'">
          <radio v-model="showPlacement" :options="['Incoming Call', 'Outgoing Call', 'Missed Call', 'Message', 'Conference']" @on-change="onPlacementChange"></radio>
        </group>
        <group title="Display As" v-show="this.stateShowPlacement === 'right' && this.stateCurrentPage === 'favorites'">
          <radio v-model="showPlacement" :options="['Tiles', 'List']" @on-change="onPlacementChange"></radio>
        </group>
        <group title="Sort By" v-show="this.stateShowPlacement === 'right' && this.stateCurrentPage === 'favorites'">
          <radio v-model="showPlacement" :options="['Last Name', 'First Name', 'Availability']" @on-change="onPlacementChange"></radio>
        </group>
        <group title="Display" v-show="this.stateShowPlacement === 'right' && this.stateCurrentPage === 'messages'">
          <radio v-model="showPlacement" :options="['New', 'Saved']" @on-change="onPlacementChange"></radio>
        </group>
      </div>

      <!-- main content -->
      <view-box ref="viewBox" body-padding-top="46px" body-padding-bottom="55px">

        <x-header slot="header"
        style="width:100%;position:absolute;left:0;top:0;z-index:100;"
        :left-options="leftOptions"
        :right-options="rightOptions"
        :title="title"
        :transition="headerTransition"
        @on-click-more="onClickMore">
          <span v-if="route.path === '/' || route.path === '/component/drawer'" slot="overwrite-left" @click="drawerVisibility = !drawerVisibility">
            <x-icon type="navicon" size="35" style="fill:#fff;position:relative;top:-8px;left:-3px;"></x-icon>
          </span>
        </x-header>



        <!-- remember to import BusPlugin in main.js if you use components: x-img and sticky -->
        <transition
        @after-enter="$vux.bus && $vux.bus.$emit('vux:after-view-enter')"
        :name="viewTransition" :css="!!direction">
          <router-view class="router-view"></router-view>
        </transition>

        <tabbar class="vux-demo-tabbar" icon-class="vux-center" v-show="this.stateCurrentPage !== 'dialpad' && this.stateCurrentPage !== 'call'" slot="bottom">
          <tabbar-item :link="{path:'/'}" :selected="route.path === '/'">
            <span class="demo-icon-22 vux-demo-tabbar-icon-home" slot="icon" style="position:relative;top: -2px;">&#xe637;</span>
            <span slot="label">Chat</span>
          </tabbar-item>
          <tabbar-item :link="{path:'/demo'}" :selected="isDemo" badge="9">
            <span class="demo-icon-22" slot="icon">&#xe633;</span>
            <span slot="label"><span v-if="componentName" class="vux-demo-tabbar-component">{{componentName}}</span><span v-else>Voicemail</span></span>
          </tabbar-item>
          <tabbar-item :link="{path:'/'}" :selected="route.path === '/'">
            <span class="demo-icon-22 vux-demo-tabbar-icon-home" slot="icon" style="position:relative;top: -2px;">&#xe637;</span>
            <span slot="label">Call</span>
          </tabbar-item>
        </tabbar>

      </view-box>
    </drawer>

  </div>
</template>

<script>
import { Radio, Group, Cell, Badge, Drawer, Actionsheet, ButtonTab, ButtonTabItem, ViewBox, XHeader, Tabbar, TabbarItem, Loading, TransferDom } from 'vux'
import { mapState, mapActions } from 'vuex'

export default {
  created: function () {
  //  this.onPlacementChange(this.stateShowPlacement)
  //  console.log('showPlacement value ' + this.stateShowPlacement)
    this.showPlacement = 'left'
    this.showPlacementValue = 'left'
  //  setTimeout(one => {
  //    this.drawerVisibility = !this.drawerVisibility
  //   }, 300)
  },
  directives: {
    TransferDom
  },
  components: {
    Radio,
    Group,
    Cell,
    Badge,
    Drawer,
    ButtonTab,
    ButtonTabItem,
    ViewBox,
    XHeader,
    Tabbar,
    TabbarItem,
    Loading,
    Actionsheet
  },
  methods: {
    onCreateContact () {
      this.$store.dispatch('updateAddContact', true)
    },
    onCellClick () {
      /** hide drawer before changing showMode **/
      console.log('cell clicked')
    },
    onShowModeChange (val) {
      /** hide drawer before changing showMode **/
      this.drawerVisibility = false
      setTimeout(one => {
        this.showModeValue = val
      }, 400)
    },
    onPlacementChange (val) {
      /** hide drawer before changing position **/
      this.drawerVisibility = false
      setTimeout(one => {
        this.showPlacementValue = val
      }, 400)
    },
    onClickMore () {
      if (this.stateShowPlacement === 'left') {
        this.showMenu = true
      } else {
        this.showPlacementValue = 'right'
        setTimeout(one => {
          this.drawerVisibility = !this.drawerVisibility
        }, 300)
      }
    },
    changeLocale (locale) {
      this.$i18n.set(locale)
      this.$locale.set(locale)
    },
    ...mapActions([
      'updateDemoPosition'
    ])
  },
  mounted () {
    this.handler = () => {
      if (this.path === '/demo') {
        this.box = document.querySelector('#demo_list_box')
        this.updateDemoPosition(this.box.scrollTop)
      } else if (this.path === '/contact') {
        this.box = document.querySelector('#contact')
        this.updateDemoPosition(this.box.scrollTop)
      }
    }
  },
  beforeDestroy () {
    this.box && this.box.removeEventListener('scroll', this.handler, false)
  },
  watch: {
    path (path) {
      if (path === '/component/demo') {
        this.$router.replace('/demo')
        return
      } else if (path === '/demo') {
        setTimeout(() => {
          this.box = document.querySelector('#demo_list_box')
          if (this.box) {
            this.box.removeEventListener('scroll', this.handler, false)
            this.box.addEventListener('scroll', this.handler, false)
          }
        }, 1000)
      } else if (path === '/contact') {
        setTimeout(() => {
          this.box = document.querySelector('#contact')
          if (this.box) {
            this.box.removeEventListener('scroll', this.handler, false)
            this.box.addEventListener('scroll', this.handler, false)
          }
        }, 1000)
      } else {
        this.box && this.box.removeEventListener('scroll', this.handler, false)
      }
    }
  },
  computed: {
    ...mapState({
      stateShowPlacement: state => state.vux.showPlacement,
      stateCurrentPage: state => state.vux.currentPage,
      route: state => state.route,
      path: state => state.route.path,
      deviceready: state => state.app.deviceready,
      demoTop: state => state.vux.demoScrollTop,
      isLoading: state => state.vux.isLoading,
      direction: state => state.vux.direction
    }),
    isShowBar () {
      if (/component/.test(this.path)) {
        return true
      }
      return false
    },
    leftOptions () {
      this.showPlacementValue = 'left'
      this.$store.dispatch('updateShowPlacement', 'left')
      this.$store.dispatch('updateCurrentPage', 'main')
      return {
        showBack: this.route.path !== '/'
      }
    },
    rightOptions () {
      return {
        showMore: true
      }
    },
    headerTransition () {
      if (!this.direction) return ''
      return this.direction === 'forward' ? 'vux-header-fade-in-right' : 'vux-header-fade-in-left'
    },
    componentName () {
      if (this.route.path) {
        const parts = this.route.path.split('/')
        if (/component/.test(this.route.path) && parts[2]) return parts[2]
      }
    },
    isDemo () {
      return /component|demo/.test(this.route.path)
    },
    isTabbarDemo () {
      return /tabbar/.test(this.route.path)
    },
    title () {
      if (this.route.path === '/') return 'Smart Office'
      if (this.route.path === '/project/donate') return 'Donate'
      if (this.route.path === '/demo') return 'Smart Office'
      if (this.route.path === '/contact') return 'Smart Office Contact'
      return this.componentName ? `Demo/${this.componentName}` : 'Demo/~~'
    },
    viewTransition () {
      if (!this.direction) return ''
      return 'vux-pop-' + (this.direction === 'forward' ? 'in' : 'out')
    }
  },
  data () {
    return {
      showMenu: false,
      menus: {
        'language.noop': '<span class="menu-title">Language</span>',
        'zh-CN': '中文',
        'en': 'English',
        'es': 'Espanol'
      },
      drawerVisibility: false,
      showMode: 'push',
      showModeValue: 'push',
      showPlacement: 'left',
      showPlacementValue: 'left'
    }
  }
}
</script>

<style lang="less">
@import '~vux/src/styles/reset.less';
@import '~vux/src/styles/1px.less';
@import '~vux/src/styles/tap.less';

body {
  background-color: #fbf9fe;
}
html, body {
  height: 100%;
  width: 100%;
  overflow-x: hidden;
}

.demo-icon-22 {
  font-family: 'vux-demo';
  font-size: 22px;
  color: #888;
}
.weui-tabbar.vux-demo-tabbar {
  /** backdrop-filter: blur(10px);
  background-color: none;
  background: rgba(247, 247, 250, 0.5);**/
}
.vux-demo-tabbar .weui-bar__item_on .demo-icon-22 {
  color: #F70968;
}
.vux-demo-tabbar .weui-tabbar_item.weui-bar__item_on .vux-demo-tabbar-icon-home {
  color: rgb(53, 73, 94);
}
.demo-icon-22:before {
  content: attr(icon);
}
.vux-demo-tabbar-component {
  background-color: #F70968;
  color: #fff;
  border-radius: 7px;
  padding: 0 4px;
  line-height: 14px;
}
.weui-tabbar__icon + .weui-tabbar__label {
  margin-top: 0!important;
}
.vux-demo-header-box {
  z-index: 100;
  position: absolute;
  width: 100%;
  left: 0;
  top: 0;
}

@font-face {
  font-family: 'vux-demo';  /* project id 70323 */
  src: url('https://at.alicdn.com/t/font_h1fz4ogaj5cm1jor.eot');
  src: url('https://at.alicdn.com/t/font_h1fz4ogaj5cm1jor.eot?#iefix') format('embedded-opentype'),
  url('https://at.alicdn.com/t/font_h1fz4ogaj5cm1jor.woff') format('woff'),
  url('https://at.alicdn.com/t/font_h1fz4ogaj5cm1jor.ttf') format('truetype'),
  url('https://at.alicdn.com/t/font_h1fz4ogaj5cm1jor.svg#iconfont') format('svg');
}

.demo-icon {
  font-family: 'vux-demo';
  font-size: 20px;
  color: #04BE02;
}

.demo-icon-big {
  font-size: 28px;
}

.demo-icon:before {
  content: attr(icon);
}

.router-view {
  width: 100%;
  top: 46px;
}
.vux-pop-out-enter-active,
.vux-pop-out-leave-active,
.vux-pop-in-enter-active,
.vux-pop-in-leave-active {
  will-change: transform;
  transition: all 500ms;
  height: 100%;
  top: 46px;
  position: absolute;
  backface-visibility: hidden;
  perspective: 1000;
}
.vux-pop-out-enter {
  opacity: 0;
  transform: translate3d(-100%, 0, 0);
}
.vux-pop-out-leave-active {
  opacity: 0;
  transform: translate3d(100%, 0, 0);
}
.vux-pop-in-enter {
  opacity: 0;
  transform: translate3d(100%, 0, 0);
}
.vux-pop-in-leave-active {
  opacity: 0;
  transform: translate3d(-100%, 0, 0);
}
.menu-title {
  color: #888;
}
</style>
