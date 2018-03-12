<template>
  <div>

    <group :title="'Connection information for your permanent conference room'">
      <cell :title="'https://pa.nuviacloud.com/collab/avci@genband.com'">
        <img slot="icon" src="../assets/demo/genband.png" />
      </cell>
    </group>

    <group :title="'You can also send an automated invitation'">
      <cell :title="'Invite By Email'">
        <img slot="icon" src="../assets/demo/email.png" align="right"/>
      </cell>
    </group>

    <br>
        </br>
    <div style="padding:2px;">
      <x-button @click.native="showPlugin" type="primary">{{ $t('START CONFERENCE') }}</x-button>
    </div>

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
import { Cell, Confirm, Group, XSwitch, XButton, Panel, Radio } from 'vux'

export default {
  components: {
    Cell,
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
