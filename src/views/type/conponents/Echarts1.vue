<template>
  <div>
    <div style="height: 600px" ref="main"></div>
  </div>
</template>

<script>
import * as echarts from 'echarts'
import { getTypeList } from '@/api/list'
export default {
  data () {
    return {
      list: [], // 描述文字
      content: [] // 数据
    }
  },
  created () {
    this.loadgetTypeList()
  },
  mounted () {
    const myChart = echarts.init(this.$refs.main)
    setTimeout(() => {
      const option = {
        label: {
          show: true, // 是否展示
          position: 'top', // 在顶端
          textStyle: {
            fontSize: '12',
            fontFamily: '微软雅黑'
          }
        },
        title: {
          text: 'Renova 纸巾品类销量表',
          subtext: '全部销量表',
          left: 'center'
        },
        xAxis: {
          type: 'category',
          data: this.list,
          axisLabel: {
            interval: 0,
            rotate: 30
          }
        },
        yAxis: {
          type: 'value'
        },
        series: [
          {
            name: '',
            data: this.content,
            type: 'bar',
            showBackground: true,
            backgroundStyle: {
              color: 'rgba(180, 180, 180, 0.2)'
            },
            // 最大值最小值
            markPoint: {
              data: [
                {
                  type: 'max',
                  name: '最大值'
                },
                {
                  type: 'min',
                  name: '最小值'
                }
              ]
            },
            // 平均值
            markLine: {
              data: [
                {
                  type: 'average',
                  name: '平均值'
                }
              ]
            }
          }
        ]
      }
      myChart.setOption(option)
    }, 1000)
  },
  methods: {
    async loadgetTypeList () {
      const { data } = await getTypeList()
      data.forEach(item => {
        this.list.push(item.list)
        this.content.push(item.num)
      })
    }
  }
}
</script>

<style lang='less' scoped>
</style>
