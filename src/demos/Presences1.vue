<template>
  <div>
    <group :title="$t('Presence')">
      <radio title="type" v-model="type" :options="['Available', 'Busy', 'On Vacation', 'Away', 'Out to Lunch', 'Be Right Back', 'Appear Offline']"></radio>
    </group>

    <br>
    <div v-transfer-dom>
      <confirm v-model="show3"
      show-input
      :title="$t('Confirm deleting the item')"
      :input-attrs="{type: 'number'}"
      @on-click="onCancel"
      @on-cancel="onCancel"
      @on-confirm="onConfirm"
      @on-show="onShow"
      @on-hide="onHide">
      </confirm>
    </div>
    <br>
    <div v-transfer-dom>
      <confirm v-model="show5"
      show-input
      ref="confirm5"
      :title="$t('Confirm deleting the item')"
      @on-cancel="onCancel"
      @on-confirm="onConfirm5"
      @on-show="onShow5"
      @on-hide="onHide">
      </confirm>
    </div>
    <br>

    <group>
      <x-switch :title="$t('Show Me')" v-model="show"></x-switch>
    </group>
    <div v-transfer-dom>
      <alert v-model="show" :title="$t('Smart Office')" @on-show="onShow" @on-hide="onHide"> {{ $t('Version 4.~') }}</alert>
    </div>

    <div v-transfer-dom>
      <alert v-model="show2" :title="$t('Congratulations')" :content="$t('Your Message is sent successfully~')"></alert>
    </div>

    <div v-transfer-dom>
      <popup v-model="show1" height="100%">
        <div class="popup1">
          <group>
            <x-switch title="Another XSwitcher" v-model="show1"></x-switch>
          </group>
        </div>
      </popup>
    </div>

    <div style="padding:2px;">
      <x-button @click.native="show=true" type="primary">About</x-button>
    </div>

    <div style="padding:2px;">
      <x-button @click="show2" type="primary">{{ $t('About') }}</x-button>
    </div>
    <div style="padding:2px;">
      <x-button @click.native="logout()" type="primary">{{ $t('Logout') }}</x-button>
    </div>
    <div style="padding:2px;">
      <x-button @click.native="show1=true" type="primary"> {{ $t('Send Feedback') }} </x-button>
    </div>



    <group>
      <x-switch title="Full popup" v-model="show1"></x-switch>
    </group>


  </div>
</template>

<i18n>
Switch the type:
  zh-CN: Switch the style
List of content with image:
  zh-CN: Graphic portfolio list
More:
  zh-CN: see more
</i18n>

<script>
import { Alert, Confirm, Group, XSwitch, XButton, Panel, Radio, TransferDomDirective as TransferDom } from 'vux'

export default {
  directives: {
    TransferDom
  },
  components: {
    Panel,
    Group,
    Radio,
    Confirm,
    XSwitch,
    XButton,
    Alert
  },
  methods: {
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
    logout () {
      this.$store.dispatch('disconnect')
    }
  },
  data () {
    return {
      show1: false,
      show: false,
      show2: false,
      show3: true,
      show5: false,
      type: '1',
      list: [{
        src: 'http://somedomain.somdomain/x.jpg',
        fallbackSrc: 'http://placeholder.qiniudn.com/60x60/3cc51f/ffffff',
        title: '标题一',
        desc: '由各种物质组成的巨型球状天体，叫做星球。星球有一定的形状，有自己的运行轨道。',
        url: '/component/cell'
      }, {
        src: 'http://placeholder.qiniudn.com/60x60/3cc51f/ffffff',
        title: '标题二',
        desc: '由各种物质组成的巨型球状天体，叫做星球。星球有一定的形状，有自己的运行轨道。',
        url: {
          path: '/component/radio',
          replace: false
        },
        meta: {
          source: '来源信息',
          date: '时间',
          other: '其他信息'
        }
      }],
      footer: {
        title: this.$t('more'),
        url: 'http://vux.li'
      }
    }
  }
}
</script>
<style lang="less" scoped>
@import '~vux/src/styles/close.less';

.popup1 {
  width:100%;
  height:100%;
}
</style>
