<template>
  <div id="report">
    <!-- 面包屑区域 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>数据统计</el-breadcrumb-item>
      <el-breadcrumb-item>数据报表</el-breadcrumb-item>
    </el-breadcrumb>

    <!-- 卡片视图区域 -->
    <el-card>
      <!-- 第二步  为ECharts准备一个具备大小（宽高）的Dom -->
      <div id="main" style="width: 800px;height:500px;"></div>
    </el-card>
  </div>
</template>

<script>
  //引入深拷贝
  import _ from 'lodash'

  //第一步  导入echarts
  import echarts from 'echarts'

  export default {
    data() {
      return {
        //echarts需要合并的数据
        options: {
          title: {
            text: '用户来源'
          },
          tooltip: {
            trigger: 'axis',
            axisPointer: {
              type: 'cross',
              label: {
                backgroundColor: '#E9EEF3'
              }
            }
          },
          grid: {
            left: '3%',
            right: '4%',
            bottom: '3%',
            containLabel: true
          },
          xAxis: [{
            boundaryGap: false
          }],
          yAxis: [{
            type: 'value'
          }]
        }
      }
    },
    created() {},
    async mounted() {
      // 第三步  基于准备好的dom，初始化echarts实例
      var myChart = echarts.init(document.getElementById('main'));

      //第四步  指定图表的配置项和数据(请求数据)
      const {
        data: res
      } = await this.$http.get('reports/type/1')

      if (res.meta.status !== 200)
        return this.$message.error('服务器繁忙，数据请求失败')

      const result = _.merge(res.data, this.options)


      //第五步 使用刚指定的配置项和数据显示图表。
      myChart.setOption(result);
    },
    methods: {

    }
  }

</script>

<style lang="less" scoped>

</style>
