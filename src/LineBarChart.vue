<template>
  <div class="container">
    <div :style="style" class="chart"></div>
  </div>
</template>
<script type="text/babel">
  import echarts from 'echarts';
  export default {
    data: function() {
      return {
        tmpOption: {
          tooltip: {
            trigger: 'axis'
          },
          toolbox: {
            show: true,
            feature: {
              dataZoom: {
                yAxisIndex: 'none'
              },
              dataView: {readOnly: false},
              magicType: {type: ['line', 'bar']},
              restore: {},
              saveAsImage: {}
            }
          }
        }
      };
    },
    props: {
      option: {
        type: Object,
        default: function() {
          return {};
        }
      },
      width: {
        type: String,
        default: function() {
          return '100%';
        }
      },
      height: {
        type: String,
        default: function() {
          return '420px';
        }
      },
      origin: {
        type: Boolean,
        default: function() {
          return false;
        }
      }
    },
    watch: {
      options: function(val, oldVal) {
        this.drawGraph('.chart');
      }
    },
    computed: {
      options: function() {
        return this.origin ? this.option : this.getOption(this.option);
      },
      style: function() {
        return {'width': this.width, 'height': this.height};
      }
    },
    mounted() {
      this.$nextTick(function() {
        this.drawGraph('.chart');
      });
    },
    methods: {
      drawGraph: function(selector) {
        let myChart = echarts.init(this.$el.querySelector(selector));
        myChart.setOption(this.options);
      },
      getOption: function(options = {}) {
        let tmp = this.tmpOption;
        let series = this.getSeries(options.series);
        let x = this.getX(options.x);
        let y = this.getY(options.y ? options.y : []);
        let title = this.getTitle(options.title ? options.title : {});
        return {
          ...tmp,
          ...series,
          xAxis: x,
          yAxis: y,
          title: title
        };
      },
      getSeries: function(options = []) {
        let ret = [];
        let legend = [];
        for (let item of options) {
          let type = item.type && item.type === 'bar' ? 'bar' : 'line';
          let name = item.name ? item.name : '';
          let barWidth = item.barWidth ? item.barWidth : '50%';
          let label = item.label === 'show'? {
            normal: {
              show: true,
              position: 'inside'
            }
          } : {};
          let index = item.index === false ? {} : {
            markPoint: {
              data: [
                {type: 'max', name: '最大值'},
                {type: 'min', name: '最小值'}
              ]
            },
            markLine: {
              data: [
                {type: 'average', name: '平均值'}
              ]
            }
          };
          let yAxisIndex = item.yIndex ? {yAxisIndex: item.yIndex} : {};
          let color = {};
          if (item.color) {
            if (typeof item.color === 'string') {
              color = {color: [item.color]};
            } else if (Object.prototype.toString.call(item.color) === '[object Array]') {
              color = {color: item.color};
            }
          }
          legend.push(name);
          ret.push({
            name: name,
            type: type,
            barWidth: barWidth,
            data: item.data,
            label: label,
            ...index,
            ...yAxisIndex,
            ...color
          });
        }
        return {series: ret, legend: {left: 'left', data: legend}};
      },
      getTitle: function(options = {}) {
        let text = options.text ? options.text : '';
        let subtext = options.subtext ? options.subtext : '';
        let pos = options.position ? options.position : 'center';
        return {
          text: text,
          subtext: subtext,
          left: pos
        };
      },
      getX: function(options = {}) {
        let name = options.name ? options.name : '';
        let rotate = options.rotate ? options.rotate : 0;
        return {
          type: 'category',
          splitLine: {show: false},
          data: options.data,
          name: name,
          axisLabel: {
            show: true,
            rotate: rotate
          }
        };
      },
      getY: function(options = []) {
        // 空数组或空
        if (options === false || !options) {
          return {name: '', type: 'value'};
        }
        // 判断对象
        if (Object.prototype.toString.call(options) === '[object Object]') {
          let suffix = options.suffix ? options.suffix : '';
          return {
            name: options.name ? options.name : '',
            type: (options.type ? options.type : 'value'),
            axisLabel: {
              formatter: '{value}' + suffix
            }
          };
        }
        let ret = [];
        for (let item of options) {
          let name = item.name ? item.name : '';
          let suffix = item.suffix ? item.suffix : '';
          let type = item.type ? item.type : 'value';
          ret.push({
            type: type,
            axisLabel: {
              formatter: '{value}' + suffix
            },
            name: name
          });
        }
        return ret;
      }
    }
  };
</script>
<style scoped>
  .container{
    padding: 0;
    margin: 0;
  }
</style>
