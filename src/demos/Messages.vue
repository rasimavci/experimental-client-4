<template>
  <div>


      <button-tab>
        <button-tab-item selected @click.native="show = 'all'">{{ $t('All') }}</button-tab-item>
        <button-tab-item @click.native="showConversation = true">{{ $t('Unread Messages') }}</button-tab-item>
      </button-tab>

    <group>
      <div v-for="conv in getConversations" :key='conv.recordId' class="list-group-item a" href="#" @click="showConversation1(conv)">
        <cell :title="`${conv.conversationId}`" value="Personal">
          <img slot="icon" src="../assets/demo/avatar_generic.png" />
        </cell>
      </div>
    </group>


    <div v-transfer-dom>
      <popup v-model="showConversation" height="100%">
        <div class="popup1">

    <popup-header
    :left-text="$t('Back')"
    :right-text="$t('Edit')"
    :title="$t('Inbox')"
    @on-click-left="showConversation = false"
    @on-click-right="favorites = true">
    </popup-header></popup-header>

        </div>
      </popup>
    </div>

  </div>
</template>

<i18n>
Toggle:
  zh-CN: 显示
Toggle show-input:
  zh-CN: 显示Prompt
Toggle_android:
  zh-CN: 安卓风格
Are you sure?:
  zh-CN: 确定咩？
Confirm deleting the item:
  zh-CN: 操作提示
Please input something:
  zh-CN: 请输入些什么
Plugin usage:
  zh-CN: 插件形式调用
Show:
  zh-CN: 显示
closeOnConfirm=false:
  zh-CN: 阻止自动关闭
Set default input value:
  zh-CN: 设置默认输入文字
Call prompt by using plugin:
  zh-CN: 插件形式调用prompt
</i18n>

<script>
import { Popup, PopupHeader, Cell, ButtonTab, ButtonTabItem, Divider, Confirm, Group, XSwitch, XButton, TransferDomDirective as TransferDom } from 'vux'
import Moment from 'moment'
// import { mapState } from 'vuex';

export default {
  created: function () {
    this.$store.dispatch('updateCurrentPage', 'messages')
    this.$store.dispatch('updateShowPlacement', 'right')
  },
  directives: {
    TransferDom
  },
  components: {
    Popup,
    PopupHeader,
    Cell,
    ButtonTab,
    ButtonTabItem,
    Divider,
    Confirm,
    Group,
    XSwitch,
    XButton
  },
  data () {
    return {
      conv: '',
      selectedContact: '',
      selectedConv: '',
      showConversation: false,
      moment: Moment,
      callee: '',
      activeCallRinging: false,
      activeCallExist: false,
      show: 'all',
      show2: false,
      show3: false,
      show4: false,
      show5: false
    }
  },
  methods: {
    getTime (timestamp) {
      let myStr = String(timestamp)
      let time1 = myStr.slice(0, 2) + ':' + myStr.slice(2)
      let time = time1.trim().substring(0, 5)
      console.log('time' + time)
      return time
    },
    onCancel () {
      console.log('on cancel')
    },
    onConfirm (msg) {
      console.log('on confirm')
      if (msg) {
        alert(msg)
      }
    },
    onConfirm4 () {
      console.log('on confirm')
      this.$vux.loading.show({
        transition: '',
        text: 'processing'
      })
      setTimeout(() => {
        this.$vux.loading.hide()
        this.show4 = false
      }, 1000)
    },
    onHide () {
      console.log('on hide')
    },
    onShow () {
      console.log('on show')
    },
    onShow5 () {
      this.$refs.confirm5.setInputValue('default')
    },
    onConfirm5 (value) {
      this.$refs.confirm5.setInputValue('')
      this.$vux.toast.text('input value: ' + value)
    },
    showPlugin () {
      this.$vux.confirm.show({
        title: 'Title',
        content: 'Content',
        onShow () {
          console.log('plugin show')
        },
        onHide () {
          console.log('plugin hide')
        },
        onCancel () {
          console.log('plugin cancel')
        },
        onConfirm () {
          console.log('plugin confirm')
        }
      })
    },
    showPlugin2 () {
      this.showPlugin()
    },
    showPlugin3 () {
      const _this = this
      this.$vux.confirm.prompt('123', {
        title: 'Title',
        onShow () {
          console.log('promt show')
          _this.$vux.confirm.setInputValue('set input value')
        },
        onHide () {
          console.log('prompt hide')
        },
        onCancel () {
          console.log('prompt cancel')
        },
        onConfirm (msg) {
          alert(msg)
        },
        getDate (conv) {
          this.showConversation = true
          console.log(conv.conversationId)
          this.selectedConv = conv
          // console.log(Moment(timestamp).calendar())
          // return Moment(timestamp).calendar()
        }
      })
    },
    showConversation1 (conv) {
      console.log('selected conv1 ' + JSON.stringify(conv))
      console.log('selected conv1 id ' + conv.conversationId)
      this.showConversation = true
      this.conv = conv
    },
    getMessages (conv) {
      let convList = this.$store.state.vux.conversations.conversations
      convList.forEach(conv => {
        console.log(conv)
      })
        // let mymsg = this.$store.state.vux.conversations.conversations[0].messages
        // console.log('messages ' + mymsg[0].timestamp)
        // return this.$store.state.vux.conversations.conversations[0].messages
    }

  },
  computed: {
    getConversations2 () {
      if (this.show === 'all') {
        let mymsg = this.$store.state.vux.conversations.conversations[0].messages
        console.log('messages ' + mymsg[0].timestamp)
        return this.$store.state.vux.conversations.conversations[0].messages
      } else if (this.show === 'incoming') {
        return this.$store.state.vux.conversations.filter(note => note.direction === 'incoming')
      }
    },
    getConversations () {
      if (this.show === 'all') {
        return this.$store.state.vux.conversations.conversations
      } else if (this.show === 'incoming') {
        return this.$store.state.vux.conversations.filter(note => note.direction === 'incoming')
      }
    },
    getMessages1 () {
      console.log('selected conv id ' + this.conv.conversationId)
      let convList = this.$store.state.vux.conversations.conversations
      console.log('selected conv1 ' + JSON.stringify(convList))
      return convList.filter(conv => conv.conversationId === this.conv.conversationId)
      // let convList = this.$store.state.vux.conversations.conversations
      // convList.forEach(conv => {
      //   if (conv.conversationId === conv1.conversationId) {
      //     console.log('conv ' + conv)
      //   }
      // })
      // this.showConversation = true
        // let mymsg = this.$store.state.vux.conversations.conversations[0].messages
        // console.log('messages ' + mymsg[0].timestamp)
        // return this.$store.state.vux.conversations.conversations[0].messages
    }
    // mapState({
    //    results: state => state.vux.conversations
    // })
  }
}
</script>
<style>
a {
    text-decoration: none;
}
</style>
