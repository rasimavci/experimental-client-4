<template>
  <div>

    <div>
      <tab :line-width=2 active-color='#fc378c' v-model="index" class="my-class">
        <tab-item class="vux-center" :selected="demo2 === item" v-for="(item, index) in list2" @click="demo2 = item" :key="index">{{item}}</tab-item>
      </tab>

      <swiper v-model="index" height="400px" :show-dots="false" >
        <swiper-item v-for="(item, index) in list2" :key="index">
          <div class="tab-swiper vux-center">

<div class="call-button-container1">
            <div class="tabCenter" @click="sendMessage()" v-if = "index === 0">
    <group class="weui-cells_form">
      <x-input v-model="message" placeholder="Message" class="weui-cell_vcode">
        <img slot="right" class="weui-vcode-img" src="../assets/demo/send_message.png" @click="endCall()">
      </x-input>

            <x-input placeholder="Message" class="weui-cell_vcode">
        <img slot="right" class="weui-vcode-img" src="../assets/demo/send_message.png" @click="endCall()">
      </x-input>

    </group>

            </div>

            <div class="call-button-container" @click="makeCall(false)" v-if = "index === 1  && activeCall.state !== 'RINGING' && activeCall.state !== 'IN_CALL'">
              <div class="tabCenter">
            <img slot="icon" src="../assets/demo/call_outline_white.png">Call {{activeCall.calleeName}}
            </div>
            </div>

            <div class="call-button-container" @click="makeCall(true)" v-if = "index === 2  && activeCall.state !== 'RINGING'">
              <div class="tabCenter">
            <img slot="icon" src="../assets/demo/camera_outline_white.png">Video {{activeCall.calleeName}}
            </div>
            </div>


<div class="call-button-container-calling" @click="endCall()" v-if = "activeCall.state === 'RINGING' || activeCall.state === 'INITIALIZING'" >Calling {{activeCall.calleeName}}
  <label class="call-button-container-cancel">CANCEL</label>
</div>



     <div class='keypad' v-if = "index === 1 && activeCall.state === 'IN_CALL'">
        <div class='keypad-container'>
          <div>
            <button class='button' @click='press(1)'>
              <div class='keypad-button-number'>1</div>
              <div class='keypad-button-text'></div>
            </button>
            <button class='button' @click='press(2)'>
              <div class='keypad-button-number'>2</div>
              <div class='keypad-button-text'>
                ABC
              </div>
            </button>
            <button class='button' @click='press(3)'>
              <div class='keypad-button-number'>3</div>
              <div class='keypad-button-text'>
                DEF
              </div>
            </button>
          </div>
          <div>
            <button class='button' @click='press(4)'>
              <div class='keypad-button-number'>4</div>
              <div class='keypad-button-text'>
                GHI
              </div>
            </button>
            <button class='button' @click='press(5)'>
              <div class='keypad-button-number'>5</div>
              <div class='keypad-button-text'>
                JKL
              </div>
            </button>
            <button class='button' @click='press(6)'>
              <div class='keypad-button-number'>6</div>
              <div class='keypad-button-text'>
                MNO
              </div>
            </button>
          </div>
          <div>
            <button class='button' @click='press(7)'>
              <div class='keypad-button-number'>7</div>
              <div class='keypad-button-text'>
                PQRS
              </div>
            </button>
            <button class='button' @click='press(8)'>
              <div class='keypad-button-number'>8</div>
              <div class='keypad-button-text'>
                TUV
              </div>
            </button>
            <button class='button' @click='press(9)'>
              <div class='keypad-button-number'>9</div>
              <div class='keypad-button-text'>
                WXYZ
              </div>
            </button>
          </div>
          <div>
            <button class='button' @click="press('*')">
              *
            </button>
            <button class='button' @click='press(0)'>
              0
            </button>
            <button class='button' @click="press('#')">
              #
            </button>
          </div>
        </div>
      </div>

