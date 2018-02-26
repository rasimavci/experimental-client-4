<template>
  <div class="donate">
    <img src="https://i.loli.net/2017/10/26/59f15b02a1739.jpg" class="donate-img">
    <p class="desc">Thanks for the donation, but donating does not give you any privilege on the project.
    <br>
    The purchase of [World and Me] products is also supporting the development of the project.
    <br>
(Lynx search livingearth)
    </p>
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

<style scoped>
.donate {
  text-align: center;
  background-color: #fff;
}
.donate-img {
  max-width: 100%;
}
.desc {
  color: #999;
  font-size: 12px;
}
</style>
