# Caroline - small multiples line charts - R

## Explanation

The first thing I did was to reformat the data. The original data set
had columns for White, Black, Asian, and Hispanic. I merged those 4
columns into a single column called Race. This was so that I would be
able to set Race as a dimension for grouping. I also had to mutate the
Year data points so that they could be used as dates.

Next, I defined my colors. I used Paint’s eyedropper tool to find the
hex code for the colors used.

The main challenge for this chart was figuring out how to do small
multiples using R. I found a function called facet\_grid which did this
for me automatically, so it was very easy to do.

After this, it was all a matter of formatting. I used two different
geom\_text statements to place the starting and ending data labels so
that I could position them correctly in relation to the line. The rest
is pretty straightforward setting font sizes, formatting dates and
labels, etc.

## Source

```r
library(ggplot2)
library(dplyr)
library(scales)

# Read in the modified data set
Data <- read.csv(file = "PolicyViz_WSJ_Remake_Normalized.csv")

# Change the Lab_Report_Date into a date attribute so that the x axis is ordered.
Data <- mutate(Data, Date = as.Date(ISOdate(Year, 1, 1)))

cBlue <- "#305578"
cRed <- "#AC2C36"
cGreen <- "#5C9866"
cYellow <- "#ECBA2D"
palette(c(cBlue, cRed, cGreen, cYellow))

# Set Race as the factor, and select the order we want the races to appear in the plot
Data <- transform(Data, Race=factor(Race,levels=c("White","Black","Asian", "Hispanic")))

g <- ggplot(Data, aes(x = Date, colour = Race))

#Set the channel to be line charts with points marked.
g <- g + geom_path(aes( y = Percent), size = 1.5)
g <- g + geom_point(aes(y = Percent), shape = 21, fill = "white", size = 3, stroke = 1.5)

#Turn on data labels for first and last values only
g <- g + geom_text(data = subset(Data, Date == "2014-01-01"),aes(y = Percent,colour = Race, 
    label = formatC(Percent,format = "f", digits =1, drop0trailing = FALSE)), nudge_y = -1)
g <- g + geom_text(data = subset(Data, Date == "2004-01-01"),aes(y = Percent,colour = Race, 
    label = formatC(Percent,format = "f", digits =1, drop0trailing = FALSE)), nudge_y = 1)
    
theme(plot.margin = unit(c(1,3,1,1), "lines"))

# facet_wrap(Race ~ ., nrow = 1) +
g <- g + facet_grid(cols = vars(Race))

#Define custom colors for our plots to match the original
g <- g + aes(fill = Race)
g <- g + scale_color_manual(values = c(cBlue, cRed, cGreen, cYellow))

#Remove x and y labels
g <- g + xlab("")
g <- g + ylab("")

#Format x axis to display 2 digit year
g <- g + scale_x_date(date_breaks = "2 years",
                 date_minor_breaks = "6 months",
                 labels=date_format("'%y"),
                 limits = as.Date(c('2003-01-01','2015-12-31')))

#Format y axis to go from 0 to 20 with breaks at 2 percent points.
g <- g + scale_y_continuous(expand = c(0, 0), limits = c(0, 20), breaks = seq(0, 20, by = 2) )

#Set the title and subtitle text
g <- g + labs(title = "Out of Work", subtitle = "Percent of families with at least one member unemployed")

#set themes for the plot -- remove vertical grid lines and all minor grid lines.
#Set the grid line color and format the axis labels.
g <- g + theme(panel.grid.major.x = element_blank(),
      panel.background = element_blank(),
      axis.line = element_blank(),
      panel.grid.major.y = element_line(color = "darkgrey"),
      panel.border = element_rect(fill = NA, colour = "darkgrey"),
      legend.position = "none",
      axis.text.x = element_text(size = 11, face="bold"),
      axis.text.y = element_text(size = 11, face="bold"),
      plot.title = element_text(size = 19, face = "bold"),
      plot.subtitle = element_text(size = 11),
      panel.spacing.x = unit(0, "lines"))

# Change the panel labels to the size we prefer, and make the fill be white
g <- g + theme(strip.text.x = element_text( size= 16),
      strip.background = element_rect(color="white", fill="white"))

g
```