</div>

        <cell :title="`${activeCall.to}`" value="Corporate" v-if = "index === 3">
          <img slot="icon" src="../assets/demo/avatar_generic.png" />
        </cell>

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

          <popup-header :left-text="$t('')" :right-text="$t('Cancel')" :title="$t()" @on-click-left="edit = false" @on-click-right="speakerSelection = false">
          </popup-header>
          </popup-header>
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
  created: function () {
    console.log('active call ' + JSON.stringify(this.activeCall))
    console.log('callPageInitialAction value ' + this.callPageInitialAction + ' activeCallstate ' + this.activeCallState)
    this.index = this.callPageInitialAction
    if (!this.activeCallState || this.activeCallState === 'ENDED') {
      if (this.callPageInitialAction === 1) {
        this.makeCall(false)
      } else {
        this.makeCall(true)
      }
    }
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
      message: '',
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
      commonList: ['Show Presence Status'],
      commonList1: ['Manage Favorites'],
      commonList2: ['Remove From Personal Contacts List'],
      commonList3: ['Remove From Personal Contacts List'],
      radio001: ['Speakerohone', 'Earpiece', 'Bluettoh Headset', 'Stereo Mix (Realtek High Definition Audio)'],
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
      //     //  this.SET_CALL_OPTIONS(options)
      //     //    let incomingCallData = {
      //     //      callId: this.incomingCall.callId,
      //     //      active: false
      //     //    }
      //   this.$store.commit('SET_USER_WITHID', 'saynaci@genband.com')
      // console.log('activeCall State ' + this.activeCallState)
      console.log('activeCall State ' + this.activeCall.state)
      console.log('make call to ' + this.callee)
      if (!this.activeCallState || this.activeCallState === 'ENDED' || this.activeCallState === '') {
        const params = {
          callee: this.callee, // 'saynaci@genband.com',
          mode: mode
        }
        let options = [{ key: 'localVideoContainer', value: document.getElementById('localVideoContainer') },
        { key: 'remoteVideoContainer', value: document.getElementById('remoteVideoContainer') }
        ]
        params.options = options
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
      console.log('make call operation finished.')
    },
    endCall () {
      this.$store.dispatch('end')
    },
    sendMessage () {
      let messageToSend = {
        type: 'IM',
        text: this.message, // 'deneme', // this.callee
        userId: this.callee // 'bkocak@genband.com'
      }
      this.$store.dispatch('sendMessage', messageToSend)
      this.message = ''
    }
  },
  computed: mapState({
    callPageInitialAction: state => state.vux.callPageInitialAction,
    user: state => state.user,
    callee: state => state.vux.callee,
    activeCall: state => state.vux.activeCall,
    activeCallTo: state => state.vux.activeCall.to,
    activeCallRinging: state => state.activeCallRinging,
    activeCallExist: state => state.activeCallExist,
    activeCallInCall: state => state.activeCallInCall,
    activeCallOnHold: state => state.vux.activeCallOnHold,
    activeCallState: state => state.vux.activeCall.state,
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
  padding-bottom: 15px;
  height: 200px;
}

.popup1 {
  width: 100%;
  height: 100%;
}

.popup2 {
  padding-bottom: 15px;
  height: 400px;
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

#block_container {
  text-align: center;
}

#bloc1 {
  display: inline;
  float: left;
}

#bloc2 {
  display: inline;
  float: right;
}

#bloc3 {
  display: inline;
  float: center;
}

div.my-class {
  cursor: default;
}

.box {
  padding: 15px;
}

.tab-swiper {
  background-color: #fff;
  height: 800px;
}

.button {
  background-color: #4caf50;
  border: none;
  color: blue;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  height: 50px;
  width: 10px;
}


.tabCenter {
  display:table-cell;
  text-align: center;
  vertical-align: middle;
}

.tabCenter2 {
  display:table-cell;
  text-align: center;
  vertical-align: bottom;
}

.calling-message {
  font-family: lato-regular;
  font-size: 18px;
  color: black;
  text-align: center;
  margin-top: 20px;
}

.call-button-container1 {
  padding-top: 160px;
}
.call-button-container {
  margin: auto;
  width: 233px;
  height: 80px;
  vertical-align: middle;
  padding: 10px;
  background: #29A3D8;
  -webkit-border-radius: 4px;
  -moz-border-radius: 4px;
  border-radius: 4px;
  font-family: lato-bold;
  font-size: 17px;
  color: white;
}

.call-button-container-calling {
  margin: auto;
  width: 233px;
  height: 80px;
  vertical-align: middle;
  padding: 10px;
  background: white;
  -webkit-border-radius: 4px;
  -moz-border-radius: 4px;
  border-radius: 4px;
  font-family: lato-bold;
  font-size: 24px;
  color: black;
}

.call-button-container-cancel {
  margin: auto;
  width: 233px;
  height: 80px;
  vertical-align: middle;
  padding: 10px;
  background: white;
  -webkit-border-radius: 4px;
  -moz-border-radius: 4px;
  border-radius: 4px;
  font-family: lato-bold;
  font-size: 24px;
  color: blue;
}


.call-button {
  margin: auto;
  width: 233px;
  height: 74px;
  vertical-align: middle;
  padding: 10px;
  background: #29A3D8;
  -webkit-border-radius: 4px;
  -moz-border-radius: 4px;
  border-radius: 4px;
  font-family: lato-bold;
  font-size: 17px;
  color: white;
}

.ex1 {
    padding-top: 125px;
}

.call-button-icon {
  width: 60px;
  float: left;
  padding-top: 15px;
}

.action {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;
}

.keypad {
  text-align: center;
  padding: 0;
  margin: 0;
  overflow: hidden;
  border: 0px solid black;
  height: 35%
}

.input-number {
  width: 100%;
  margin: 0 auto;
  /* margin-top: 5px; */
  border: 0;
  background-color: #eceff0;
  height: 50px;
  font-size: 1.5em;
  text-align: center;
}

.keypad-container {
  overflow: hidden;
  border: 0px solid black;
  padding: 5px;
    margin: auto;
    width: 75%;
}

.keypad-container div {
  width: 100%;
  border: 0px;
}

.keypad-container div button {
  width: 33.33%;
  padding: 0;
  margin: 0;
  display: block;
  float: left;
  height: 70px;
  border: 0px solid gray;
  background: white;
  color: #1E90FF;
  border-radius: 5px;
  font-size: 1.5em;
  margin: 5px 0;
}

.swiper {
   position: absolute;
   transform: translateY(50%); /* or translateX(50%) -  */
   transform: translateY(-50%); /* translateX(-50%) */
}

.parent {
  display: flex;
  justify-content: center;
  align-items: center;
}

.child {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}
</style>
