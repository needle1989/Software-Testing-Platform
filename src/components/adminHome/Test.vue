<template>
<div>
    <el-button type="primary" @click="submitOption()" style="width: 100px"
            >SUBMIT</el-button>
  <div id="main" style="width:900px ;height:300px;"></div>
</div>
</template>

<script>
  import * as echarts from 'echarts';
  export default {
    name: 'index',
    data() {
      return {}
    },
    // mounted: function() {
    //   this.$nextTick(function() {
    //     this.getPie()
    //   })
    // },
    methods: {
      submitOption(){
          this.getPie()
      },
      getPie() {
        // 绘制图表
        var myChart = echarts.init(document.getElementById('main'))
        // 指定图表的配置项和数据
        var option = {
          //标题
          title: {
            text: 'Testing Report',
            x: 'left' ,              //标题位置
            // textStyle: { //标题内容的样式
            //   color: '#000',
            //   fontStyle: 'normal',
            //   fontWeight: 100,
            //   fontSize: 16 //主题文字字体大小，默认为18px
            // },
          },
          // stillShowZeroSum: true,
          //鼠标划过时饼状图上显示的数据
          tooltip: {
            trigger: 'item',
            formatter: '{a}<br/>{b}:{c} ({d}%)'
          },
          //图例
          legend: {//图例  标注各种颜色代表的模块
            // orient: 'vertical',//图例的显示方式  默认横向显示
            bottom: 0,//控制图例出现的距离  默认左上角
            left: 'center',//控制图例的位置
            // itemWidth: 16,//图例颜色块的宽度和高度
            // itemHeight: 12,
            textStyle: {//图例中文字的样式
              color: '#000',
              fontSize: 16
            },
            data: ['SUCCESS', 'FAILED', 'UNTESTED']//图例上显示的饼图各模块上的名字
          },
          //饼图中各模块的颜色
          color: ['#32dadd', '#b6a2de', '#5ab1ef'],
          // 饼图数据
          series: {
            // name: 'bug分布',
            type: 'pie',             //echarts图的类型   pie代表饼图
            radius: '70%',           //饼图中饼状部分的大小所占整个父元素的百分比
            center: ['50%', '50%'],  //整个饼图在整个父元素中的位置
            // data:''               //饼图数据
            data: [                  //每个模块的名字和值
              { name: 'SUCCESS', value: 40 },
              { name: 'FAILED', value: 5},
              { name: 'UNTESTED', value: 0 }
            ],
            itemStyle: {
              normal: {
                label: {
                  show: true,//饼图上是否出现标注文字 标注各模块代表什么  默认是true
                  // position: 'inner',//控制饼图上标注文字相对于饼图的位置  默认位置在饼图外
                },
                labelLine: {
                  show: true//官网demo里外部标注上的小细线的显示隐藏    默认显示
                }
              }
            },
          }

        }
        // 使用刚指定的配置项和数据显示图表。
        myChart.setOption(option)
      }
    }

  }

</script>

<style scoped>

</style>