<template>
  <div ref="wrapper">
    <div ref="box" class="bigScreen">
      <img class="topImg" src="@/assets/bigScreen/top.png" alt="">
      <div class="bigScreen-content">
        <div class="main-table">
          <Table></Table>
          <img class="bl-img" src="@/assets/bigScreen/btl.png" alt="">
          <img class="br-img" src="@/assets/bigScreen/btr.png" alt="">
        </div>
        <div class="bigScreen-bottom">
          <a-row :gutter="30">
            <a-col :span="15">
              <Summary></Summary>
            </a-col>
            <a-col :span="9">
              <Chart></Chart>
            </a-col>
          </a-row>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import Table from './Table'
import Summary from '@views/bigScreen/Summary'
import Chart from '@views/bigScreen/Chart'

export default {
  components: {
    Table,
    Chart,
    Summary
  },
  mounted() {
    this.$nextTick(() => {
      this.updateStyle();
    });
    setTimeout(() => {
      this.updateStyle();
    }, 500);
    window.addEventListener('resize', this.updateStyle)
  },
  beforeCreate() {
    document.body.classList.add('screen-bg')
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.updateStyle)
    document.body.classList.remove('screen-bg')
  },
  methods: {
    updateStyle() {
      if (!this.$refs.wrapper) return
      const { width } = this.$refs.wrapper.getBoundingClientRect()
      this.$refs.box.style.zoom = width / 1920;
    }
  }
}
</script>


<style lang="less" scoped src="./index.less"></style>
<style lang="less">
.screen-bg {
  background-color: rgba(4, 7, 47, 1);
  //overflow: hidden;
  &::-webkit-scrollbar {
    width: 0;
    opacity: 0;
  }
  &::-webkit-scrollbar-thumb {
    background-color: transparent;
    box-shadow: none !important;
    opacity: 0;
  }
}
</style>