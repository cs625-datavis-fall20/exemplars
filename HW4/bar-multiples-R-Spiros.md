# Spiros - small multiples bar charts - R

## Source

```r
library(ggplot2)
library(tidyr)
library(grid)

#   read dataset
df <- data.frame(read.csv("PolicyViz_OECD_Skills_Data.csv"))

#   order countries based on total percentage values
df$Country <- factor(df$Country, levels = df$Country[order(df$Total)])

#   create a combination of column to preview it using geom_bar
df <- gather(df, key = "category", value = "value", c("Low.skilled", "Medium.skilled", "High.skilled", "Total"))

# Change order of strip texts
df$category <- factor(df$category, levels = c("Low.skilled", "Medium.skilled", "High.skilled", "Total"))

#   set column labels
variable_names <- list(
  "Low.skilled" = "Low Skilled",
  "Medium.skilled" = "Middle Skilled",
  "High.skilled" = "High Skilled",
  "Total" = "Total Trained Workforce"
)
variable_labeller <- function(variable, value) {
  return(variable_names[value])
}
colors <- c("#006bb9", "#80a6d7", "#00a9cf", "#294476")
plot <- ggplot(df, aes(x = Country, y = value, fill = category)) +
  geom_bar(stat = "identity", width = 0.5) +
  facet_wrap(~category, ncol = 4, labeller = variable_labeller) +
  coord_flip() +
  ylab("") +
  xlab("") +
  geom_hline(aes(yintercept = -Inf), color = "black") +
  scale_y_continuous(expand = c(0, 0)) +
  scale_fill_manual(values = colors) +
  scale_colour_manual(values = colors) +
  theme(panel.background = element_blank(), panel.grid.major.x = element_blank(), 
        panel.grid.major.y = element_blank(), axis.text.x = element_blank(), axis.ticks.x = element_blank(), 
        axis.ticks.y = element_blank(), strip.background = element_blank(), 
        strip.text = element_text(hjust = 0, size = 15), legend.position = "none")

#   set color of strip.text for each facet
g <- ggplot_gtable(ggplot_build(plot))
strip_both <- which(grepl('strip-', g$layout$name))
k <- 1
for (i in strip_both) {
  j <- which(grepl("text", g$grobs[[i]]$grobs[[1]]$childrenOrder))
  g$grobs[[i]]$grobs[[1]]$children[[j]]$children[[1]]$
    gp$
    col <- colors[k]
  k <- k + 1
}
grid.draw(g)
```
