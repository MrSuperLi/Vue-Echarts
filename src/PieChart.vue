<template>
    <div class="container">
        <div :style="style" class="chart"></div>
    </div>
</template>
<script type="text/babel">
    import echarts from 'echarts';
    export default{
        data: function(){
            return {
                tmpOption:{
                    tooltip: {
                        trigger: 'item',
                        formatter: '{a} <br/>{b} : {c} ({d}%)'
                    },
                    toolbox: {
                        show: true,
                        feature: {
                            dataZoom: {
                                yAxisIndex: 'none'
                            },
                            dataView: {readOnly: false},
                            restore: {},
                            saveAsImage: {}
                        }
                    }
                }
            };
        },
        props:{
            option:{
                type: Object,
                default: function(){
                    return {
                        series:[],
                        title:''
                    };
                }
            },
            width: {
                type:String,
                default:function(){
                    return '100%';
                }
            },
            height: {
                type:String,
                default:function(){
                    return '420px';
                }
            },
            origin: {
                type:Boolean,
                default: function(){
                    return false;
                }
            }
        },
        computed:{
            style: function(){
                return {'width':this.width,'height':this.height};
            },
            options : function(){
                return this.origin ? this.option : this.getOption(this.option);
            }
        },
        mounted: function(){
            this.$nextTick(function() {
                this.drawGraph('.chart');
            })
        },
        watch: {
            options:function(val, oldVal){
                this.drawGraph('.chart');
            }
        },
        methods: {
            drawGraph: function(selector){
                var myChart = echarts.init(this.$el.querySelector(selector));
                myChart.setOption(this.options);
            },
            getTitle: function(title) {
                let option = title;
                if(typeof option == 'string'){
                    return this.title({text:option});
                }
                return this.title(option);
            },
            title: function(options){
                let text = options.text ? options.text : '';
                let subtext = options.subtext ? options.subtext : '';
                let pos = options.position ? options.position : 'center';
                return {
                    text: text,
                    subtext: subtext,
                    left: pos
                };
            },
            // n 第几个 sum共几个, num分割的数
            getPercent: function(n,sum,num, gap=5){
                let width = (num - gap*(sum-1))/sum;
                let start = n*(width + gap);
                let end = start + width
                return [start, end];
            },
            getSeries: function(options=[], level=4, gap=5){
                let ret = [];
                let legend = [];
                for(let index in options){
                    let pie = options[index];
                    for(let item of pie.data){
                        legend.push(item.name);
                    }
                    let name = pie.name?pie.name:'';
                    if( pie.type == 'ring' ){
                        ret.push({
                            type:'pie',
                            data:pie.data,
                            name:name,
                            radius: ['45%', '70%'],
                        });
                    }else{
                        let currentLevel = pie.level?pie.level:index;
                        let percent = this.getPercent(currentLevel, level, 70, gap);
                        percent = [ percent[0] + '%', percent[1] + '%' ];
                        ret.push({
                            type:'pie',
                            data:pie.data,
                            name:name,
                            radius: percent,
                            label: {
                                normal: {
                                    position: 'inner'
                                }
                            },
                            labelLine: {
                                normal: {
                                    show: false
                                }
                            },
                        });
                    }
                }
                return {series:ret,legend:{orient: 'vertical', x: 'left',data:legend}};
            },
            getOption: function(option){
                const tmp = this.tmpOption;
                const level = option.level?option.level:option.series.length;
                const gap = option.gap?option.gap:5;
                const series = this.getSeries(option.series, level, gap);
                const title = this.getTitle(option.title?option.title:{});
                return {
                    ...tmp,
                    ...series,
                    title:title
                };
            }
        }
    }
</script>
