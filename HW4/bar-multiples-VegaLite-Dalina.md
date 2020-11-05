# Dalina - small multiples bar charts - Vega-Lite

## Explanation

To begin the Observable notebook, the Vega-Lite API was imported. The
API contained the functions needed to recreate the chart.

The json file was attached to the observable notebook with the
FileAttachment() function and set to the *demand* variable.

Each skill level was created as a standalone chart and would then be
concatenated to display all together. For example, the *Low Skilled*
chart was created with the code below. The other skill levels were
created similarly except the y-axis labels were kept on the *Low
skilled* chart. Each markBar() was given a color from an html color code
and height which was adjusted be made the same height as the bars in the
original chart. A title, as well as, a color corresponding to the bar,
the position, font size, font family, and font weight were given to each
skill set. None of the skill levels had x-axis labels therefore the
axis() function was set to *null*. The values within each skill level
was a quantitative value therefore the fieldQ() function was used to
display the value along the horizontal position. Each skill level was
sorted by *Total* column with the *Country* displayed along the y-axis.
For the country label, the label color was set to grey, the size
adjusted to 10, and padding was added between the label and the chart.
The width() and height() function was added to each skill level chart to
make adjustments to the size of the chart in order to closely resemble
the original chart.

The Middle Skilled, High Skilled, and Total charts had the titles,
labels, and ticks removed from the y-axis.

As mentioned earlier each individual skill level chart was concatenated
to produce one chart with all skill levels and the total. The chart was
horizontally concatenated using the hconcat() function. Spacing was
added between each chart with the *spacing* parameter. The borders
around each chart was removed with *strpke* parameter set to
*transparent*. Lastly, the render() function was used to render the
chart.

## Source

```javascript
import { vl } from "@vega/vega-lite-api"

demand = FileAttachment("PolicyViz_OECD_Skills_Data_p2_small_multi.json").json()

viewof smallMulti = {
  
  // low
  const low = vl.markBar({color: "#3D60BE", height:8})  // Make a bar chart  
  .title({text:"Low Skilled", color:"#3D60BE", anchor:"start", dx:81, dy:-10, font: "Arial", fontSize:16, 
        fontWeight:"normal"})
  .encode( 
    vl.y({sort:"Total"}).fieldN("Country").axis({title:null, ticks:false, labelPadding:10, labelColor:"grey", 
        labelFontSize:10}),
    vl.x().fieldQ("Low skilled").axis( null)
   
  )
  .width(65)
  .height(484)
  ;
  // mid
  const mid = vl.markBar({color: "#92A3D5", height:8})
  .title({ text: "Middle Skilled", color: "#92A3D5", anchor:"start", dy:-10, font: "Arial", fontSize:16, 
      fontWeight:"normal"})
     .encode( 
    vl.y({sort:"Total"}).fieldN("Country").axis({title:null, labels:false, ticks:false}), 
    vl.x().fieldQ("Medium skilled").axis(null)
  )
    .width(100)
  .height(484)
  ;
  //high
  const high = vl.markBar({color: "#29A6D1", height:8})
  .title({ text: "High Skilled",color: "#29A6D1", anchor:"start", dy:-10, font: "Arial", fontSize:16, 
      fontWeight:"normal"})
   .encode( 
    vl.y({sort:"Total"}).fieldN("Country").axis({title:null, labels:false, ticks:false}), 
    vl.x().fieldQ("High skilled").axis(null)
  )
    .width(165)
  .height(484)
  ;
  // Total
  const tot = vl.markBar({color: "#1E3768", height:8})
  .title({ text: "Total Trained Workforce", color: "#1E3768", anchor:"start", dy:-10, font: "Arial", 
      fontSize:16, fontWeight:"normal"})
   .encode( 
    vl.y({sort:"-x"}).fieldN("Country").axis({title:null, labels:false, ticks:false}), 
    vl.x().fieldQ("Total").axis(null)
  )
    .width(325)
  .height(484)
  ;

  
  return vl.hconcat(low, mid, high, tot)
    .data(demand)
  .config({ concat: { spacing: 40}, view:{ stroke: "transparent"}})
    .render();
}
```
