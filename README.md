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
sales_data <- read.csv("Grocerysales.csv")
```

**To clean the given dataset**
```rscript
install.packages("janitor")
library(janitor)
sales_data <- clean_names(sales_data)
```

**Replace NAs with 0s and fix category names**
```rscript
sales_data$category[sales_data$category == "beverage"] <- "Beverages"
sales_data$category[sales_data$category == "bakery"] <- "Bakery"
sales_data$category[sales_data$category == "Eggs, Meat, Fish"] <- "Eggs, Meat & Fish"
sales_data[is.na(sales_data)] <- 0
```

**Replacing 0s as Offline and 1s as Online**
```rscript
sales_data$online_purchase[sales_data$online_purchase == 1] <- "Online"
sales_data$instore_purchase[sales_data$instore_purchase == 1] <- "Instore"
sales_data <- sales_data %>%
  mutate(Channel = ifelse(online_purchase == "Online","Online","Instore"))
sales_data <- sales_data[,c(-4,-5)]
sales_data <- sales_data[,c(1,2,3,10,4,5,6,7,8,9)]
sales_data
```

**Check for NAs in the cleaned dataset**
```rscript
sum(is.na(sales_data))
```


**Format data for use with km function**
```rscript
sales_km<- as.matrix(sales_data[, c("central", "east", "north", "south", "west", "total_sales")])
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
km <- kmeans(sales_km, 3, nstart = 1)
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
g1 <- ggplot(data = plotdata, aes(x = central, y = total_sales, color = as.factor(cluster))) +
  geom_point() +
  theme(legend.position = "right") +
  geom_point(data = centers,
             aes(x = east, y = total_sales, color = as.factor(1:3)),
             size = 10, alpha = 0.3, show.legend = FALSE)
g1 + labs(title = "Sales Clusters",
       subtitle = "K-means clustering of sales data by region") +
  xlab("Central") +
  ylab("Total Sales")
```

**Export the output**
```rscript
write.csv(sales_data, file = "data.csv")
```

## Output
![image](https://user-images.githubusercontent.com/85166438/228749714-1fdd0243-4b36-4f07-a92c-6f0eb69ae194.png)

![image](https://user-images.githubusercontent.com/85166438/228749684-125d991c-bd5a-4e0c-86dc-52b62ccb7b17.png)

## Insights
Through the analysis of grocery sales data, groups of customers who exhibit similar purchasing behavior or who prefer certain types of products can be identified in various regions including Central, East, North, South and West . This information can then be utilized to develop targeted marketing campaigns and customer retention strategies tailored to the preferences and behaviors of each customer group.

## Recommendations
K-means cluster analysis has been performed on Grocery sales data and has resulted in the identification of three clusters based on sales performance: Cluster 3.0 represents product items with low sales, Cluster 2.0 represents product items with medium sales, and Cluster 1.0 represents product items with high sales. This information could be used to inform marketing and sales strategies, such as increasing advertising efforts for products in Cluster 3.0, optimizing pricing strategies for products in Cluster 2.0, and focusing on maintaining customer loyalty for games in Cluster 1.0.












