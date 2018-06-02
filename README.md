# Echart components
> The main thing is to encapsulate its configuration so only incoming data is needed

## Installation
```
npm install git+https://github.com/MrSuperLi/Vue-Echarts.git --save
```

## LineBarChart
> Bar and Line Charts

### Configuration
```
// default line chart
let data = {
   title:{ // The title and the properties inside are optional
    text:"Headline" //Default is null
    subtext: 'subheader', // defaults to null
    position: 'center', // default center
    },
   x: {data: ['first day','..','..','..']}, // x axis
   y: {name: "RMB(yuan)", suffix: 'meta'}, // y axis
   series: [ // specific data default line chart
       {
           name:"Monthly recharge amount",
           data:[1,2,3,4],
           color:"#3398DB" // Specify color, optional
       }
   ]
};

let data = {
 y: [ // Set the left and right y-axis
   {name: 'recharge number', suffix: "person"}, // left
   {name: 'recharge amount', suffix: "meta"} // right
 ],
 x: {data:[...]},
 series:[{ // Discounted Chart
   name: 'Daily recharge amount',
   sata: [...],
   yIndex: 1
 },{
   name: 'recharge number'
   type: 'bar', // Specified as a bar chart
   data: [...],
   color:"#3398DB"
 }]
};

```

## PieChart
> Pie Chart (Circular)

```
The default is a circular map
let data = {
    title:{}, // Optional, same as LineBarChart
    series:[{
      type:'ring', // Specified as a circle
      sata:[
          {value:335, name:'Direct access'},
          {value:310, name:'Mail Marketing'}
      ],
      name:'',
    }]
}

let data = {
    gap:5, // Set the gap between the rings, default 5
    level:4, // Set the number of points to divide. The default is series.length
    series:[{ // for many data will automatically use the ring diagram representation
      data:[
          {value:335, name:'Direct access'},
          {value:310, name:'Mail Marketing'}
      ],
      name:'',
    },{
        // Set the number of rings in the circle graph, starting from 0, the default is the index of the element in the series
        level:1,
        data:[
            {value:335, name:'Direct access'},
            {value:310, name:'Mail Marketing'}
        ],
        name:'',
      }]
}
```

### Tips
> If you want to have only one element in the series, and to set the ring in any position and size?

```
let data = {
    gap:0, // set the gap
    level:4, // Tell it to divide into 4 rings
    series:[{
        level:2, // and I want to be in the third ring position
        data:[
            {value:335, name:'Direct access'},
            {value:310, name:'Mail Marketing'}
        ],
        name:'',
      }]
}
// For simplicity, provide a type attribute, set to ring to use the default style ring
```

## Height and width
> Just set the height and width properties

> height defaults to 420px, width defaults to 100%

## original configuration
> If you need to use the original configuration of ECharts, you need to use the origin attribute


## Use
```
// Same PieChart as this
<LineBarChart :option="data" width='width' height='height'></LineBarChart>
// original configuration
<LineBarChart :option="data" width='width' height='height' : origin='true'></LineBarChart>
```
