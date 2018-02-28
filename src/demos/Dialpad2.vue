<template>
  <div>

     <div class='keypad'>
        <div class='keypad-container' v-show='!activeCallExist  && !activeCallRinging'>
          <div>
            <button class='button' @click='showActiveCallRinging()'>
              <div class='keypad-button-number'>1</div>
              <div class='keypad-button-text'></div>
            </button>
            <button class='button' @click='press(2)'>
              <div class='keypad-button-number'>2</div>
              <div class='keypad-button-text'>
                ABC
              </div>
            </button>
            <button class='button' @click='press(3)'>
              <div class='keypad-button-number'>3</div>
              <div class='keypad-button-text'>
                DEF
              </div>
            </button>
          </div>
          <div>
            <button @click='press(4)'>
              <div class='keypad-button-number'>4</div>
              <div class='keypad-button-text'>
                GHI
              </div>
            </button>
            <button @click='press(5)'>
              <div class='keypad-button-number'>5</div>
              <div class='keypad-button-text'>
                JKL
              </div>
            </button>
            <button class='button' @click='press(6)'>
              <div class='keypad-button-number'>6</div>
              <div class='keypad-button-text'>
                MNO
              </div>
            </button>
          </div>
          <div>
            <button class='button' @click='press(7)'>
              <div class='keypad-button-number'>7</div>
              <div class='keypad-button-text'>
                PQRS
              </div>
            </button>
            <button class='button' @click='press(8)'>
              <div class='keypad-button-number'>8</div>
              <div class='keypad-button-text'>
                TUV
              </div>
            </button>
            <button class='button' @click='press(9)'>
              <div class='keypad-button-number'>9</div>
              <div class='keypad-button-text'>
                WXYZ
              </div>
            </button>
          </div>
          <div>
            <button class='button' @click="press('*')">
              *
            </button>
            <button class='button' @click='press(0)'>
              0
            </button>
            <button class='button' @click="press('#')">
              #
            </button>
            <button class='button' @click='sendMessage()'>
              Send
            </button>
            <button class='button' @click='sendMessage2()'>
              Send2
            </button>
            <button class='button' @click='getPresence()'>
              Pre1
            </button>
            <button class='button' @click='presenceSubscribe()'>
              Subs
            </button>
          </div>
        </div>
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


<style scoped>
.btn-group button {
    background-color: #4CAF50; /* Green background */
    border: 1px solid green; /* Green border */
    color: white; /* White text */
    padding: 10px 24px; /* Some padding */
    cursor: pointer; /* Pointer/hand icon */
    float: left; /* Float the buttons side by side */
}

.h3 {
  color: #1abc9c;
}

.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  transition: opacity 0.3s ease;
}

.modal-container {
  width: 300px;
  margin: 40px auto 0;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-body {
  margin: 0;
}

.keypad {
  text-align: center;
  padding: 0;
  margin: 0;
  overflow: hidden;
}

.input-number {
  width: 100%;
  margin: 0 auto;
  /* margin-top: 5px; */
  border: 0;
  background-color: #eceff0;
  height: 50px;
  font-size: 1.5em;
  text-align: center;
}

.keypad-container {
  overflow: hidden;
}

.keypad-container div {
  width: 100%;
}

.keypad-container div button {
  width: 33.33%;
  padding: 0;
  margin: 0;
  display: block;
  float: left;
  height: 70px;
  border: 1px solid gray;
  background: white;
  color: rgba(0, 0, 0, 0.8);
  border-radius: 5px;
  font-size: 1.5em;
  margin: 5px 0;
}


.keypad-button-number {
  font-size: 28px;
  display: block;
  margin: 18px auto 5px;
  color: rgba(0, 0, 0, 0.8);
  line-height: 22px;
  text-align: center;
}

.keypad-button-text {
  font-size: 12px;
  display: block;
  margin: 0 auto;
  color: rgba(0, 0, 0, 0.5);
  line-height: 12px;
  text-align: center;
  height: 12px;
}

.button {
  background-color: #4caf50;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  height: 50px;
  width: 10px;
}

.dialpad-button {
  width: 84px;
  height: 72px;
  border-bottom: 1px solid;
  float: left;
}

.keypad-menu {
  text-align: center;
}

.keypad-menu button {
  width: 64px;
  height: 64px;
  margin: 10px;
  border-radius: 32px;
  background-color: gray;
  padding: 0;
}

.keypad-menu button:hover {
  background-color: #1E90FF;
}

.keypad-menu myhover {
  background-color: #1abc9c;
}

.dialpad-button:hover {
  background-color: blue;
}

.dialpad-button .number {
  font-size: 28px;
  display: block;
  margin: 18px auto 5px;
  color: black;
  line-height: 22px;
  text-align: center;
}

.dialpad-button .letters {
  font-size: 12px;
  display: block;
  margin: 0 auto;
  color: darkgray;
  line-height: 12px;
  text-align: center;
}

.hide-keypad {
  max-height: 0px;
  transition: max-height 0.25s ease-in;
}

.backspace-button {
  background: white;
  border: 0px;
}
.keypad-container button{
  border: none !important;
  border-radius: 0px !important;
  color: #1E90FF !important;
}

.keypad-container button div{
  color: #1E90FF !important;
}

.keypad-container button:hover {
  background-color: #1abc9c;
}
.keypad-container button:hover div {
  /* background-color: #1abc9c; */
  color: white !important;
}

</style>
