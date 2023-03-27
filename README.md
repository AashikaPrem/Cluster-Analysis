# Cluster-Analysis
Cluster analysis is a useful technique in sales for grouping customers or products based on shared characteristics or behaviors. By identifying these groups, sales teams can develop targeted marketing campaigns and personalized sales approaches that are tailored to the needs and preferences of each customer group. In addition, cluster analysis can help sales teams to identify underperforming product categories and adjust pricing, features, or marketing strategies accordingly to improve sales performance and profitability.

# Code

**Determine the current working directory and change it**
getwd()
setwd("C:/Users/aashi/Downloads")

**Load the dataset**
sales_data <- read.csv("vgsales.csv")

**To clean the given dataset**
library(janitor)
sales_data <- clean_names(sales_data)
sales_data

**Format data for use with km function**
#NA_Sales - Sales in North America (in millions)
#EU_Sales - Sales in Europe (in millions)
#JP_Sales - Sales in Japan (in millions)

sales_km<- as.matrix(sales_data[, c("na_sales", "eu_sales", "jp_sales")])

**Calculate and plot WSS for a series of k values**
**within sum of squares**
wss <- numeric(15)
for (k in 1:15) wss[k] <- sum(kmeans(sales_km, centers = k, nstart = 25)$withinss)
plot(1:15, wss, type = "b", xlab = "Number of Clusters", ylab = "Within Sum of Squares (WSS)")

**Generate and review the results of a k-means analysis with k=3**
km <- kmeans(sales_km, 3, nstart = 25)
km

**Prepare cluster analysis data for plotting**
library(ggplot2)
library(grid)
library(gridExtra)
plotdata <- sales_data[, c("na_sales", "eu_sales", "jp_sales")]
plotdata$cluster <- km$cluster
centers <- as.data.frame(km$centers)

**Plot the data**
g1 <- ggplot(data = plotdata, aes(x = na_sales, y = jp_sales, color = cluster)) +
  geom_point() +
  theme(legend.position = "right") +
  geom_point(data = centers,
             aes(x = na_sales, y = jp_sales, color = c(1, 2, 3)),
             size = 10, alpha = 0.3, show.legend = FALSE)
g1











