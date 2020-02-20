<template>
  <div id="app" ref='chinaMap'>
  </div>
</template>

<script>
import echarts from 'echarts'
import $ from 'jquery'
import '../node_modules/echarts/map/js/china.js'
export default {
  data(){
     return {    
      }
  },
  methods:{
    randomValue(){
      return Math.random()
    },
    getChinaMap(areaList){
      let eChart = echarts.init(this.$refs.chinaMap)
      let option = {
            tooltip: {
                    formatter:function(params,ticket, callback){
                        // console.log(params)
                        return params.seriesName+'<br />'+params.name+'：'+params.value
                    }//数据格式化
                },
            visualMap: {
                min: 0,
                max: 1500,
                left: 'left',
                top: 'bottom',
                text: ['高','低'],//取值范围的文字
                inRange: {
                    color: ['#ffffff', '#ff0000']//取值范围的颜色
                },
                show:true//图注
            },
            geo: {
                map: 'china',
                roam: false,//不开启缩放和平移
                zoom:1.23,//视角缩放比例
                label: {
                    normal: {
                        show: true,
                        fontSize:'10',
                        color: 'rgba(0,0,0,0.7)'
                    }
                },
                itemStyle: {
                    normal:{
                        borderColor: 'rgba(0, 0, 0, 0.2)'
                    },
                    emphasis:{
                        areaColor: '#F3B329',//鼠标选择区域颜色
                        shadowOffsetX: 0,
                        shadowOffsetY: 0,
                        shadowBlur: 20,
                        borderWidth: 0,
                        shadowColor: 'rgba(0, 0, 0, 0.5)'
                    }
                }
            },
            series : [
                {
                    name: '信息量',
                    type: 'map',
                    geoIndex: 0,
                    data:areaList.list
                }
            ]
        };
        eChart.setOption(option)
    },
    getVirus(){
      $.ajax({
        url:'https://interface.sina.cn/news/wap/fymap2020_data.d.json',
        type:'get',
        dataType:'jsonp',
        jsonpCallback: "handleCallback"
      }).then(res=>{
        console.error(res)
        this.getChinaMap(res.data)
      })
    }
  },
  mounted(){
    this.getVirus()
    this.getChinaMap()
  }
}
</script>

<style>
        #app{
            width: 600px;
            height: 450px;
            margin: 150px auto;
            border: 1px solid #ddd;
        }
</style>
