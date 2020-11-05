# Heramb -  small multiples bar charts - R

## Explanation

Below are the steps taken to recreate the chart using vega-lite.

  - The above chart is a small multiples bar chart where *marks* are
    **lines** and *channels* are **Horizontal positions of attributes**.
    The data consists of *one quantitative attribute*(***High Skilled,
    low skilled, middle skilled, total trained workforce columns***) and
    *one categorical attribute*(***states***).

  - As this is a small multiples bar chart I have charted 4 different
    graphs and concatenated them using `hconcat()` method from
    *vega-lite*.

  - All the four bar charts were created using the `markBar()` method of
    *vega-lite*. The data on the y-axis is ordered in a descending way
    based on the **‘Total Trained workforce’** column. The x-axis on
    each plot different based on the columns(**High Skilled, low
    skilled, middle skilled, total trained workforce columns**).

  - The width of each plot is different and is configured using a
    built-in method `width()` from the *vega-lite* library. Below are
    the widths of each plot.
    
      - Low Skilled: 180
      - Middle Skilled: 180
      - High Skilled: 200
      - Total Trained workforce: 400

  - The range of x-axis is also different for each plot where the first
    three plots(*Low Skilled, Medium Skilled, High Skilled*) have the
    range of \[0,50\] the final plot of *Total Trained Workforce* have a
    range of \[0,100\]. This can be configured using the `scale()`
    method with the `domain` key to specify the domain range.

  - All the other attributes of the graph like ‘grid’, ‘ticks’, ‘title’,
    ‘colors’, ‘font’ can be configured in the `axis()` method. Below are
    some of the field values.
    
      - font: Calibri
      - color codes
          - low skilled: \#006bb8
          - medium skilled: \#7fa8d9
          - high skilled: \#00aacc
          - total trained workforce: \#264478

## Source

```javascript
import {vl} from @vega/vega-lite-api
import {fromColumns, printTable} from @uwdata/data-utilities

dataQ2 = FileAttachment("part-2-b.csv").csv({typed: true})

{
  const yVal = vl.y().fieldN('Country').title('').sort(vl.max("Total").order("descending"));
  
  const lowSkilled = vl.markBar({size: 10, color: '#006bb8'})
      .data(dataQ2)
      .encode(
        vl.x().fieldQ('High skilled').scale({ domain: [0,50] })
        .axis({
        'domainColor': 'white',
        'orient':'top',
        'labels':false,
        'grid':false,
        'tickSize':5, 
        'tickColor':'white',
        'title':'Low Skilled',
        'titleFont':'calibri light',
        'titleFontSize':20,
        'titleAnchor':'start',
        'titleAlign':'left',
        'titleOrient':'top',
        'titleColor':'#006bb8',
        }),
        yVal.axis({'tickSize':5, 'tickColor':'white', 'labelFont':'calibri','labelFontSize':12,
            'labelColor':'grey'}),
      )
      .width(180);
      
  const midSkilled = vl.markBar({size: 10, color: '#7fa8d9'})
      .data(dataQ2)
      .encode(
        vl.x().fieldQ('Medium skilled').scale({ domain: [0,50] })
        .axis({
        'domainColor': 'white',
        'orient':'top',
        'labels':false,
        'grid':false,
        'tickSize':5, 
        'tickColor':'white',
        'title':'Middle Skilled',
        'titleFont':'calibri light',
        'titleFontSize':20,
        'titleAnchor':'start',
        'titleAlign':'left',
        'titleOrient':'top',
        'titleColor':'#7fa8d9'
        }),
        yVal.axis({'labels':false,'ticks':false}),
      )
      .width(180);
      
  const highSkilled = vl.markBar({size: 10, color: '#00aacc'})
      .data(dataQ2)
      .encode(
        vl.x().fieldQ('Low skilled').scale({ domain: [0,50] })
        .axis({
        'domainColor': 'white',
        'orient':'top',
        'labels':false,
        'grid':false,
        'tickSize':5, 
        'tickColor':'white',
        'title':'High Skilled',
        'titleFont':'calibri light',
        'titleFontSize':20,
        'titleAnchor':'start',
        'titleAlign':'left',
        'titleOrient':'top',
        'titleColor':'#00aacc'
        }),
        yVal.axis({'labels':false,'ticks':false}),
      )
      .width(200);
      
  const total = vl.markBar({size: 10, color: '#264478'})
      .data(dataQ2)
      .encode(
        vl.x().fieldQ('Total').scale({ domain: [0,100] })
        .axis({
        'domainColor': 'white',
        'orient':'top',
        'labels':false,
        'grid':false,
        'tickSize':5, 
        'tickColor':'white',
        'title':'Total Trained Workforce',
        'titleFont':'calibri light',
        'titleFontSize':20,
        'titleAnchor':'start',
        'titleAlign':'left',
        'titleOrient':'top',
        'titleColor':'#264478'
        }),
        yVal.axis({'labels':false,'ticks':false}),
      )
      .width(400);
      
  return vl.hconcat(lowSkilled, midSkilled, highSkilled, total)
    .config({ concat: { spacing: 10 } , view:{"stroke": "transparent"}})
    .render();
}
```
