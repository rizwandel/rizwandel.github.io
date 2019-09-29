DDP_Project
========================================================
author: Mohamed Rizwan
date: 27/09/2019
autosize: true

Description of data set
========================================================

- The data give the speed of cars and the distances taken to stop, recorded in the 1920s.
- A data frame with 50 observations on 2 variables.
- speed	 numeric	 Speed (mph)
- dist	 numeric	 Stopping distance (ft)

ShinyApp description
========================================================
- This shinyapp is built to predict the stopping distance of cars at different speeds
- lm(dist~speed,cars) is used to fit the model,with p value less than 5%
- predict function used to predict the stopping distance using the above model
- plot shows the predicted values of distance on the regression line

ui.R code
========================================================


```r
library(datasets)
library(shiny)

  ui = fluidPage(
  titlePanel("stopping distance of cars"),
  sidebarLayout(
    sidebarPanel(
       h4("choose the speed of the car"),
       sliderInput("speed",label="speed",min = min(cars$speed),max =max(cars$speed),5,1)
       ),
        mainPanel(
       plotOutput("Plot1"),
       h4("Distance to stop"),
       verbatimTextOutput("Pred1")
       )
    )
)
```
  
server.R code
========================================================
 
 ```r
 server = function(input, output) {
        fit <- lm(dist~speed, cars)
        Pred1 <- reactive({
        speed <- input$speed
        predict(fit, newdata=data.frame(speed=input$speed))
        })
    output$Plot1 <- renderPlot({
        speed <- input$speed
        plot(cars$speed,cars$dist, xlab="speed",ylab="distance",bty="n", pch=20)
        abline(fit,col="red",lwd=1)
        points(speed,Pred1(), col='blue',pch= 16,cex=3)
    })
    output$Pred1 <- renderText({
        Pred1()
    })
 }
 ```

Plot and a link to interactive shinyApp
========================================================
![plot of chunk unnamed-chunk-3](DDP_Project-figure/unnamed-chunk-3-1.png)
https://rizwandatascientist.shinyapps.io/Shinyapp_StoppingDistance/
