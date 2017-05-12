# a3-software-design
INFO 474 a3-software-design

## Circle Packing Chart

Circle packing charts are useful for desplaying hierarchical sets of data. This code can be used to make a simple circle pack visualization using a dataset of your choice.

<br />


### Data Prep
You will need to prep your data before you can use it to build your chart. After reading your data into your js file, you will need to nest your data using the `d3.nest()` function and specify the key as the variable you want the data to be grouped by. For example:
```
    var nestedData = d3.nest()
        .key(function(d) {
            return d.region;
        })
        .entries(data);
```
This will group the data by the variable you chose, in this case region.

Next you will neet to define a hierarchy for your data using the `d3.hierarchy()` function and store it in a variable to pass to the chart function, and define which variable you want to visualize using the `.sum()` function. For example:
```
    var root = d3.hierarchy({
        values: nestedData
    }, function(d) {
        return d.values;
    }).sort(function(a, b) {
        return b.value - a.value;
    }).sum(function(d) {
        return +d['fertility_rate'];
    });
```

Now your data should be ready to vizualize! 


<br />


### Creating the chart
1. You will need to import `CirclePack.js` into your project.


2. Call `CirclePack()` to get the function for building the charts. For example:
```
    var myChart = CirclePack();
```


3. You can call methods on CirclePack() to customize the chart. For example:
```
    myChart.height(750);
    myChart.width(750);
```
All available functions for customizing the chart are listed below.


4. Select the div where you want to place the chart and bind the data to it, then call the chart function.
```
    var chartWrapper = d3.select('#vis').datum(data).call(myChart);
```

5. To change the data you want to display, bind the new data to you chart wrapper element and call the chart function on it just as before.
```
    chartWrapper.datum([newDataSet]).call(myChart);
```
<br />


### CirclePack functions
The following functions can be used to change the appearance of your circle pack.

`CirclePack()`
> Constructs a Circle Packing Chart and returns itself.

`CirclePack.width(value)`
> If no argument is given, returns the current width of the chart.
If a value is given, it sets the chart width to that value and returns the chart.
The default value is `800`.

`CirclePack.height(value)`
> If no argument is given, returns the current height of the chart.
If a value is given, it sets the chart height to that value and returns the chart.
The default value is `800`.

`CirclePack.padding(value)`
> If no argument is given, returns the current padding between the circles in the chart.
If a value is given, it sets the padding between the circles in the chart to that value and returns the chart.
The default value is `0`.

`CirclePack.circleScale(value)`
> If no argument is given, returns the current factor for sizing the circles in the chart.
If a value is given, it sets the factor for sizing the circles to that value and returns the chart. 
The default value is `1`.

`CirclePack.colors(value)`
> If no argument is given, returns the color scale of the chart.
If a value is given, it sets the color scale to that value and returns the chart.
The default value is `d3.schemeCategory20b`. Other color scheme options include:
```
d3.category10
d3.category20
d3.category20c
```

`CirclePack.title(value)`
> If no argument is given, returns the title of the chart.
If a value is given, it sets the title of the chart to that value and returns the chart.
The default value is `My CirclePack`.

`CirclePack.nameVar(value)`
> If no argument is given, returns the name of the variable used to assign names to the circles.
If a value is given, it sets the text on the circles to the values of the variable name given. For example, if you wanted your circles to be labeled with the country value from each piece of data, you would pass 'country'.