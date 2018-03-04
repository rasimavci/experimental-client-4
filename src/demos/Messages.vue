<template>
  <div>


      <button-tab>
        <button-tab-item selected @click.native="show = 'all'">{{ $t('All') }}</button-tab-item>
        <button-tab-item @click.native="show = 'incoming'">{{ $t('Unread Messages') }}</button-tab-item>
      </button-tab>

    <group :title="$t('This will be data for the group')">
          <a v-for="conv in getConversations2" :key='conv.deneme' class="list-group-item" href="#" @click.native="getDate(conv)">
      <img  src="../assets/demo/avatar_generic.png">
      <cell :title="$t(conv.conversationId)"></cell>
          </a>
    </group>

    <group :title="$t('This will be data for the group')">
          <a v-for="conv in getConversations" :key='conv.deneme' class="list-group-item" href="#" @click.native="getDate(conv)">
     <img  src="../assets/demo/avatar_generic.png">
      <cell :title="$t(conv[0].conversationId)"></cell>
          </a>
          <a v-for="conv in getConversations" :key='conv.deneme' class="list-group-item" href="#" @click.native="getDate(conv)">
      <img  src="../assets/demo/avatar_generic.png">
      <cell :title="$t(conv[1].conversationId)"></cell>
          </a>
          <a v-for="conv in getConversations" :key='conv.deneme' class="list-group-item" href="#" @click.native="getDate(conv)">
      <img  src="../assets/demo/avatar_generic.png">
      <cell :title="$t(conv[2].conversationId)"></cell>
          </a>
    </group>

    <group :title="$t('This will be data for the group')" v-model="showConversation">
          <a v-for="conv in getConversations2" :key='conv.deneme' class="list-group-item" href="#" @click.native="getDate(conv)">
      <img  src="../assets/demo/avatar_generic.png">
      <cell :title="$t(conv[0].conversationId)"></cell>
          </a>
    </group>

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
import { Cell, ButtonTab, ButtonTabItem, Divider, Confirm, Group, XSwitch, XButton, TransferDomDirective as TransferDom } from 'vux'
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
          console.log(conv.conversationId)
          // console.log(Moment(timestamp).calendar())
          // return Moment(timestamp).calendar()
        }
      })
    }
  },
  computed: {
    getConversations2 () {
      if (this.show === 'all') {
        console.log('conversations2 ' + this.$store.state.vux.conversations2)
        return this.$store.state.vux.conversations2
      } else if (this.show === 'incoming') {
        return this.$store.state.vux.conversations.filter(note => note.direction === 'incoming')
      }
    },
    getConversations () {
      if (this.show === 'all') {
        console.log('conversations ' + this.$store.state.vux.conversations)
        console.log('conversations1 ' + this.$store.state.vux.conversations[1])
        let conv = this.$store.state.vux.conversations[0]
        console.log('conv ' + conv[1].conversationId)
        return this.$store.state.vux.conversations
      } else if (this.show === 'incoming') {
        return this.$store.state.vux.conversations.filter(note => note.direction === 'incoming')
      }
    },
    getMessages () {
      return this.$store.state.vux.conversations
    }
    // mapState({
    //    results: state => state.vux.conversations
    // })
  }
}
</script>
