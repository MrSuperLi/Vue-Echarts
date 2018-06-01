# Echart components
> The main thing is to encapsulate its configuration so only incoming data is needed

## Installation
```
Npm install git+https://github.com/MrSuperLi/Vue-Echarts.git --save
```

## LineBarChart
> Bar and Line Charts

### Configuration
```
// default line chart
Let data = {
   Title:{ // The title and the properties inside are optional
    Text:"Headline" //Default is null
    Subtext: 'subheader', // defaults to null
    Position: 'center', // default center
    },
   x: {data: ['first day','..','..','..']}, // x axis
   y: {name: "RMB(yuan)", suffix: 'meta'}, // y axis
   Series: [ // specific data default line chart
       {
           Name:"Monthly recharge amount",
           Data:[1,2,3,4],
           Color:"#3398DB" // Specify color, optional
       }
   ]
};

Let data = {
 y: [ // Set the left and right y-axis
   {name: 'recharge number', suffix: "person"}, // left
   {name: 'recharge amount', suffix: "meta"} // right
 ],
 x: {data:[...]},
 Series:[{ // Discounted Chart
   Name: 'Daily recharge amount',
   Data: [...],
   yIndex: 1
 },{
   Name: 'recharge number'
   Type: 'bar', // Specified as a bar chart
   Data: [...],
   Color:"#3398DB"
 }]
};

```

## PieChart
> Pie Chart (Circular)

```
The default is a circular map
Let data = {
    Title:{}, // Optional, same as LineBarChart
    Series:[{
      Type:'ring', // Specified as a circle
      Data:[
          {value:335, name:'Direct access'},
          {value:310, name:'Mail Marketing'}
      ],
      Name:'',
    }]
}

Let data = {
    Gap:5, // Set the gap between the rings, default 5
    Level:4, // Set the number of points to divide. The default is series.length
    Series:[{ // for many data will automatically use the ring diagram representation
      Data:[
          {value:335, name:'Direct access'},
          {value:310, name:'Mail Marketing'}
      ],
      Name:'',
    },{
        // Set the number of rings in the circle graph, starting from 0, the default is the index of the element in the series
        Level:1,
        Data:[
            {value:335, name:'Direct access'},
            {value:310, name:'Mail Marketing'}
        ],
        Name:'',
      }]
}
```

### Tips
> If you want to have only one element in the series, and to set the ring in any position and size?

```
Let data = {
    Gap:0, // set the gap
    Level:4, // Tell it to divide into 4 rings
    Series:[{
        Level:2, // and I want to be in the third ring position
        Data:[
            {value:335, name:'Direct access'},
            {value:310, name:'Mail Marketing'}
        ],
        Name:'',
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