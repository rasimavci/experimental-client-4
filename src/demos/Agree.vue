<template>
  <div>
    <agree>Read and agree<a href="javascript:void(0);">《Related Terms》</a></agree>
    <agree v-model="valueTrue">Read and agree<a href="javascript:void(0);">《Related Terms》</a></agree>
  </div>
</template>

<script>
import { Agree } from 'vux'

export default {
  components: {
    Agree
  },
  data () {
    return {
      valueTrue: true
    }
  }
}
</script>
