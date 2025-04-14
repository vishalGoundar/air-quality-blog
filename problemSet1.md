---
title: "ENVSCI 705 'Handling Environmental Data' - Problem Set 1"
author: "Vishal Goundar vgou006"
output: html_document
---

Answers to this problem set are due **Friday 21^st^ March** [week 3 of semester].  Please upload the RMarkdown file via Canvas - you don't need to upload the knitted output. **Submissions in other file formats will not be marked**  You will need to install and use the `ggplot2` and `dslabs` packages to complete this problem set. `dslabs` provides some data sets that we will use here.  You will need to use the help to answer some of these questions, but we have discussed nearly everything here in the first class sessions. Insert the code chunks and any comments after each question.  Make sure to name the code chunks and include code comments!  For all plots, axes should be labelled appropriately, etc.; otherwise all the instructions are given below.   

In the following, I have used *italics* for variable (column) names, **bold** for data frames, and the standard formatting for R code.


#### Problem 1
The chunk below has some broken code in it so (i) the document will not knit and (ii) R will throw some errors.  Fix the chunk so the document knits and the code runs. I have commented the lines out to make this document - you will need to uncomment them.

```{r 1_fix_syntax}
# Add missing commas in letter_words
letter_words <- c("the", "quick", "brown", "fox")

# Correct function name (`medain` to `median`) and replace `;` with `:`
median(1:10)

# Add missing closing parenthesis `)` in max()
max(c(1, 17, 2, -9))

# Remove extra space and add missing comma
grp <- c("species 1", "species 2")

```

```{r 1_fix_logic}
# Define `x` before computing mean
x <- c(1, 2, 3, 4, 5)
mean_x <- mean(x)

# Change `==` (comparison) to `<-` (assignment)
y <- mean_x

# Convert `a` from character to numeric before addition
a <- as.numeric("5")
b <- 10
a + b  # Now correctly adds 5 + 10

# Remove the incorrect `negative = FALSE` argument in `sum()`
sum(1:3)
```

