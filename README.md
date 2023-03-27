# Cluster-Analysis
Cluster analysis is a useful technique in sales for grouping customers or products based on shared characteristics or behaviors. By identifying these groups, sales teams can develop targeted marketing campaigns and personalized sales approaches that are tailored to the needs and preferences of each customer group. In addition, cluster analysis can help sales teams to identify underperforming product categories and adjust pricing, features, or marketing strategies accordingly to improve sales performance and profitability.

# Code

**Determine the current working directory and change it**
```rscript
getwd()
setwd("C:/Users/aashi/Downloads")
```

**Load the dataset**
```rscript
sales_data <- read.csv("vgsales.csv")
```

**To clean the given dataset**
```rscript
library(janitor)
sales_data <- clean_names(sales_data)
sales_data
```

**Format data for use with km function**
```rscript
#NA_Sales - Sales in North America (in millions)
#EU_Sales - Sales in Europe (in millions)
#JP_Sales - Sales in Japan (in millions)

sales_km<- as.matrix(sales_data[, c("na_sales", "eu_sales", "jp_sales")])
```

**Calculate and plot WSS for a series of k values**
**within sum of squares**
```rscript
wss <- numeric(15)
for (k in 1:15) wss[k] <- sum(kmeans(sales_km, centers = k, nstart = 25)$withinss)
plot(1:15, wss, type = "b", xlab = "Number of Clusters", ylab = "Within Sum of Squares (WSS)")
```

**Generate and review the results of a k-means analysis with k=3**
```rscript
km <- kmeans(sales_km, 3, nstart = 25)
km
```

**Prepare cluster analysis data for plotting**
```rscript
library(ggplot2)
library(grid)
library(gridExtra)
plotdata <- sales_data[, c("na_sales", "eu_sales", "jp_sales")]
plotdata$cluster <- km$cluster
centers <- as.data.frame(km$centers)
```

**Plot the data**
```rscript
g1 <- ggplot(data = plotdata, aes(x = na_sales, y = jp_sales, color = cluster)) +
  geom_point() +
  theme(legend.position = "right") +
  geom_point(data = centers,
             aes(x = na_sales, y = jp_sales, color = c(1, 2, 3)),
             size = 10, alpha = 0.3, show.legend = FALSE)
g1
```

## Output
![image](https://user-images.githubusercontent.com/85166438/227846490-21c13ff3-a653-4088-8aba-3a97283351c6.png)
![image](https://user-images.githubusercontent.com/85166438/227846546-645ffcc5-a517-4ec0-8bc2-06fa69788f60.png)

## Insights
Through the analysis of video game sales data, groups of customers who exhibit similar purchasing behavior or who prefer certain types of video games can be identified in various regions including North America, Europe, Japan, and other countries. This information can then be utilized to develop targeted marketing campaigns and customer retention strategies tailored to the preferences and behaviors of each customer group.

## Recommendations
K-means cluster analysis has been performed on video game sales data and has resulted in the identification of three clusters based on sales performance: Cluster 1.0 represents video games with low sales, Cluster 2.0 represents video games with medium sales, and Cluster 3.0 represents video games with high sales. This information could be used to inform marketing and sales strategies, such as increasing advertising efforts for games in Cluster 1.0, optimizing pricing strategies for games in Cluster 2.0, and focusing on maintaining customer loyalty for games in Cluster 3.0.












