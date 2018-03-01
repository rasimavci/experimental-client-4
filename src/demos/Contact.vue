<template>
  <div>

      <button-tab>
        <button-tab-item selected @click.native="showdata = 'all'">{{ $t('All Contacts') }}</button-tab-item>
        <button-tab-item @click.native="showdata = 'favorites'">{{ $t('Favorites') }}</button-tab-item>
      </button-tab>

      <!-- render calllogs in a list -->
      <div class="container">
        <div class="list-group">
          <a v-for="logrecord in getContacts" :key='logrecord.recordId' class="list-group-item" href="#" @click="updateActiveCall(logrecord)">
            <h4 class="list-group-item-heading">
              <i :class="[
                  'glyphicon',
                  logrecord['direction'] === 'incoming' ? 'glyphicon-import' : 'glyphicon-export'
                  ]"></i>
              {{logrecord.firstName}} {{logrecord.lastName}}
            </h4>
          </a>
        </div>
      </div>

    <div style="padding:2px;">
      <x-button @click.native="onCreateContact" type="primary">Create</x-button>
    </div>
    <div style="padding:2px;">
      <x-button @click.native="edit=true" type="primary">{{ $t('Edit') }}</x-button>
    </div>
    <div style="padding:2px;">
      <x-button @click.native="addContact" type="primary"> {{ $t('Add') }} </x-button>
    </div>


  <div v-transfer-dom>
      <alert v-model="currentPageAddContact" :title="$t('Smart Office')" @on-show="onShow" @on-hide="onHide"> {{ $t('Version 4.~') }}</alert>
    </div>
    <div v-transfer-dom>
      <popup v-if="this.currentPageAddContact" height="100%">
        <div class="popup1">

<div id="block_container">
    <div id="bloc1" @click="show1 = false">Back</div>
    <div id="bloc3">Create Profile</div>
    <div id="bloc2" @click="show1 = false">Save</div>
</div>

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



        </div>
      </popup>
    </div>


    <div v-transfer-dom>
      <popup v-model="edit" height="100%">
        <div class="popup1">

<div id="block_container">
    <div id="bloc1" @click="edit = false">Back</div>
    <div id="bloc3">Edit Profile</div>
    <div id="bloc2" @click="edit = true">Edit</div>
</div>

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
import { ButtonTab, ButtonTabItem, Divider, XImg, Checklist, Alert, Radio, TransferDom, Popup, Group, Cell, XButton, XSwitch, XInput, Toast, XAddress, ChinaAddressData } from 'vux'
import { mapState, mapGetters } from 'vuex'

export default {
  ...mapState({
    currentPageAddContact: state => state.vux.currentPageAddContact
  }),
  created: function () {
    this.$store.dispatch('updateCurrentPage', 'contact')
    this.$store.dispatch('updateShowPlacement', 'right')
  },
  directives: {
    TransferDom
  },
  components: {
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
      showdata: 'all',
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
    },
    addcontact () {
      this.add = true
      this.$store.dispatch('updateCurrentPage', 'addcontact')
    },
    onClickBack () {
      // this.add = true
    }
  },
  computed: {
    ...mapGetters(['mystate']),
    getContacts () {
      if (this.showdata === 'all') {
        console.log(this.$store.state.vux.contacts)
        return this.$store.state.vux.contacts
      } else if (this.showdata === 'favorites') {
        return this.$store.state.vux.contacts // this.$store.state.vux.contacts.filter(note => note.direction === 'incoming')
      }
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
