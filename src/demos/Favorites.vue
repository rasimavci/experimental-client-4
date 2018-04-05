<template>
  <div>

    <div class='modal-container' display='none'>
      <br>
      </br>
      <label>Your favorites tab is empty. To add favorites, click on the Contacts tab and add a contact to your favorites tab.</label>
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
import { Confirm, Group, XSwitch, XButton, TransferDomDirective as TransferDom } from 'vux'
export default {
  created: function () {
    this.$store.dispatch('updateCurrentPage', 'favorites')
    this.$store.dispatch('updateShowPlacement', 'right')
  },
  directives: {
    TransferDom
  },
  components: {
    Confirm,
    Group,
    XSwitch,
    XButton
  },
  data () {
    return {
      callee: '',
      activeCallRinging: false,
      activeCallExist: false,
      show: false,
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
        }
      })
    }
  }
}
</script>

