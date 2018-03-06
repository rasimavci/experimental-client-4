<template>
  <div>


    <div>
       <tab :line-width=2 active-color='#fc378c' v-model="index">
       <tab-item class="vux-center" :selected="demo2 === item" v-for="(item, index) in list2" @click="demo2 = item" :key="index">{{item}}</tab-item>
      </tab>
      <swiper v-model="index" height="100px" :show-dots="false">
        <swiper-item v-for="(item, index) in list2" :key="index">
          <div class="tab-swiper vux-center">{{item}} Screen
              <div class="box"  v-show="item === 'VIDEO'">
              <x-button @click.native="prev" type="primary" @click="makeCall(true)">Call {{activeCallTo}}</x-button>
              </div>
              <div class="box"  v-show="item === 'VOICE'">
              <x-button @click.native="prev" type="primary" @click="makeCall(false)">Call {{activeCallTo}}</x-button>
              </div>
          </div>
        </swiper-item>
      </swiper>
    </div>

    <tabbar>
      <tabbar-item>
        <img slot="icon" src="../assets/demo/call_speaker_blue_outline.png" @click="speakerSelection = !speakerSelection">
      </tabbar-item>
      <tabbar-item>
        <img slot="icon" src="../assets/demo/add_party_outline_blue.png" @click="addContact(false)">
      </tabbar-item>
      <tabbar-item>
        <img slot="icon" src="../assets/demo/hold_outline_blue.png" @click="holdCall(false)">
      </tabbar-item>
      <tabbar-item>
        <img slot="icon" src="../assets/demo/mute_blue_outline.png" @click="muteCall(true)">
      </tabbar-item>
    </tabbar>

    <div v-transfer-dom>
      <popup v-model="speakerSelection" height="100%">
                      <div class="popup1">

            <group title="default">
      <radio :options="radio001" @on-change="change"></radio>
    </group>

    </div>
      </popup>
</div>


  </div>
</template>

<i18n>
set bar-active-color:
  zh-CN: 设置bar颜色
</i18n>

<script>
import { Group, Radio, Popup, Tabbar, TabbarItem, Tab, TabItem, Sticky, Divider, XButton, Swiper, SwiperItem } from 'vux'
import { mapState } from 'vuex'

const list = () => ['CHAT', 'VOICE', 'VIDEO', 'PEOPLE']

export default {
  created: function () {
    this.$store.dispatch('updateCurrentPage', 'call')
    this.$store.dispatch('updateShowPlacement', 'right')
  },
  computed: mapState({
    user: state => state.user,
    activeCall: state => state.vux.activeCall,
    activeCallTo: state => state.vux.activeCall.to,
    activeCallRinging: state => state.activeCallRinging,
    activeCallExist: state => state.activeCallExist,
    activeCallInCall: state => state.activeCallInCall,
    activeCallOnHold: state => state.activeCallOnHold,
    activeCallState: state => state.activeCallState,
    activeCallMuted: state => state.activeCallOnHold,
    activeCallsendingVideo: state => state.activeCallOnHold,
    activeCallEnded: state => state.activeCallOnHold,
    callstart: state => state.callstart,
    // callState: state => { if(callState === 'RINGING') { return true } else {return false}},
    isloadingComplete: state => state.isloadingComplete,
    busy: state => state.busy
  }),
  components: {
    Group,
    Radio,
    Popup,
    Tabbar,
    TabbarItem,
    Tab,
    TabItem,
    Sticky,
    Divider,
    XButton,
    Swiper,
    SwiperItem
  },
  data () {
    return {
      radio001: [ 'China', 'Japan' ],
      speakerSelection: false,
      index01: 0,
      list2: list(),
      demo2: 'Food',
      list3: ['Received news', 'Message sent'],
      demo3: 'Received news',
      list4: ['Being shown', 'coming soon'],
      demo4: 'coming soon',
      demoDisabled: 'A',
      index: 0,
      getBarWidth: function (index) {
        return (index + 1) * 22 + 'px'
      }
    }
  },
  methods: {
    addContact () {
      this.$store.dispatch('selectContactToAddCall', true)
      this.$router.push('contact')
    },
    holdCall (mode) {
      if (this.activeCall.state === 'ON_HOLD') {
        this.$store.dispatch('unhold', true)
      } else {
        this.$store.dispatch('hold', true)
      }
    },
    muteCall (mode) {
      console.log(this.activeCall)
      if (this.activeCall.muted) {
        this.$store.dispatch('unmute', true)
      } else {
        this.$store.dispatch('mute', true)
      }
    },
    switchTabItem (index) {
      console.log('on-before-index-change', index)
      this.$vux.loading.show({
        text: 'loading'
      })
      setTimeout(() => {
        this.$vux.loading.hide()
        this.index01 = index
      }, 1000)
    },
    onItemClick (index) {
      console.log('on item click:', index)
    },
    addTab () {
      if (this.list2.length < 5) {
        this.list2 = list().slice(0, this.list2.length + 1)
      }
    },
    removeTab () {
      if (this.list2.length > 1) {
        this.list2 = list().slice(0, this.list2.length - 1)
      }
    },
    next () {
      if (this.index === this.list2.length - 1) {
        this.index = 0
      } else {
        ++this.index
      }
    },
    prev () {
      if (this.index === 0) {
        this.index = this.list2.length - 1
      } else {
        --this.index
      }
    },
    makeCall (mode) {
  //   this.$store.commit('SET_USER_WITHID', 'saynaci@genband.com')
      console.log('activeCall State ' + this.activeCallState)
      console.log('activeCall State ' + this.activeCall.state)
      //     this.$store.dispatch('call', 'bkocak@genband.com')
      if (this.activeCallState === 'ENDED' || this.activeCallState === '') {
        const params = {
          callee: 'saynaci@genband.com',
          mode: mode
        }
        //    let incomingCallData = {
        //      callId: this.incomingCall.callId,
        //      active: false
        //    }
        // this.setIncomingCall(incomingCallData)
        this.$store.dispatch('call', params)
        //  this.$store.dispatch.answer(this.incomingCall.callId)
      // } else {
      //   this.$store.dispatch('end')
      } else {
        // debugger
        // this.toggleActiveCall()
        // debugger
        this.$store.dispatch('end')
      }
      console.log('make call')
    }
  }
}
</script>

<style lang="less" scoped>
@import '~vux/src/styles/1px.less';
@import '~vux/src/styles/center.less';

.box {
  padding: 15px;
}
.active-6-1 {
  color: rgb(252, 55, 140) !important;
  border-color: rgb(252, 55, 140) !important;
}
.active-6-2 {
  color: #04be02 !important;
  border-color: #04be02 !important;
}
.active-6-3 {
  color: rgb(55, 174, 252) !important;
  border-color: rgb(55, 174, 252) !important;
}
.tab-swiper {
  background-color: #fff;
  height: 100px;
}
.popup1 {
  width:100%;
  height:100%;
}
</style>
