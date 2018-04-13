<template>
  <div>

    <group :title="$t('Presence')">
      <radio title="type" v-model="type" :options="['Available', 'Busy', 'On Vacation', 'Away', 'Out to Lunch', 'Be Right Back', 'Appear Offline']"></radio>
    </group>


    <div style="padding:2px;">
      <x-button @click.native="show=true" type="primary">About</x-button>
    </div>
    <div style="padding:2px;">
      <x-button @click.native="logout()" type="primary">{{ $t('Logout') }}</x-button>
    </div>
    <div style="padding:2px;">
      <x-button @click.native="show1=true" type="primary"> {{ $t('Send Feedback') }} </x-button>
    </div>

    <div v-transfer-dom>
      <alert v-model="show" :title="$t('Smart Office')" @on-show="onShow" @on-hide="onHide"> {{ $t('Version 4.~') }}</alert>
    </div>

    <div v-transfer-dom>
      <popup v-model="show1" height="100%">
        <div class="popup1">
          <group>
        <label>ISSUE DESCRIPTION</label>
      <x-input :placeholder="$t('Enter your feedback')"></x-input>
            <x-switch title="Close" v-model="show1"></x-switch>
      <x-button @click.native="toast" type="primary">Send Feedback</x-button>
          </group>
        </div>
      </popup>
    </div>
  </div>
</template>

<script>
import { Alert, Radio, TransferDom, Popup, Group, Cell, XButton, XSwitch, XInput, Toast, XAddress, ChinaAddressData } from 'vux'

export default {
  created: function () {
    this.$store.dispatch('updateCurrentPage', 'presences')
    this.$store.dispatch('updateShowPlacement', 'right')
  },
  directives: {
    TransferDom
  },
  components: {
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
      type: '',
      show2: false,
      addressData: ChinaAddressData,
      show: false,
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
      show13: false
    }
  },
  methods: {
    log (str) {
      console.log(str)
    },
    logout (val) {
      this.$store.commit('SET_CONNECTED', false)
      setTimeout(() => {
        console.log('loggin out')
      }, 3000)
      this.$store.dispatch('disconnect')
      this.$router.push('/')
    },
    onImgError (item, $event) {
      console.log(item, $event)
    },
    onCancel () {
      console.log('on cancel')
    },
    onConfirm () {
    },
    onConfirm5 () {
    },
    onShow () {
    },
    onShow5 () {
    },
    onHide () {
    }
  },
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
}
</style>
