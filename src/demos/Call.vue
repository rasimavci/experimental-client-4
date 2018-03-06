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
              <x-button @click.native="prev" type="primary" @click="makeCall(true)">Audio Call {{activeCall.calleeName}}</x-button>
              </div>
              <div class="box"  v-show="item === 'VOICE'">
              <x-button @click.native="prev" type="primary" @click="makeCall(false)">Video Call {{activeCall.calleeName}}</x-button>
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
           <popup v-model="speakerSelection" height="33%">
                      <div class="popup1">

    <popup-header
    :left-text="$t('')"
    :right-text="$t('Cancel')"
    :title="$t()"
    @on-click-left="edit = false"
    @on-click-right="speakerSelection = false">
    </popup-header></popup-header>
            <group title="Speaker Selection">
      <radio :options="radio001" @on-change="change"></radio>
    </group>

    </div>
      </popup>
</div>


  </div>
</template>

<script>
import { Tabbar, TabbarItem, Tab, TabItem, Sticky, Swiper, SwiperItem, Confirm, Rater, CellBox, CellFormPreview, PopupHeader, ButtonTab, ButtonTabItem, Divider, XImg, Checklist, Alert, Radio, TransferDom, Popup, Group, Cell, XButton, XSwitch, XInput, Toast, XAddress, ChinaAddressData } from 'vux'
import { mapState } from 'vuex'

const list = () => ['CHAT', 'VOICE', 'VIDEO', 'PEOPLE']

export default {
  ...mapState({
    currentPageAddContact: state => state.vux.currentPageAddContact,
    joinStarted: state => state.vux.joinStarted
  }),
  created: function () {
    this.$store.dispatch('updateCurrentPage', 'call')
    this.$store.dispatch('updateShowPlacement', 'right')
  },
  directives: {
    TransferDom
  },
  components: {
    Tabbar,
    TabbarItem,
    Tab,
    TabItem,
    Sticky,
    Swiper,
    SwiperItem,
    Confirm,
    Rater,
    CellBox,
    CellFormPreview,
    PopupHeader,
    ButtonTab,
    ButtonTabItem,
    Divider,
    XImg,
    Checklist,
    Alert,
    Radio,
    Popup,
    Group,
    Cell,
    XSwitch,
    XInput,
    Toast,
    XAddress,
    XButton
  },
  data () {
    return {
      activeCallTo: '',
      confirmDelete: false,
      selectedContact: {},
      showdata: 'all',
      list: [],
      error: false,
      checklist001: true,
      src: 'https://o5omsejde.qnssl.com/demo/test1.jpg',
      labelPosition: 'left',
      type: '',
      show2: false,
      addressData: ChinaAddressData,
      data1: 1,
      data2: 1,
      data3: 1,
      show: false,
      create: false,
      contact: false,
      edit: false,
      add: false,
      favorites: false,
      show1: false,
      show3: false,
      show4: false,
      show5: false,
      show6: false,
      title6: 'The default is empty',
      value6: [],
      show7: false,
      showToast: false,
      show8: false,
      show9: false,
      show10: false,
      show11: false,
      show12: false,
      show13: false,
      commonList: [ 'Show Presence Status' ],
      commonList1: [ 'Manage Favorites' ],
      commonList2: [ 'Remove From Personal Contacts List' ],
      commonList3: [ 'Remove From Personal Contacts List' ],
      radio001: [ 'Speakerohone', 'Earpiece', 'Bluettoh Headset', 'Stereo Mix (Realtek High Definition Audio)' ],
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
    change (val, label) {
      console.log('change', val, label)
    },
    onCreateContact () {
      this.$store.dispatch('updateAddContact', true)
    },
    log (str) {
      console.log(str)
    },
    logout () {
      this.$store.dispatch('disconnect')
    },
    onImgError (item, $event) {
      console.log(item, $event)
    },
    onCancel () {
      console.log('on cancel')
    },
    onConfirm () {
      console.log('contact deleted')
    },
    onConfirm5 () {
    },
    onShow () {
    },
    onShow5 () {
    },
    onHide () {
    },
    success () {
    },
    addcontact () {
      this.add = true
      this.$store.dispatch('updateCurrentPage', 'addcontact')
    },
    onClickBack () {
      // this.add = true
    },
    updateActiveCell (args) {
      if (this.$store.state.vux.joinStarted) {
        console.log('selected for join ' + args.primaryContact)
      } else {
        this.selectedContact = args
        this.contact = true
        console.log('edit ' + args.firstName)
      }
    },
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
  watch: {
    show10 (val) {
      if (val) {
        setTimeout(() => {
          this.show10 = false
        }, 1000)
      }
    }
  }
}
</script>

<style lang="less" scoped>
@import '~vux/src/styles/close.less';

.popup0 {
  padding-bottom:15px;
  height:200px;
}
.popup1 {
  width:100%;
  height:100%;
}
.popup2 {
  padding-bottom:15px;
  height:400px;
}
.position-vertical-demo {
  background-color: #ffe26d;
  color: #000;
  text-align: center;
  padding: 15px;
}
.position-horizontal-demo {
  position: relative;
  height: 100%;
  .vux-close {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translateX(-50%) translateY(-50%) scale(4);
    color: #000;
  }
  .img1 {
    float: right;
  }
  .img2 {
    float: left;
  }
}
#block_container
{
    text-align:center;
}

#bloc1
{
    display:inline;
    float:left;
}

#bloc2
{
    display:inline;
    float:right;
}

#bloc3
{
    display:inline;
    float:center;
}
</style>
