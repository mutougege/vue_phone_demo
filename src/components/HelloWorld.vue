<template>
  <div>
    <van-button type="primary">click</van-button>
    <van-button type="primary">主要按钮</van-button>
    <van-cell title="单元格" value="内容" />
  </div>
</template>

<script>
import { Button, Cell } from 'vant'
export default {
  name: 'HelloWorld',
  components: {
    [Button.name]: Button,
    [Cell.name]: Cell
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<style scoped>
</style>
