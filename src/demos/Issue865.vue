<template>
  <div>
    <group labelWidth="4em" label-margin-right="1em">
      <x-input title="password" type="password" placeholder="Please enter 6 ~ 20 password" required :min="6" :max="20" v-model="password" ref="password"></x-input>
      <x-input title="confirm password" :type="passwordType" placeholder="Please confirm the input" v-model="confirmPassword" required :min="6" :max="20" :equal-with="password" ref="confirmPassword" :show-clear="false">
        <span slot="right" @click="passwordType=passwordType === 'password' ? 'text' : 'password'">显示{{passwordType === 'password' ? '明文' : '密文'}}</span>
      </x-input>
    </group>
    <div style="padding:15px;">
      <x-button @click.native="getValid" type="primary">GET valid</x-button>
    </div>
  </div>
</template>

<script>
import { Group, XInput, XButton } from 'vux'

export default {
  components: {
    Group,
    XInput,
    XButton
  },
  data () {
    return {
      password: '',
      confirmPassword: '',
      passwordType: 'password'
    }
  },
  methods: {
    getValid () {
      console.log(this.$refs.confirmPassword.valid)
    }
  }
}
</script>
