# Abigail - small multiples bar charts - R

## Source

```r
library(tidyr)
library(ggplot2)
library(grid)

# Read in the CSV file
data <- read.csv("Q2_Data.csv")

# Add levels to the Area column to ensure that the places are ordered the
# same on the barchart as they are in the data file. Essentially, prevent
# geom_bar from ordering alphabetically.
data$Area <- factor(data$Area, levels = data$Area)

# Create dataframe to use with geom_bar to create the 4 separate charts
df <- data.frame(data$aLow.skilled, data$cHigh.skilled, data$bMedium.skilled, data$dTotal, data$Area)

# Create the list of category titles for the facet labeller  
categories <- c(
  `data.cHigh.skilled`="High Skilled                    ",
  `data.bMedium.skilled`="Middle Skilled                ",
  `data.aLow.skilled`="Low Skilled                     ",
  `data.dTotal`="Total Trained Workforce  "
)

# Create the labeller function
plot_labeller <- function(variable,value){
  return(categories[value])
}

# Create the list of colors for the facet strip text
txt_colors <- c("dodgerblue3", "cornflowerblue", "turquoise3", "royalblue4")

# Create the barchart
barchart <- ggplot(data = df %>% gather(Variable, Percent, -data.Area)) +
  aes(x = data.Area, y = Percent, fill = Variable) +
  geom_bar(stat = 'identity', position = 'dodge', width = 0.5) +
  scale_x_discrete(limits = rev(levels(data$Area))) +
  scale_fill_manual(name = "", labels = c("Low Skilled", "Middle Skilled", "High Skilled", 
      "Total Trained Workforce"), values = c("dodgerblue3", "cornflowerblue", "turquoise3", "royalblue4")) +
  facet_grid(~ Variable, labeller = as_labeller(categories)) +
  coord_flip() +
  theme(legend.position="none") + # Remove legend
  theme(strip.background =element_rect(fill="white"), strip.text = element_text(size = 16)) + # Edit facet strips
  # Edit panel theme
  theme(panel.grid.major = element_blank(), panel.grid.minor = element_blank(), 
        panel.background = element_blank()) +
  # Remove x axis and edit y axis
  theme(axis.title.x = element_blank(), axis.text.x = element_blank(), axis.ticks.x = element_blank(), 
      axis.title.y = element_blank(),  axis.line.x = element_blank(), axis.ticks.y=element_blank(), 
      axis.line = element_line(colour = "black"))

# Generate the ggplot2 plot grob from the barchart
g <- grid.force(ggplotGrob(barchart))
# Get the names of grobs and their gPaths into a data.frame structure
grobs_df <- do.call(cbind.data.frame, grid.ls(g, print = FALSE))
# Build optimal gPaths that will be later used to identify grobs and edit them
grobs_df$gPath_full <- paste(grobs_df$gPath, grobs_df$name, sep = "::")
grobs_df$gPath_full <- gsub(pattern = "layout::", 
                            replacement = "", 
                            x = grobs_df$gPath_full, 
                            fixed = TRUE)

#Get the gPaths of the strip background grobs
strip_bg_gpath <- grobs_df$gPath_full[grepl(pattern = ".*strip\\.background.*", x = grobs_df$gPath_full)]

# Get the gPaths of the strip titles
strip_txt_gpath <- grobs_df$gPath_full[grepl(pattern = "strip.*titleGrob.*text.*", x = grobs_df$gPath_full)]

# Edit the grobs
for (i in 1:length(strip_bg_gpath)){
  g <- editGrob(grob = g, gPath = strip_txt_gpath[i], gp = gpar(col = txt_colors[i]))
}

grid.draw(g)

```

## Explanation

*The libraries used were tidyr, ggplot, and grid.*

1. First, the dataset needed to be read in: `data <- read.csv("Q2_Data.csv")`

2. Next, ggplot likes to order the different countries alphabetically by default. To force it to keep the countries in the same order that they are in the data file, I needed to set a factor for the Area column and set the levels in the columns to the country entries themselves: `data$Area <- factor(data$Area, levels = data$Area)`

3. The data contains extra columns that are not needed, so a new data frame that only contains the required columns needed to be created: `df <- data.frame(data$aLow.skilled, data$cHigh.skilled, data$bMedium.skilled, data$dTotal, data$Area)`

4. Next, a list of colors and a labeller function for the facets was created. The facet labels contain spaces in order to left-align the strip text. The colors for the text are the same as the ones used for the bars.

5. With setup tasks complete, ggplot could be used. In the function call, the data was gathered into key-value pairs so that the different bar charts could be created: `ggplot(data = df %>% gather(Variable, Percent, -data.Area))`. Then the x and y axes were set and geom_bar was used to created the intial grouped bar chart. The discrete scale was set to the country levels in the Area column. 

6. Next the titles and colors for the four different bar charts were set.

7. The facet grid was then created. The labeller was passed as an argument to properly label the facets. 

8. Coordinate flip and other theme settings were then applied: 
    * `coord_flip() `
    * Remove legend.
    * Set facet strip background to white and change font size.
    * Panel grids were removed.
    * Removed x axis and edited y axis labels.

9. Finally, the strip text color needed to be altered to match the bars. To do this, the plot was converted to a grob and the strip text was grabbed using grepl. The resource for this technique can be found here: [Change color of font and background in facet strip?](https://stackoverflow.com/questions/53455092/r-ggplot2-change-colour-of-font-and-background-in-facet-strip)

10. Finally, the chart could be drawn: `grid.draw(g)`
