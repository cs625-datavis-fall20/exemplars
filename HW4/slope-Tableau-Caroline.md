# Caroline - slope chart - Tableau

## Explanation

Similar to Part 1, the first thing I did was to normalize the data by
merging the two time spans into a single column so I ended up with
Region, Timespan, and Export Percent. Timespan was my column parameter,
and Export Percent was my rows parameter. I used a dual axis with Export
percent so that I could plot as both a line and a point. I set Region as
a dimension for color, but in the end, I ended up coloring the lines
manually, so I didn’t use that.

For my marks, I set Export Percent 1 to shape, filled circle, and Export
Percent 2 to line. I could have used circle for Export 1.

I turned off all titles and axes and gridlines, except for the x-axis
labels.

I didn’t actually use data labels, instead I used annotations. This
allowed me to set color and position for each point separately. For most
of the annotations I included the fields (e.g. <Region>), but for the
South America and… I typed in the text as I didn’t find a way to have it
use ellipses instead of wrapping the labels.

I also colored the marks manually as I didn’t see a way to set the color
automatically based on a calculation.

I used a dual axis to create the endpoints for the lines. This allowed me to resize the data points separately from the lines. I was able to get the size right using this method, but not the shape.
