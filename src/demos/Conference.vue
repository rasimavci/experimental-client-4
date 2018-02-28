<template>
  <div>
    <br>
        </br>
    <div style="padding:2px;">
      <x-button @click.native="showPlugin" type="primary">{{ $t('START COLLABORATION') }}</x-button>
    </div>
<label>You can also send an automoted invitation</label>
    <div style="padding:2px;">
      <x-button @click.native="showPlugin" type="primary">{{ $t('Invıte By Email') }}</x-button>
    </div>

    <br>
        </br>


    <div style="padding:2px;">
      <x-button @click.native="showPlugin" type="primary">{{ $t('Connection Information for your permanent collaboration room') }}</x-button>
    </div>

<label>Url</label>



<label>{{conferenceData.videoConferenceRoomURL || conferenceData.webCollabRoomURL}}</label>

<label>MeetmeAccess Code</label>

<label>{{conferenceData.accessCode}}</label>

<label>{{conferenceData.confBridgeNumList}}</label>


    <group :title="$t('Presence')">
      <radio title="type" v-model="type" :options="['Available', 'Busy', 'On Vacation', 'Away', 'Out to Lunch', 'Be Right Back', 'Appear Offline']"></radio>
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
import { Confirm, Group, XSwitch, XButton, Panel, Radio } from 'vux'

export default {
  components: {
    Panel,
    Group,
    Radio,
    Confirm,
    XSwitch,
    XButton
  },
  methods: {
    onImgError (item, $event) {
      console.log(item, $event)
    }
  },
  data () {
    return {
      conferenceData: {},
      type: '1',
      list: [{
        src: 'http://somedomain.somdomain/x.jpg',
        fallbackSrc: 'http://placeholder.qiniudn.com/60x60/3cc51f/ffffff',
        title: '标题一',
        desc: 'A giant spherical object made of various materials, called the planet. The planet has a certain shape, with its own orbit.',
        url: '/component/cell'
      }, {
        src: 'http://placeholder.qiniudn.com/60x60/3cc51f/ffffff',
        title: '标题二',
        desc: 'A giant spherical object made of various materials, called the planet. The planet has a certain shape, with its own orbit.',
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
