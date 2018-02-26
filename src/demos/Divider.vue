<template>
<div>
  <divider>{{ $t('I have bottom line') }}</divider>
</div>
</template>

<i18n>
I have bottom line:
  zh-CN: I have a bottom line
</i18n>

<script>
import { Divider } from 'vux'

export default {
  components: {
    Divider
  }
}
</script>
