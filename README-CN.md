# Echart组件
> 主要是对其配置进行封装，因此只需要传入数据即可

## 安装
```
npm install git+https://github.com/MrSuperLi/Vue-Echarts.git --save
```

## LineBarChart
> 条形和折线图

### 配置
```
// 默认 折线图
let data = {
   title:{                  // 标题与里面的属性都为可选
    text:"大标题"           //默认为空
    subtext: '小标题',      // 默认为空
    position: 'center',     // 默认center
    }, 
   x: {data: ['第一天','..','..','..']}, // x轴
   y: {name: "RMB(元)", suffix: '元'}, // y轴
   series: [                          // 具体数据 默认折线图
       {
           name:"每月充值金额",
           data:[1,2,3,4],
           color:"#3398DB"          // 指定颜色，可选
       }
   ]
};

let data = {
 y: [                            // 设定左右两边的y轴
   {name: '充值人数', suffix:"人"},  // 左边
   {name: '充值金额', suffix:"元"}   // 右边
 ],
 x: {data:[...]},
 series:[{                        // 折现图
   name: '每日充值金额',
   data: [...],
   yIndex: 1
 },{
   name: '充值人数',
   type: 'bar',                 // 指定为条形图
   data: [...],
   color:"#3398DB"
 }]
};

```

## PieChart
> 饼状图（环状图）

```
默认为环状图
let data = {
    title:{}, // 可选,与LineBarChart一样
    series:[{
      type:'ring',  // 指定为环状图
      data:[
          {value:335, name:'直接访问'},
          {value:310, name:'邮件营销'}
      ],
      name:'',
    }]
}

let data = {
    gap:5,           // 设置环状图之间的空隙,默认5
    level:4,         // 设置分多少环，默认为series.length
    series:[{        // 对于多个数据的会自动使用环状图表示
      data:[
          {value:335, name:'直接访问'},
          {value:310, name:'邮件营销'}
      ],
      name:'',
    },{
        // 设置处于环状图中的第几环，从0开始，默认为该元素在series的索引
        level:1,
        data:[
            {value:335, name:'直接访问'},
            {value:310, name:'邮件营销'}
        ],
        name:'',
      }]
}
```

### 小技巧
> 如果想在series只有一个元素，并且向设置处于任意位置和大小的环状图？

```
let data = {
    gap:0,           // 设置空隙
    level:4,         // 告诉它要分4环
    series:[{
        level:2,    // 并且我要在第3环的位置上
        data:[
            {value:335, name:'直接访问'},
            {value:310, name:'邮件营销'}
        ],
        name:'',
      }]
}
// 为了可以简化，提供一个type属性，设置为ring即可使用默认样式的环状图
```

## 高度和宽度
> 只要设定height和width属性即可

> height默认为420px,width默认为100%

## 原始配置
> 如果需要使用ECharts的原始的配置方式，需要使用origin属性


## 使用
```
// PieChart与此相同
<LineBarChart :option="data" width='宽度' height='高度'></LineBarChart>
// 原始配置
<LineBarChart :option="data" width='宽度' height='高度' :origin='true'></LineBarChart>
```
