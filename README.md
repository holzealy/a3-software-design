function CirclePack() {
    // default properties
    var width = 800,
        height = 800,
        margin = {
                top: 40,
                right: 10,
                bottom: 10,
                left: 10
        },
        padding = 0,
        circleScale = 1,
        title = 'My CirclePack',
        colorScale = d3.scaleOrdinal(d3.schemeCategory20b)
        nameVar = null;

    // generate chart
    function myChart(selection) {
        // pack function that will compute layout given the data structure
        var pack = d3.pack()
            //.sort(d3.descending)
            .size([width - margin.left, height - margin.top])
            .padding(padding);

        // iterate through selections
        selection.each(function(root) {
            var svg = d3.select(this)
                .selectAll('svg')
                .data([root]);

            var svgEnter = svg.enter()
                .append('svg')
                .attr('width', width)
                .attr('height', height);

            svgEnter.append('text')
                .attr('x', width / 2)
                .attr('y', margin.top)
                .style('text-anchor', 'middle')
                .text(title);

            svgEnter.append('g')
                .attr('transform', 'translate(' + margin.left + ',' + margin.top + ')')
                .attr('class', 'chart');
            
            // build circle pack data structure
            pack(root);

            var node = d3.select(this).select('.chart')
                .selectAll('node')
                .data(root.leaves())
                .enter()
                .append('g')
                .attr('class', 'node')
                .attr('transform', function(d) { return 'translate(' + d.x + ',' + d.y + ')'; });

            // var circles = d3.select(this).select('.chart')
            //     .selectAll('circle')
            //     .data(root.leaves());

            //node.enter().append('circle')
            node.append('circle')
                //.merge(node)
                //.attr('class', 'node')
                //.transition().duration(2000)
                .attr('r', function(d) { return d.r * circleScale; })
                //.attr('cx', function(d) { return d.x; })
                //.attr('cy', function(d) { return d.y; })
                .attr('fill', function(d) {
                    return colorScale(d.parent.data.key);
                })
                .attr('opacity', 0.5);

            node.append('text')
                .text(function(d) {
                    return d.data[nameVar];
                })
                .attr('x', -15)
                .attr('font-size', 10);
                

            node.exit().remove();

            // have user pass in array of values to group by

            // var legend = svgEnter.append('g')
            //     .attr('id', 'legend')
            //     .attr('transform', 'translate(' + margin.left + ',' + (margin.top + 20) + ')');

            // var labels = legend.selectAll('text')
            //     .data(root.leaves());
            //     console.log(labels)
                
            // labels.merge(labels)
            //     .append('text')
            //     .text(function(d) {
            //         return d.parent.data.key;
            //     });
                

            

        });
    }


    // function to get or set width property
    myChart.width = function(value) {
        if (!arguments.length) return width;
        width = value;
        return myChart;
    }

    // function to get or set height property
    myChart.height = function(value) {
        if (!arguments.length) return height;
        height = value;
        return myChart;
    }

    // function to get or set padding property
    myChart.padding = function(value) {
        if (!arguments.length) return padding;
        padding = value;
        return myChart;
    }

    // function to get or set circleScale property
    myChart.circleScale = function(value) {
        if (!arguments.length) return circleScale;
        circleScale = value;
        return myChart;
    }

    // function to get or set color property
    myChart.colorScale = function(value) {
        if (!arguments) return colorScale;
        colorScale = d3.scaleOrdinal(value);
        return myChart;
    }

    // function to get or set chart title
    myChart.title = function(value) {
        if (!arguments) return title;
        title = value;
        return myChart;
    }

    // function to get or set circle name variable
    myChart.nameVar = function(value) {
        if (!arguments) return nameVar;
        nameVar = value;
        return myChart;
    }

    return myChart;
}