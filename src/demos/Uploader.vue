<template>
  <div>
    <divider>No package components, the introduction of the use of style</divider>
    <div class="weui-uploader" style="padding:15px;">
      <div class="weui-uploader__hd">
          <p class="weui-uploader__title">upload picture</p>
          <div class="weui-uploader__info">0/2</div>
      </div>
      <div class="weui-uploader__bd">
          <ul class="weui-uploader__files" id="uploaderFiles">
              <li class="weui-uploader__file" style="background-image:url(https://static.vux.li/uploader_bg.png)"></li>
              <li class="weui-uploader__file" style="background-image:url(https://static.vux.li/uploader_bg.png)"></li>
              <li class="weui-uploader__file" style="background-image:url(https://static.vux.li/uploader_bg.png)"></li>
              <li class="weui-uploader__file weui-uploader__file_status" style="background-image:url(https://static.vux.li/uploader_bg.png)">
                  <div class="weui-uploader__file-content">
                      <i class="weui-icon-warn"></i>
                  </div>
              </li>
              <li class="weui-uploader__file weui-uploader__file_status" style="background-image:url(https://static.vux.li/uploader_bg.png)">
                  <div class="weui-uploader__file-content">50%</div>
              </li>
          </ul>
          <div class="weui-uploader__input-box">
              <input id="uploaderInput" class="weui-uploader__input" type="file" accept="image/*" multiple />
          </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Divider } from 'vux'

export default {
  components: {
    Divider
  }
}
</script>

<style lang="less">
@import '~vux/src/styles/weui/widget/weui-uploader/index.less';
</style>
