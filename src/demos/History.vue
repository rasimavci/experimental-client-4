<template>
  <div>


      <button-tab>
        <button-tab-item selected @click.native="show = 'all'">{{ $t('All') }}</button-tab-item>
        <button-tab-item @click.native="show = 'incoming'">{{ $t('Incoming') }}</button-tab-item>
        <button-tab-item @click.native="show = 'outgoing'">{{ $t('Outgoing') }}</button-tab-item>
        <button-tab-item @click.native="show = 'missed'">{{ $t('Missed') }}</button-tab-item>
      </button-tab>

    <group :title="$t('This will be data for the group')">
          <a v-for="logrecord in getCallLogs" :key='logrecord.recordId' class="list-group-item" href="#" @click="goCall(logrecord)">
    <cell :title="`  ${logrecord.callerName}`">{{logrecord.startTime.trim ().substring (0, 4)}}
      <img slot="icon" src="../assets/demo/avatar_generic.png" />
    </cell>
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
import { mapState } from 'vuex'

export default {
  ...mapState({
    historyFilterSelection: state => state.vux.historyFilterSelection
  }),

  created: function () {
    this.$store.dispatch('updateCurrentPage', 'history')
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
      moment: Moment,
      callee: '',
      activeCallRinging: false,
      activeCallExist: false,
      show: 'All Call',
      show2: false,
      show3: false,
      show4: false,
      show5: false
    }
  },
  methods: {
    goCall (logrecord) {
      console.log('go call page for ' + logrecord.callerDisplayNumber)
      const params = {
        callee: 'saynaci@genband.com',
        mode: false
      }
      this.$store.dispatch('call', params)
      this.$router.push('call')
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
        getDate (timestamp) {
          console.log(Moment(timestamp).calendar())
          return Moment(timestamp).calendar()
        }
      })
    }
  },
  computed: {
    getCallLogs () {
      if (this.$store.state.vux.historyFilterSelection === 'All Call') {
        console.log(this.$store.state.vux.history)
        return this.$store.state.vux.history
      } else if (this.$store.state.vux.historyFilterSelection === 'Incoming Call') {
        return this.$store.state.vux.history.filter(note => note.direction === 'incoming')
      } else if (this.$store.state.vux.historyFilterSelection === 'Outgoing Call') {
        return this.$store.state.vux.history.filter(note => note.direction === 'outgoing')
      } else if (this.$store.state.vux.historyFilterSelection === 'Missed Call') {
        return this.$store.state.vux.history.filter(note => note.direction === 'missed')
      }
    }
  }
}
</script>

