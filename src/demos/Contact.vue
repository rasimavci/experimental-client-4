<template>
  <div>


    <div style="padding:2px;">
      <x-button @click.native="show1=true" type="primary">Create</x-button>
    </div>
    <div style="padding:2px;">
      <x-button @click.native="edit=true" type="primary">{{ $t('Edit') }}</x-button>
    </div>
    <div style="padding:2px;">
      <x-button @click.native="add=true" type="primary"> {{ $t('Add') }} </x-button>
    </div>


  <div v-transfer-dom>
      <alert v-model="show" :title="$t('Smart Office')" @on-show="onShow" @on-hide="onHide"> {{ $t('Version 4.~') }}</alert>
    </div>
    <div v-transfer-dom>
      <popup v-model="show1" height="100%">
        <div class="popup1">
    <label>Create Screen</label>
          <group>
      <x-img :src="src" :webp-src="`${src}?type=webp`" @on-success="success" @on-error="error" class="ximg-demo" error-class="ximg-error" :offset="-100" container="#vux_view_box_body"></x-img>
          </group>
          <group :placeholder="$t('IDENTIFICATION')">
      <x-input :placeholder="$t('First Name')"></x-input>
      <x-input :placeholder="$t('Last Name*')"></x-input>
      <x-input :placeholder="$t('NickName*')"></x-input>
      <x-input :placeholder="$t('User ID')"></x-input>
      <x-input :placeholder="$t('Email')"></x-input>
          </group>
          <group :title="$t('CONTACT')">
      <x-input :placeholder="$t('Home*')"></x-input>
      <x-input :placeholder="$t('mobile*')"></x-input>
      <x-input :placeholder="$t('Work*')"></x-input>
          </group>

    <checklist :title="$t('SETTINGS')" :label-position="labelPosition" required :options="commonList" v-model="checklist001" @on-change="change"></checklist>

          <group>
            <x-switch title="Close" v-model="show1"></x-switch>
      <x-button @click.native="toast" type="primary">Create</x-button>
          </group>

        </div>
      </popup>
    </div>


    <div v-transfer-dom>
      <popup v-model="edit" height="100%">
        <div class="popup1">
        <label>Edit Screen</label>
          <group>

        <img  src="../assets/demo/call_outline_blue.png">
        <img src="../assets/demo/video_outline_blue.png">
         <img src="../assets/demo/dp_action_keyboard.png">

          </group>
          <group :placeholder="$t('IDENTIFICATION')">
      <x-input :placeholder="$t('First Name')"></x-input>
      <x-input :placeholder="$t('Last Name*')"></x-input>
      <x-input :placeholder="$t('NickName*')"></x-input>
      <x-input :placeholder="$t('User ID')"></x-input>
      <x-input :placeholder="$t('Email')"></x-input>
          </group>
          <group :title="$t('CONTACT')">
      <x-input :placeholder="$t('Home*')"></x-input>
      <x-input :placeholder="$t('mobile*')"></x-input>
      <x-input :placeholder="$t('Work*')"></x-input>
          </group>

    <checklist :label-position="labelPosition" required :options="commonList1" v-model="checklist001" @on-change="change"></checklist>
    <checklist :label-position="labelPosition" required :options="commonList2" v-model="checklist001" @on-change="change"></checklist>
    <checklist :label-position="labelPosition" required :options="commonList3" v-model="checklist001" @on-change="change"></checklist>

          <group>
            <x-switch title="Close" v-model="show1"></x-switch>
      <x-button @click.native="toast" type="primary">Save</x-button>
          </group>

        </div>
      </popup>
    </div>

    <div v-transfer-dom>
      <popup v-model="add" height="100%">
        <div class="popup1">
        <label>Add Screen</label>
          <group>

        <img  src="../assets/demo/call_outline_blue.png">
        <img src="../assets/demo/video_outline_blue.png">
         <img src="../assets/demo/dp_action_keyboard.png">

          </group>
          <group :title="$t('CONTACT')">
      <x-input :placeholder="$t('Home*')"></x-input>
      <x-input :placeholder="$t('mobile*')"></x-input>
      <x-input :placeholder="$t('Work*')"></x-input>
      <x-input :placeholder="$t('NickName*')"></x-input>
      <x-input :placeholder="$t('User ID')"></x-input>
      <x-input :placeholder="$t('Email')"></x-input>
          </group>

          <group :title="$t('SETTINGS')">

    <checklist :label-position="labelPosition" required :options="commonList1" v-model="checklist001" @on-change="change"></checklist>
    <checklist :label-position="labelPosition" required :options="commonList2" v-model="checklist001" @on-change="change"></checklist>
    <checklist :label-position="labelPosition" required :options="commonList3" v-model="checklist001" @on-change="change"></checklist>
          </group>
          <group>
            <x-switch title="Close" v-model="show1"></x-switch>
      <x-button @click.native="toast" type="primary">Save</x-button>
          </group>

        </div>
      </popup>
    </div>


  </div>
</template>

<script>
import { XImg, Checklist, Alert, Radio, TransferDom, Popup, Group, Cell, XButton, XSwitch, XInput, Toast, XAddress, ChinaAddressData } from 'vux'

export default {
  created: function () {
    this.$store.dispatch('updateCurrentPage', 'contact')
    this.$store.dispatch('updateShowPlacement', 'right')
  },
  directives: {
    TransferDom
  },
  components: {
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
      error: false,
      checklist001: true,
      src: 'https://o5omsejde.qnssl.com/demo/test1.jpg',
      labelPosition: 'left',
      type: '',
      show2: false,
      addressData: ChinaAddressData,
      show: false,
      edit: false,
      add: false,
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
      commonList3: [ 'Remove From Personal Contacts List' ]

    }
  },
  methods: {
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
    change () {
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
