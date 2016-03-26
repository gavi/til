# US Map with clickable states using Google GeoChart

The requirement was for a quick clickable US Map with clickable states. We had multiple options like

- Draw image map on a PNG of a map
- D3 based Geo JSON
- GeoChart API

We did not want to spend time either for Geo JSON or image map. So we chose GeoChart. The default for Google GeoChart looks like a choropleth map. So it comes with legend and data values for intensity. We dont need this. So the data looks like below. All states will have the same value.

### Data

```javascript
data = google.visualization.arrayToDataTable([
        ['State', ''],
        ['AL', 100],
        ['AK', 100],
        ['AZ', 100],
        ['AR', 100],
        ['CA', 100],
        ['CO', 100],
        ['CT', 100],
        ['DE', 100],
        ['DC', 100],
        ['FL', 100],
        ['GA', 100],
        ['HI', 100],
        ['ID', 100],
        ['IL', 100],
        ['IN', 100],
        ['IA', 100],
        ['KS', 100],
        ['KY', 100],
        ['LA', 100],
        ['ME', 100],
        ['MT', 100],
        ['NE', 100],
        ['NV', 100],
        ['NH', 100],
        ['NJ', 100],
        ['NM', 100],
        ['NY', 100],
        ['NC', 100],
        ['ND', 100],
        ['OH', 100],
        ['OK', 100],
        ['OR', 100],
        ['MD', 100],
        ['MA', 100],
        ['MI', 100],
        ['MN', 100],
        ['MS', 100],
        ['MO', 100],
        ['PA', 100],
        ['RI', 100],
        ['SC', 100],
        ['SD', 100],
        ['TN', 100],
        ['TX', 100],
        ['UT', 100],
        ['VT', 100],
        ['VA', 100],
        ['WA', 100],
        ['WV', 100],
        ['WI', 100],
        ['WY', 100]
    ]);
```

### Options


Here are the options that worked after trial and error

```javascript
var options = {
            region: "US", 
            resolution: "provinces",
            //displayMode:"text",
            minValue: 0,  
            colors: ['#333', '#333'],
            color:"purple",
            backgroundColor:"lightgray",
            tooltip: {isHtml: true},
            magnifyingGlass:{enable: true, zoomFactor: 7.5},
            //tooltip: {"trigger":"none"},
            legend:"none"                     
    };
```

If you put `displayMode:"text"` you would loose the color, but the name of the state would appear.

### Chart loaded event

If you would like to write your own javascript code to manipulate the SVG elements generated after the GoeChart is loaded

```javascript

 google.visualization.events.addListener(chart, 'ready', function(){
        $("path").attr("fill","#85A694");                    
    });

```

### Click Event handler

```javascript

google.visualization.events.addListener(chart, 'select', function(e){
        var state = data.getValue(chart.getSelection()[0].row,0);
        console.log(state);  
    });

```





 