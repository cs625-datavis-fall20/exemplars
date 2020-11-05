# Trupti - slope chart - Vega-Lite

## Explanation

For the recreation of this chart, only three attributes used, namely,
Region, Year, and Value. The Average year’s column is into one column
called “Year” as it is the same Ordered attribute. This sheet is
uploaded to the observable notebook as a CSV file.

All three attributes are used inside the *markLine* function in the
vega-lite to plot lines.

`slope = FileAttachment("Slope Chart.csv").csv()`

Hollow circles at the end of the line are plot using the *markPoint*
function on the same attributes as above.

In the original chart, the text is plot only on the left side of the
line at the end of the points. The *slope* dataset is further filtered
to plot this:

`slope1995 = slope.filter(d => d.Year === '1995-99 Average').filter(r =>
r.Region !== 'Sub-Saharan Africa')`

Region *Sub-Saharan Africa* and *Sub-Saharan Africa* gets overlapped if
plot without filtering, so filter it as above and store in the variable
*“slope1995”*. Function *markText* used to plot *Region* and *Values* at
the end of point marks.

The *markText* function is used to plot the value attribute. The data
for *2011-15 Average* is filtered as:

`slope2011 = slope.filter(d => d.Year === '2011-15 Average')`

The values for *slope1995* and *slope2011* are plot using the markText
function and aligned by adjusting the **dx** and **dy** parameters
passed to this function.

Now, to plot the value and region of **Sub-Saharan Africa**, one more
variable is used as below.

`slope1995_sub_saharan_africa = slope.filter(d => d.Year === '1995-99
Average').filter(r => r.Region === 'Sub-Saharan Africa')`

The variable *slope1995\_sub\_saharan\_africa* is plot twice, once to
plot the *Region* and again for the *Value* of this region. The **dx**
and **dy** parameters are adjusted to align them with other text as per
the original chart.

To assign specific colors to the line, point, and text corresponding to
the region as per the original chart, the *scale* function is under the
*color* function. Inside scale function, *domain* assigns the Region and
*range* assigns the color to the corresponding domain. The color codes
for each *range* are found from the *HTML Color Codes* website.

`scale({domain:['High income Asia and Pacific', 'North America', 'Europe
and Central Asia', 'South America and Caribbean' ,'Developing East Asia'
,'Middle East and North Africa', 'Sub-Saharan Africa', 'South Asia'],
range:['rgb(222,57,27)','rgb(31,143,125)','rgb(73,147,233)','rgb(222,209,27)','darkred','rgb(61,101,202)',
'rgb84,162,75)','rgb(16,24,85)']})`

The labels on X-axis for Year are displayed vertically by default. To
make them horizontal *“0”* value is assigned to the *labelAngle*
parameter under *axis* function.

`axis({labelAngle : 0})`

Remove legends of the chart by passing the *null* value to the *legend*
method of *Region(Nominal)* attribute. Value and the title of the Y-axis
is cleared by passing parameters, *axis* and *title* to the
*Value(Quantitative)* attribute.

The size of the mark points increases by changing the *size* parameter
passed to the markPoint function. The default size is “30”, increased it
to “50” to replicate the original chart.

## Source

```javascript
import {vl} from '@vega/vega-lite-api'
import {fromColumns, printTable} from '@uwdata/data-utilities'

slope = FileAttachment("Slope Chart.csv").csv()

slope1995 = slope.filter(d => d.Year === '1995-99 Average').filter(r => r.Region !== 'Sub-Saharan Africa')
slope2011 = slope.filter(d => d.Year === '2011-15 Average')
slope1995_sub_saharan_africa = slope.filter(d => d.Year === '1995-99 Average')
                                    .filter(r => r.Region === 'Sub-Saharan Africa')

vl.layer(
  vl.markText({ align: "right", dx: -30, dy: 1 })
  .data(slope1995)
  .encode(
    vl.x().axis({labelAngle : 0}).fieldO('Year').title(null),
    vl.y().fieldQ('Value').axis(null).title(null),
    vl.text('Year').fieldN('Region'),
    vl.color().fieldN("Region").scale({
      domain:['High income Asia and Pacific', 'North America', 'Europe and Central Asia', 
              'South America and Caribbean' ,'Developing East Asia' ,'Middle East and North Africa', 
              'Sub-Saharan Africa', 'South Asia'],
    range:['rgb(222,57,27)', 'rgb(31,143,125)','rgb(73,147,233)', 'rgb(222,209,27)','darkred', 
          'rgb(61,101,202)','rgb(84,162,75)', 'rgb(16,24,85)']
    })
  ),

  vl.markText({dx: -15, dy: 1 })
  .data(slope1995)
  .encode(
    vl.x().fieldO('Year'),
    vl.y().fieldQ('Value').axis(null).title(null),
    vl.text().fieldN('Value'),
    vl.color().fieldN("Region")
  ),
  
  vl.markText({dx:-78, dy:-10 })
  .data(slope1995_sub_saharan_africa)
  .encode(
    vl.x().fieldO('Year'),
    vl.y().fieldQ('Value').axis(null).title(null),
    vl.text().fieldN('Region'),
    vl.color().fieldN("Region")
  ),
  
  vl.markText({dx:-15, dy:-10 })
  .data(slope1995_sub_saharan_africa)
  .encode(
    vl.x().fieldO('Year'),
    vl.y().fieldQ('Value').axis(null).title(null),
    vl.text().fieldQ('Value'),
    vl.color().fieldN("Region")
  ),
  
  vl.markText({dx: 15, dy: 1 })
  .data(slope2011)
  .encode(
    vl.x().fieldO('Year'),
    vl.y().fieldQ('Value').axis(null).title(null),
    vl.text().fieldN('Value'),
    vl.color().fieldN("Region")
  ),
  
vl.markLine()
  .data(slope)
  .encode(
    vl.x().fieldO('Year'),
    vl.y().fieldQ('Value'),
    vl.color().fieldN('Region').legend(null)
  ),
  
vl.markPoint({size:50})
  .data(slope)
  .encode(
    vl.x().fieldO('Year'),
    vl.y().fieldQ('Value'),
    vl.color().fieldN('Region').legend(null)
  )
)
  .height(300)
  .width(700)
  .render()
```
