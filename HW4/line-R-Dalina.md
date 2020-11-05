Dalina - HW4

## Source 

```r
library(reshape2)
library(directlabels)
library(extrafont)
library(tidyverse)
font_import()
loadfonts(device = "win")

p1_lc <- read_csv("PolicyViz_WSJ_Remake_p1_line_chart_multi_line.csv")
d <- melt(p1_lc, id.vars="Year")

png(filename="p1_recreate.PNG", width=1024, height=613)

ggplot(d, aes(x = Year, y = value, group = variable, colour = variable)) + 
geom_line(lwd=1.4)+ 
theme_bw() + 
scale_color_manual(values=c("#1C00AF","#E70000" ,"#0A9400","#BD7F00")) +
theme(legend.position = "none", axis.ticks = element_blank(), panel.border = element_blank(), 
    panel.grid.major.x = element_blank(), panel.grid.minor.x = element_blank(),
    panel.grid.major.y =element_line( size=.9, color="#CBCBCB"), panel.grid.minor.y = element_blank(), 
    axis.title.x = element_blank(), axis.title.y = element_blank(), 
    axis.text.x = element_text( margin = margin(t = 8), size = 21, face="bold"),  
    axis.text.y = element_text(margin = margin(r = 10), size = 21, face="bold"), 
    plot.margin=unit(c(.5,1.5,.3,.3),"cm"), plot.title= element_text(size=34,face="bold", hjust = -.045),  
    plot.subtitle = element_text(size=20, margin = margin(b=28), hjust = -.07)) +
labs(title = "Out of Work", subtitle = "Percent of families with at least one member unemployed") +
scale_y_continuous(limits=c(0, 20), breaks = seq(0, 20, by = 2), expand = c(0,0)) +
scale_x_continuous(labels = as.character(p1_lc$Year), breaks = p1_lc$Year, expand = c(0,0)) + 
geom_label(aes(x=2012, y = 18, label = "Black", color = "Black"), fill="white", label.size = NA, size=6.5, 
          hjust=-.3) + 
geom_label(aes(x=2012, y = 13, label = "Hispanic", color = "Hispanic"), fill=NA, label.size = NA, size=6.5, 
          hjust=-.02, vjust=.5) +
geom_label(aes(x=2012, y = 10, label = "White", color = "White"), fill="white", label.size = NA, size=6.5, 
          hjust=-.3) +
geom_label(aes(x=2012, y=7, label="Asian",color="Asian"), fill = "white", label.size = NA, size=6.5, hjust=-.3)
      
dev.off()
```

## Explanation

The library reshape2 was used to adjust the data by using the melt()
function. The melt() function transformed the data into a column of
years labeled *Year*, a column containing the ethnic groups labeled
*variable*, and a column containing the percentages labeled *value*.

The png() function supports adjustments to the width and height of the
exported chart.

To plot the percentages for each ethnic group, the x-axis contained the
Years and the y-axis contained the percentages, where each line
reflected each ethnic group. The line with of the lines can be adjusted
using *geom\_line* with the parameters *lwd*. Each line can be given a
color by usin *scale\_color\_manual*.

Adding theme\_bw() removes the grey background from the chart.

Adjusting the elements of the chart can be done using the theme()
function. The legend, tick marks, panel border, x/y-axis title,
minor/major x-axis grid, as well as, the minor y-axis grid lines were
removed from the chart. The major y-axis grid line was given a grey
color and the width size of the line was increased. The color codes were
acquired from an html color code website. The x and y-axis text font
size were adjusted and set the bold. Additionally, The margin around the
text were adjusted using the *margin* parameter. The title and subtitle
property were also modified by adjusting the text size, face, margin,
and alignment. The *hjust* parameter was used to align the text
horizontally.

The title and subtitle text were added by using the labs() function.

To modify the y-axis values, a limit between 0 and 20 was set where all
even numbers were displayed. Using the parameter *expand* allowed for
the removal of spaces at the end and beginning of the list of values.
For the x-axis values, each year was displayed by setting the *breaks*
to *Year*.

Each ethnic group was given a label to display by the corresponding line
shown. The geom\_label() function was used to display the labels for
each ethnic group. Setting *fill* to *NA* removed the background color
from the label or setting it to white created a contrast for the label
to be seen when displayed above a line. The size and alignment were
adusted for each label and the position along the x/y-axis displayed by
using the aes() function.

Lastly, the function dev.off() exported the chart to the png file noted
earlier.

