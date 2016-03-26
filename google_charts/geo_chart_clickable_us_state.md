# Basic US Map with clickable states using Google GeoChart

The requirement was for a quick clickable US Map with clickable states. We had multiple options like

- Draw image map on a PNG of a map
- D3 based Geo JSON
- GeoChart API

We did not want to spend time either for Geo JSON or image map. So we chose GeoChart. The default for Google GeoChart looks like a chloropeth map. So it comes with legend and data values for intensity.
### Options


Here are the options that worked after trial and error

```
var options = {
            region: “US”, 
            resolution: “provinces”,
            //displayMode:”text”,
            minValue: 0,  
            colors: [‘#333’, ‘#333’],
            color:”purple”,
            backgroundColor:”lightgray”,
            tooltip: {isHtml: true},
            magnifyingGlass:{enable: true, zoomFactor: 7.5},
            //tooltip: {“trigger”:”none”},
            legend:”none”                     
    };
```

If you put `displayMode:”text”` you would loose the color, but the name of the state would appear.

### Chart loaded event

If you would like to write your own javascript code to manipulate the SVG elements generated after the GoeChart is loaded

```javascript

 google.visualization.events.addListener(chart, ‘ready’, function(){
        $(“path”).attr(“fill”,”#85A694”);                    
    });

```

### Click Event handler

```javascript

google.visualization.events.addListener(chart, ‘select’, function(e){
        var state = data.getValue(chart.getSelection()[0].row,0);
      console.log(state);  
    });

```





 