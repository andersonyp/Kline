<template>
  <div class="content-box">
    <common-header :tittle="tittle" :showback="false"></common-header>
    <First></First>
    <kecharts></kecharts>
    <div class="box">
      <div></div>
    </div>
  </div>
</template>

<script>
import { mapMutations, mapGetters, mapState } from 'vuex'
import commonHeader from 'common/common-header'
import First from 'common/First'
import kecharts from 'common/Kecharts'
export default {
  data() {
    return {
      tittle: '我的故事',
      num: 0
    }
  },
  created() {},
  methods: {
    ...mapMutations({
      setNum: 'SET_NUM'
    }),
    todetail() {
      this.$router.togo('/Home/Detail')
    }
  },
  components: {
    commonHeader,
    First,
    kecharts
  },
  computed: {
    ...mapGetters(['number']),
    ...mapState({
      number: state => state.home.number
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
@import '~styles/index.less';
@import '~styles/variable.less';
.page-content {
  .mb(98);
}

.box {
  width: 300px;
  height: 300px;
  position: relative;

  div {
    width: 4px;
    height: 4px;
    background: rgba(16, 142, 233, 1);
    border: 7px solid rgb(146, 199, 236);
    border-radius: 50%;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
}
</style>