#### Problem 2
Load the dslabs package (use `install.packages` if you haven't previously installed it).  Open the built-in data frame called **brexit_polls**.  Provide five different ways of showing how many rows are in the **brexit_polls** data frame.  Give two commands that provide a summary (statistical or data structure) of the **brexit_polls** dataframe.

```{r 2_load_libraries}
# Load dslabs library
library(dslabs)
```

```{r 2_load_data}
# Load the brexit_polls data
data(brexit_polls)
```

```{r 2_count_number_of_rows}
# 1. Using the nrow() function
nrow(brexit_polls) # The `nrow()` function returns the number of rows in the `brexit_polls` dataset.

# 2. Using the dim() function (returns dimensions, rows are the first element)
dim(brexit_polls)[1]

# 3. Count the number of rows in the first column of the data frame
length(brexit_polls[[1]]) 

# 4. Using the rownames() function
length(rownames(brexit_polls))  # Counts the number of row names

# 5. Using the attr() function to check row attributes
length(attr(brexit_polls, "row.names"))

```
```{r 2_statistical_summary}
# print summary of the data
summary(brexit_polls) 
```

- The dataset covers **polling from January 8, 2016, to June 23, 2016**.
- The variable **poll_type** contains two values: **Online** and **Telephone**.
- Five other variables provide numerical summaries, as displayed in `summary()`.


```{r 2_data_structure}
# Display structure, column types, and first few values
str(brexit_polls)
```

The dataset contains `r nrow(brexit_polls)` observations across `r ncol(brexit_polls)` variables.

#### Problem 3
Assign the standard deviation of the _remain_ variable to an object. Create another object and assign it the value of the mean of the _spread_ variable. Finally, calculate the [coefficient of variation, CV](https://en.wikipedia.org/wiki/Coefficient_of_variation) for the _leave_ variable and store it. An alternative to the CV is the [quartile coefficient of dispersion] (https://en.wikipedia.org/wiki/Quartile_coefficient_of_dispersion), calculate this as well.  I have not named chunks from this point -- you need to do this.

```{r 3_load_libraries}
# load library
library(dslabs)
```

```{r 3_load_data}
# load data
data(brexit_polls)

# Check first few lines
head(brexit_polls)
```

```{r 3_data_structure}
# print data structure
str(brexit_polls)
```
The dataset contains `r nrow(brexit_polls)` observations across `r ncol(brexit_polls)` variables.
Two of these are of data type Date, two of these are of data type Factor, and five of these are of data type numeric.

```{r 3_std_dev_remain}
# Assign the standard deviation of the remain variable to an object
std_remain <- sd(brexit_polls$remain, na.rm = TRUE)

std_remain
```

```{r 3_mean_spread}
# Assign the mean of the spread variable to another object
mean_spread <- mean(brexit_polls$spread, na.rm = TRUE)

mean_spread
```

```{r 3_cv_leave}
# Calculate the coefficient of variation (CV) for the Leave variable
mean_leave <- mean(brexit_polls$leave, na.rm = TRUE)
std_leave <- sd(brexit_polls$leave, na.rm = TRUE)
cv_leave <- std_leave / mean_leave

cv_leave
```

```{r 3_qcd_leave}
# Calculate the quartile coefficient of dispersion (QCD) for the Leave variable
q1_leave <- quantile(brexit_polls$leave, probs = 0.25, na.rm = TRUE, names = FALSE)

q3_leave <- quantile(brexit_polls$leave, probs = 0.75, na.rm = TRUE, names = FALSE)

qcd_leave <- (q3_leave - q1_leave) / (q3_leave + q1_leave)

qcd_leave
```

```{r 3_print_results}
# Print results for clarity in Markdown output
cat("Standard Deviation of Remain:", std_remain, "\n")
cat("Mean of Spread:", mean_spread, "\n")
cat("Coefficient of Variation for Leave:", cv_leave, "\n")
cat("Quartile Coefficient of Dispersion for Leave:", qcd_leave, "\n")
```

#### Problem 4
Using ggplot, make a histogram of the _temp_ variable in the **stars** dataset (again in `dslabs`). 
You can see that the distribution of _temp_ is right-skewed. Use `xlim` in ggplot to restrict the x-axis length. 

```{r 4_load_libraries}
# Load required libraries
library(dslabs)
library(ggplot2)
```
```{r 4_load_stars_data}
# Load dataset
data("stars")

# check first few rows
head(stars)
```
```{r 4_data_structure}
# print the data structure
str(stars)
```
- The stars dataset contains `r nrow(stars)` observations across `r ncol(stars)` variables. One variable is of Factor data type, one is of numeric, one variable is of integer data type and one variable is of character data. 

```{r 4_investigate_missing_values}
# Find the number of rows with missing values in temp column
length(stars[is.na(stars$temp), ])
```
There are 4 rows with missing temp values. 

```{r 4_remove_missing_value_rows}
# Delete the observations with missing values
stars <- stars[!is.na(stars$temp), ]

# check the data structure
str(stars)
```

```{r 4_create_histogram, fig.align = "center"}
# Create a histogram of the temp variable with axis tick marks
p <- ggplot(stars, aes(x = temp)) +
  labs(
    title = "Histogram of Star Temperatures",
    x = "Temperature (K)",
    y = "Frequency") +
  theme(
    plot.title = element_text(hjust = 0.5, face="bold"),
    axis.ticks = element_line(color = "black"), # Ensure tick marks are visible
    axis.ticks.length = unit(0.1, "cm"), # Adjust tick mark Length
  ) + 
  theme_set(theme_minimal())
```

```{r 4_visualize_the_plot}
p + 
  geom_histogram(binwidth = 1000, color = NA)

```

- The histogram shows that temperature values are right skewed and most of the stars temperature's are below 10000K.

```{r 4_investigate_high_temperature}
# print the observations of star temperature more than 30000
stars[stars$temp > 30000, ]
```
- The high temperature is for the star: Alnitak.
- This star is one of the brightest stars in the night sky.  
Reference: https://nineplanets.org/alnitak-%CE%B6-orionis/?utm_source=chatgpt.com

```{r 4_check_outside_scale_range}
# print the number of observations with temp values outside the range of 0 to 10000
length(stars[stars$temp >= 0 & stars$temp <= 10000, ])
```

```{r 4_restrict_x-axis}
# plot by restricting the x-axis to the interval of 0 to 10000
p +
  geom_histogram(binwidth = 300, color = NA) + 
  scale_x_continuous(limits = c(0, 10000)) + # Maintains the full dataset but restricts the visible range
  labs(subtitle = "With restricted x-axis") +
  theme(
    plot.subtitle = element_text(hjust = 0.5, face = "italic"),
  )
```

- The restricted histogram shows that most stars have temperatures ranging from 2500K to 5000K, supporting the observation that most stars are cooler than blue giants.

#### Problem 5
Open the **historic_co2** dataframe from the `dslabs` package.  Using ggplot, plot the changes in CO~2~ over time, modifying the theme, labelling axes, etc. as you wish.  Use different colours to denote the different data sources. 

```{r 5_load_libraries}
# load libraries
library(ggplot2)
library(dslabs)
```

```{r 5_load_data}
# load data
data(historic_co2)

# check first few rows
head(historic_co2)
```
```{r 5_data_structure}
# print structure of the dataset
str(historic_co2)
```
-  The historic_co2 dataset contains `r nrow(historic_co2)` observations across `r ncol(historic_co2)` variables. Two variables are of data type numeric, and one variable is of data type character.

```{r 5_investigate_source}
# unique values of source
unique(historic_co2$source)
```
- The source variable has two values and the data type could be converted to factor.

```{r 5_convert_data_type}
# convert char to factor
historic_co2$source <- factor(historic_co2$source)

# Data structure
str(historic_co2)
```
```{r 5_investigate_year}
# print the summary of the year variable
summary(historic_co2$year)
```
- The year variable range from year 803182 before the common era (BCE) to the recent year of 2018 in the common era (CE). The high number of years before the common era is better shown on a log scale graph.

```{r 5_investigate_Mauna_Loa}
# print the summary of the source Mauna Loa
summary(historic_co2[historic_co2$source == "Mauna Loa", ])
```
- There are 60 observations sourced from Mauna Loa, for which the common era years range from 1959 to 2018 and the \(CO_2\) range from 316ppm to 408.5ppm.
- The \(CO_2\) levels from Mauna Loa are more focused on modern era.

```{r 5_investigate_Ice_Cores}
# print the summary of the source Ice Cores
summary(historic_co2[historic_co2$source == "Ice Cores", ])
```
- There are 634 observations sourced from Ice Cores, for which the years range from 803182 before the common era to 2001 in the common era.
- The \(CO_2\) level range from 177.7ppm to 368ppm.

```{r 5_missing_data}
# Check for rows with missing data
historic_co2[!complete.cases(historic_co2), ]
```
- None of the rows contain missing data

```{r 5_co2_trend_plot, fig.align = "center"}
# plot the line plot of CO2 vs. year of different source
p <- ggplot(historic_co2, aes(x = year, y = co2, color = source, group = source)) +
  geom_line(linewidth = 0.4) +  # Line plot to show CO2 changes over time
  scale_color_manual(values = c("Ice Cores" = "#1b9e77", "Mauna Loa" = "#d95f02")) +
  labs(
    title = "Atmospheric CO2 Changes Over Time",
    y = "CO2 Concentration (ppm)",
    color = "Data Source: "
  ) +
  theme_set(theme_minimal()) +
  theme(
    text = element_text(size = 12),
    plot.title = element_text(hjust = 0.5, size = 12, face = "bold"),
    legend.key = element_blank(), # Removes the box around legend keys
    legend.key.size = unit(1, "lines"),  # Adjusts the size of the legend key
    legend.text = element_text(size = 10), # Adjusts text size in the legend
    panel.border = element_rect(color = "black", fill = NA, linewidth = 0.7),  # Adds a boundary box around the grid
    panel.background = element_rect(fill = NA),  # Ensures background is not filled
    axis.ticks = element_line(color="black"), # Ensure tick marks are visible
    axis.ticks.length = unit(0.1, "cm") # Adjust tick mark length
  )
```

```{r 5_visualize_plot}
# plot showing the GAM smoother
p +
  geom_smooth(method = "gam", formula = y ~ s(x), se = TRUE, linewidth = 0.5, alpha = 0.4) +
  labs(
    subtitle = "From Ice Core Data and Mauna Loa Measurements",
    x = "Year (CE)"
    ) +
  theme(
    plot.subtitle = element_text(hjust = 0.5, size = 10, face = "italic"),
    legend.position = "top"
  )
```

- The plot demonstrates that \(CO_2\) levels have fluctuated naturally over centuries, as seen in the Ice Cores data, but have sharply increased in recent decades due to human activities, particularly in the recent common era, when measurements were more systematically recorded at Mauna Loa.

```{r 5_restrict_x-axis}
# Investigate the carbon dioxide levels from 1950 to 2018 by adding breaks of size 10 in the x-axis
p +
  geom_smooth(method = "gam", formula = y ~ s(x), se = TRUE, linewidth = 0.5, alpha = 0.4) +
  scale_x_continuous(limits = c(1950, 2018), breaks = seq(1950, 2020, by = 10)) + # Adjust x-axis scale for better readability
  labs(subtitle = "With restricted x-axis",
        x = "Year (CE)"
       ) +
  theme(
    plot.subtitle = element_text(hjust = 0.5, face = "italic"),
    legend.position = "top"
  )
```

- The above restricted x-axis plot shows that there is steady increase in \(CO_2\) concentration since 1950, with a bigger increase starting in the 1980s.  
- The plot provides a better visualization of the modern \(CO_2\) rise, particularly after the 1950s when data from Mauna Loa become available.

**Use a Logarithmic Scale for the X-Axis:**

Since the data spans a very large range (from 803,182 BCE to 2018 CE), using a logarithmic scale on the x-axis could make it easier to visualize the trends, especially for early years.

```{r 5_logarithmic_scale}
# plot using logarithmic scale to the base 10
p +
  scale_x_continuous(
    trans = "log10",
    labels = scales::label_comma()
    ) +
  labs(
    subtitle = "Logarithmic Scale Applied to x-axis for better Historical Trend Visualization",
     x = "Year (log scale)"
    ) +
  theme(
    plot.subtitle = element_text(hjust = 0.5, size = 10, face = "italic"),
    legend.position = "top"
  )
```

- A logarithmic x-axis  in the plot above help visualize historical \(CO_2\) levels more clearly over both ancient and recent periods, revealing natural long-term cycles and the rapid, human-driven increase in \(CO_2\) since industrialization.

**Facet by the source variable**
```{r 5_Facet_by_source}
# Create facets for each source 
p + 
  facet_wrap(~source) +
  scale_x_continuous(limits = c(-803182, NA), breaks = seq(-803182, 2020, by = 300000)) + # Adjust x-axis scale for better readability
  labs(
    subtitle = "Using Faceting on the source variable",
     x = "Year (CE)"
    ) +
  theme(
    plot.subtitle = element_text(hjust = 0.5, size = 10, face = "italic"),
    legend.position = "top"
  )
```

- The above faceting of the source variable confirms that the historical data is sourced from Ice Cores.

#### Problem 6
The dataset **research_funding_rates** (again in `dslabs`) contains information on gender-bias in Netherlands research funding (described in [van der Lee & Ellemers 2015](https://www.pnas.org/content/112/40/12349.abstract). 
Make a scatter plot of success rates for men vs. women, with a 1:1 line (i.e., a line where _x_ = _y_) superimposed on the data, and the points labelled by discipline (`geom_text`). 

```{r 6_load_libraries}
# load libraries
library(ggplot2)
library(dslabs)
library(ggrepel) # For better text label positioning
```
```{r 6_load_data}
# Load the research_funding_rates dataset
data("research_funding_rates")

# Data structure
str(research_funding_rates)
```
The research_funding_rates dataset contains `r nrow(research_funding_rates)` observations across `r ncol(research_funding_rates)` variables.

` The code below correctly generates a scatter plot of success rates for men versus women, with the 1:1 line superimposed and labels added for each discipline.

```{r 6_scatterplot_of_men_vs_women, fig.align = "center"}
# Create scatter plot of success rates for men vs. women
p <- ggplot(research_funding_rates, aes(x = success_rates_men, y = success_rates_women, label = discipline)) +
  geom_point(size = 2) +  # Plot the points
  geom_abline(intercept = 0, slope = 1, linetype = "dashed", color = "red") +  # Add 1:1 line
  geom_text_repel(size = 3, box.padding = 0.3, max.overlaps = 10) + # Use geom_text_repel for better labelling
  coord_cartesian(x = c(0, 30), y = c(0, 30)) + # Ensure axes starts from 0 without cutting data
  labs(
    title = "Research Funding Success Rates: Men vs. Women",
    x = "Success Rate for Men (%)",
    y = "Success Rate for Women (%)"
  ) +
  theme_set(theme_minimal()) +
  theme(
    panel.grid.major = element_blank(), # Remove major grid lines
    panel.grid.minor = element_blank(), # Remove minor grid lines
    axis.line = element_line(color = "black", linewidth = 0.5), # Keep x-axis and y-axis
    axis.ticks = element_line(color = "black", linewidth = 1), # Add axis ticks
    axis.ticks.length = unit(0.1, "cm"),  # Adjust tick length
    plot.title = element_text(hjust = 0.5, size = 14, face = "bold") # Center-align title
  )
```

```{r 6_visualize_plot}
# plot the graph
plot(p)
```

In the above plot:

- Points above the 1:1 (x = y) red line indicates that in those disciplines, women have a higher success rate than men.  
- Points below the 1:1 (x = y) red line shows men have a higher success rate than women in those disciplines.  
- Success rates for men and women are almost identical for chemical sciences discipline with the men's slightly higher. 

#### Problem 7
Open the **gapminder** data frame from `dslabs.` Make a scatterplot of _fertility_ (y) against _life_expectancy_ (x) with the points coloured by the _year_ variable.  Facet the fertility-life_expectancy figure by continent and add a smoother to each facet.  

```{r 7_load_libraries}
# load libraries
library(dslabs)
library(ggplot2)
```

```{r 7_load_data}
# Load the gapminder dataset
data("gapminder")

# view first few lines
head(gapminder)
```
```{r 7_data_structure}
# print the structure of the dataset
str(gapminder)
```
The gapminder dataset contains `r nrow(gapminder)` observations across `r ncol(gapminder)` variables. Three of these features are of data type Factor, five features are of data type numeric and one is of data type integer.

```{r 7_investigate_missing_values}
# Find the number of rows with missing values in columns: fertility and life_expectancy
length(gapminder[is.na(gapminder$fertility) | is.na(gapminder$life_expectancy), ])
```

```{r 7_remove_missing_value_rows}
# Delete the observations with missing values
gapminder <- gapminder[!is.na(gapminder$fertility) & !is.na(gapminder$life_expectancy), ]

# check the data structure
str(gapminder)
```

```{r 7_create_Scatterplot, fig.align = "center"}
# Scatterplot of fertility vs. life expectancy, with year as color
p <- ggplot(gapminder, aes(x = life_expectancy, y = fertility, color = year)) +
  geom_point(alpha = 0.7) + # Scatter points with some transparency
  scale_color_gradient(low = "navyblue", high = "lightblue") + # Gradient color scale for the year
  labs(title = "Fertility vs. Life Expectancy by Continent",
       x = "Life Expectancy (years)",
       y = "Fertility Rate (children / women)",
       color = "Year") +
  theme_set(theme_minimal()) + # Minimal theme for clarity
  theme(
    panel.border = element_rect(fill = NA, linewidth = 0.2),  # Adds a boundary box around the grid
    axis.ticks = element_line(),
    axis.ticks.length = unit(0.1, "cm"),
    plot.title = element_text(hjust = 0.5, face = "bold")
  )
```

```{r 7_Visualize_the_plot}
# plot the graph
plot(p)
```

- The above plot shows a negative relationship, with countries in earlier years having higher fertility and lower life expectancy, while more recent years shows lower fertility and higher life expectancy.

```{r 7_facet_the_scatterplot, fig.align = 'center'}
# Facet the scatterplot by continent and add a smoother
p +
  facet_wrap(~ continent) + # Split the plot by continent
  geom_smooth(method = "gam", formula = y ~ s(x, bs = "cs"), se = TRUE, color = "red", alpha = 0.3, fill = "darkgray") + # Add GAM smoother with confidence interval
  theme_minimal() + # Clean minimalistic theme
  theme(
    panel.border = element_rect(color = "gray", fill = NA, linewidth = 1),  # Border for each facet
    strip.background = element_rect(fill = "gray", color="gray"), # Background color for facet labels
    axis.ticks = element_line(color = "gray"),
    axis.ticks.length = unit(0.1, "cm")
  )
```

The plot above shows the following trends:  

 - Across all continents, from 1960 to 2010, life expectency increased while fertility rate decreased.  
 - All continents display a clear downward trend in fertility over time, with lower fertility rates and higher life expectancy.  
 - Europe continues to maintain high life expectency and low fertility rates.  

#### 8. 
Finally, visualise the relationship between [divorce rates in Maine and the consumption (_per capita_) of margarine in the US](http://www.tylervigen.com/spurious-correlations).  
These data are in the **divorce_margarine** data set in `dslabs`.  Produce a scatter plot showing the relationship between margarine consumption and divorce rate in Maine and add a linear smoother.  Add the correlation coefficient between the two variables to the plot (hint: `annotate`).  Any comments on this relationship?

```{r 8_load_libraries}
# Load necessary libraries
library(dslabs)
library(ggplot2)
```

```{r 8_load_data}
# Load the dataset
data(divorce_margarine)

# check first few lines
head(divorce_margarine)
```
- There are three columns in this dataset.

```{r 8_data_structue}
# print the data structure
str(divorce_margarine)
```
The divorce_margarine dataset contains `r nrow(divorce_margarine)` observations across `r ncol(divorce_margarine)` variables. Two variables are of data types numeric and one variable is of data type integer.

```{r 8_correlation_coefficient}
# Compute correlation coefficient
cor_coeff <- cor(divorce_margarine$margarine_consumption_per_capita, divorce_margarine$divorce_rate_maine)

cor_coeff
```

```{r 8_create_scatterplot, fig.align = "center"}
# Create scatterplot with linear smoother and correlation coefficient
p <- ggplot(divorce_margarine, aes(x = margarine_consumption_per_capita, y = divorce_rate_maine)) +
  geom_point(color = "black", size=3) + # Larger points for better visibility
  geom_smooth(method = "lm", formula = y~x, color = "blue", se = TRUE) + # Linear regression line
  annotate(
    "text", 
    x = min(divorce_margarine$margarine_consumption_per_capita) + 0.8,
    y = max(divorce_margarine$divorce_rate_maine) - 0.1,
    label = paste("Correlation coefficient:", round(cor_coeff, 3)),
    size = 4,
    color = "black"
    ) +
  labs(
    title = "Divorce Rate in Maine vs. Margarine Consumption (per Capita)",
    subtitle = "A classic example of spurious correlation",
    x = "Margarine Consumption (kg per capita)",
    y = "Divorce Rate in Maine (per 1000 people)"
    ) +
  theme_set(theme_minimal()) +
  theme(
    panel.border = element_rect(color="black", fill=NA, linewidth = 1), # Adds a border to the entire plot
    axis.ticks = element_line(),
    axis.ticks.length = unit(0.1, "cm"),
    plot.title = element_text(hjust = 0.5, face = "bold"),
    plot.subtitle = element_text(hjust = 0.5, size = 10, face = "italic")
    )
```

```{r 8_visualize_plot}
# plot the graph
plot(p)
```

- The plot highlights how correlation does not imply causation, as it is unlikely that margarine consumption directly affects divorce rates.
- The linear regression line and the correlation coefficient shows a strong numerical relationship, but this is not a causal relationship, and is a classic example of spurious correlation. It is unlikely that margarine consumption directly affects divorce rates.
- The relationship might exist due to coincidence or an underlying confounding factor.

## Conclusion
- This problem set explored environmental datasets using `dslabs`.
- We performed **descriptive statistics, visualizations, and coefficient calculations**.
- The findings highlight **trends in polling, \(CO_2\) levels, and spurious correlations**.
- Further analysis could involve **hypothesis testing**.