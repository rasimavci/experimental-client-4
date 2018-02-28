<template>
  <div>

    <div class='modal-container' display='none'>
      <div class='input-group' v-show='!activeCallRinging'>
        <input type='text' class='form-control' v-model='callee' placeholder='Username or Number...'>
        <span class='input-group-btn'>
          <button class='backspace-button' type='button' @click="callee = ''">
            <i class='material-icons'>backspace</i>
          </button>
        </span>
      </div>

      <div align='center' class='h3' v-show='activeCallRinging || activeCallExist'>
        <h3>
          {{activeCallState}}
        </h3>

        <img class='media-object pull-center' :src='user.photoUrl'/>
      </div>
      <div class='keypad'>
        <div class='keypad-container' v-show='!activeCallExist  && !activeCallRinging'>
          <div>
            <button class='button' @click='showActiveCallRinging()'>
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
            <button @click='press(4)'>
              <div class='keypad-button-number'>4</div>
              <div class='keypad-button-text'>
                GHI
              </div>
            </button>
            <button @click='press(5)'>
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
            <button class='button' @click='connect()'>
              Login
            </button>
            <button class='button' @click='sendMessage()'>
              Send
            </button>
            <button class='button' @click='sendMessage2()'>
              Send2
            </button>
            <button class='button' @click='getPresence()'>
              Pre1
            </button>
            <button class='button' @click='presenceSubscribe()'>
              Subs
            </button>
          </div>
        </div>
      </div>
      <div class='keypad-menu'>

        <button class='button' @click='endCall()' v-show='activeCallRinging'>
          <i class='material-icons'>{{!activeCall? 'phone_in_talk' : 'not interested' }}</i>
        </button>

        <button class='button' @click='toggleHold()' v-show='activeCallExist'>
          <i class='material-icons'>{{!activeCall? 'pause' : 'not interested' }}</i>
        </button>

        <button class='button' @click='toggleMute()' v-show='activeCallExist'>
          <i class='material-icons'>{{!activeCall? 'mic' : 'not interested' }}</i>
        </button>

        <button class='button' @click='isShowKeypad = !isShowKeypad' v-show='activeCallInCall'>
          <i class='material-icons'>{{!activeCall? 'dialpad' : 'not interested' }}</i>
        </button>

        <button class='rasim-button' @click='makeCall()' v-show='!activeCallRinging'>
          <i class='icon_call_outline_blue'></i>
        </button>

        <button class='button' @click='toggleVideo()' v-show='!activeCallRinging'>
          <i class='material-icons'>{{!activeCallsendingVideo ? 'videocam' : 'videocam_off'}}</i>
        </button>

        <button class='button myhover' @click='toggleVideo()' v-show='!activeCallRinging'>
          <i class='material-icons'>{{!activeCallsendingVideo ? 'videocam' : 'videocam_off'}}</i>
        </button>

      </div>
      <div class='dropdown filterbox' :class="{ 'open': filterToggle }" v-show='activeCall'>
        <button type='button' class='button glyphicon glyphicon-option-horizontal' v-on:click='filterToggle = !filterToggle'>
          <i class='option-horizontal'></i>
        </button>
        <ul class='dropdown-menu list-group'>
          <li class='list-group-item' @click='transferCall()'>
            Transfer
          </li>
          <li class='list-group-item' @click='transferCall()'>
            <label>Join</label>
          </li>
          <li class='list-group-item' @click='transferCall()'>
            Mute
          </li>
          <li class='list-group-item' @click='transferCall()'>
            <label>Send Conference Link</label>
          </li>
          <li class='list-group-item' @click='transferCall()'>
            Hold
          </li>
          <li class='list-group-item' @click='transferCall()'>
            <label>End Call</label>
          </li>
        </ul>
      </div>
    </div>


    <tabbar>
      <tabbar-item>
        <img slot="icon" src="../assets/demo/dp_action_keyboard.png">
        <span slot="label">Wechat</span>
      </tabbar-item>
      <tabbar-item show-dot>
        <img slot="icon" src="../assets/demo/images.png">
        <span slot="label">Message</span>
      </tabbar-item>
      <tabbar-item selected link="/component/demo">
        <img slot="icon" src="../assets/demo/call_outline_blue.png">
        <span slot="label">Explore</span>
      </tabbar-item>
      <tabbar-item badge="2">
        <img slot="icon" src="../assets/demo/video_outline_blue.png">
        <span slot="label">News</span>
      </tabbar-item>
      <tabbar-item badge="2">
        <img slot="icon" src="../assets/demo/dp_action_voicemail.png">
        <span slot="label">News</span>
      </tabbar-item>
    </tabbar>
  </div>
</template>

<script>
import { Tabbar, TabbarItem, Group, Cell } from 'vux'
import { mapState } from 'vuex'

export default {
  created: function () {
  //  this.onPlacementChange('right')
    console.log('showPlacement value ')
    this.$store.dispatch('updateCurrentPage', 'dialpad')
    this.$store.dispatch('updateShowPlacement', 'right')
  },
  data () {
    return {
      isShowKeypad: true,
      activeCall: false,
      callee: '',
      joinClicked: false,
      activeNote: '',
      filterByStatus: '',
      filterToggle: false,
      target: '',
      show: 'all',
      options: [
        {
          label: 'Personal Address Book',
          value: 1
        },
        {
          label: 'Global Address Book',
          value: 2
        }
      ]
    }
  },
  components: {
    Tabbar,
    TabbarItem,
    Group,
    Cell
  },
  computed: mapState({
    user: state => state.user,
    activeCallRinging: state => state.activeCallRinging,
    activeCallExist: state => state.activeCallExist,
    activeCallInCall: state => state.activeCallInCall,
    activeCallOnHold: state => state.activeCallOnHold,
    activeCallState: state => state.activeCall.state,
    activeCallMuted: state => state.activeCall.muted,
    activeCallsendingVideo: state => state.activeCall.sendingVideo,
    activeCallEnded: state => state.activeCall.activeCallEnded,
    callstart: state => state.callstart,
    // callState: state => { if(callState === 'RINGING') { return true } else {return false}},
    isloadingComplete: state => state.isloadingComplete,
    busy: state => state.busy
  })
}
</script>
