Course Project - Visualing Data and Creating Graphs with Shiny
========================================================
author: Partha S. Ghose
date: "5/6/2017"
autosize: true


Introduction
=========================================================

The present project deals with exploring patterns in datasets using graph built on ggplot2 platform. For this a drop down list was created using five datasets. These datasets are part of a drop down list. The user can select the data from this list and visualise the data table, its structure and the summary stats. Finally they can explore the data patterns through graphs. All the information are presented in seperate tabs.


Slide With Code
========================================================
Five datasets were used -

- rock (Measurements on Petroleum Rock Samples)
- Iris (Edgar Anderson's Iris Data)
- cars (Speed and Stopping Distances of Cars)
- diamonds (Prices of 50,000 round cut diamond)
- mpg (Fuel economy data from 1999 and 2008 for 38 popular models of car)

First three are part of R package "datasets" and the last two are part of "ggplot2" package of R



ui.R Code
========================================================


```r
library(shiny)
library(ggplot2)
library(datasets)

shinyUI(fluidPage(
  
 tags$style("body{background-color: white}; margin:0; padding:0"),
 tags$style(".container-fluid{background-color: #084C76; width: 1100px; margin:0 auto; padding:0}"),
 tags$style("h2{color: white;left-margin:0; padding-left:20px}"),
 tags$style("p{color: black;left-margin:0; padding-left:20px; width:700px}"),
 tags$style(".col-sm-8{background-color: #6DD0FB; }"),
 tags$style(".well{background-color: white; width: 300px; height: 1500px; marging: 0;padding:20}"),
 tags$style(".row{background-color: #6DD0FB; margin:0; padding:0}"),
  # APPLICATION HEADING
  #=====================
  titlePanel("Visualing Data and Creating Graphs with Shiny"),
  
  
 # SETTING THE SIDEBAR PANELS
 #==============================
  sidebarLayout(
    
    sidebarPanel(
      
      conditionalPanel(condition = "input.tabselected==1",
                       selectInput(inputId = "dataset", label = "Choose your dataset:", choices = c("rock", "iris", "cars","diamonds", "mpg")),
                       numericInput("obs", "Number of observations to view:", 10)
                      
      ),
      
      conditionalPanel(condition = "input.tabselected==2",
                       selectInput(inputId = "dataset", label = "Choose your dataset:", choices = c("rock", "iris", "cars","diamonds", "mpg")),
                       numericInput("obs", "Number of observations to view:", 10)
                      
      ),
      
      conditionalPanel(condition = "input.tabselected==3",
                       selectInput(inputId = "dataset", label = "Choose your dataset:", choices = c("rock", "iris", "cars","diamonds", "mpg")),
                       numericInput("obs", "Number of observations to view:", 10)
                    
      ),
      
      conditionalPanel(condition = "input.tabselected==4", uiOutput("vx"), uiOutput("vy"),
                       br(),
      
      # SELECTING THE PLOT TYPE
      #===============================
                       h4("Select Suitable Plot Type:"),
                       checkboxInput("box", "Boxplot"),
                       checkboxInput("scatter", "Scatter Plot"),
                       checkboxInput("line", "Line Plot"),
                       br(),
                       
      # DISPLAYING THE COLOUR DROP DOWN LIST FOR SELECTION
      #===================================================
                       
                       uiOutput("colours"),
                       
      # CHECKBOXES FOR JITTERS AND LOESS
      #==================================
                      
                       h4("Select:"),
                      checkboxInput("jitter", "Jitter"),
                      checkboxInput("smooth", "Smooth"),
      
      textInput("title","Enter Plot Title", value = ""),
      textInput("x","Enter X-Axis Label", value = ""),
      textInput("y","Enter Y-Axis Label", value = "")
      ),
      
      #APP DESCRIPTION
      #================
      h4("APP DESCRIPTION"),
      h6("This app allows the user to select the database from a dropdown list, visualise the data, its structure and summary. It also helps to explore the data patterns with graphs."),
      h5("APP By: Partha Sarathi Ghose")
    ),
    
    # DISPLAYING THE DATA AND PLOT
    #==============================
    mainPanel(
      tags$style(type="text/css",
                 ".shiny-output-error { visibility: hidden; }",
                 ".shiny-output-error:before { visibility: hidden; }"
      ),
      tabsetPanel(type = "tab",
        tabPanel("Data", value = 1, tableOutput("view")),
        tabPanel("Data Structure", value = 2, verbatimTextOutput("str")),
        tabPanel("Summary", value = 3, verbatimTextOutput("summary")),
        tabPanel("Plot", value = 4, plotOutput("p1")),
        id = "tabselected"
      ),
      br(),
h2("Documentation"),
h3("Datasets Used"),
p("Five datasets were used as parth of this app. These are - "),
h4("Measurements on Petroleum Rock Samples. Dataset - rock"),
p("Twelve core samples from petroleum reservoirs were sampled by 4 cross-sections. Each core sample was measured for permeability, and each cross-section has total area of pores, total perimeter of pores, and shape."),
p("Source: Data from BP Research, image analysis by Ronit Katz, U. Oxford."),
br(),
h4("Edgar Anderson's Iris Data. Dataset - Iris"),
p("This famous (Fisher's or Anderson's) iris data set gives the measurements in centimeters of the variables sepal length and width and petal length and width, respectively, for 50 flowers from each of 3 species of iris. The species are Iris setosa, versicolor, and virginica."),
p("References: Becker, R. A., Chambers, J. M. and Wilks, A. R. (1988) The New S Language. Wadsworth & Brooks/Cole. (has iris3 as iris.)"),
br(),
h4("Speed and Stopping Distances of Cars. Dataset - cars"),
p("The data give the speed of cars and the distances taken to stop. Note that the data were recorded in the 1920s."),
p("References: McNeil, D. R. (1977) Interactive Data Analysis. Wiley."),
br(),
h4("Prices of 50,000 round cut diamonds. Dataset - diamond (ggplot2)"),
p("A dataset containing the prices and other attributes of almost 54,000 diamonds. "),
p("The variables are as follows:price - price in US dollars; carat - weight of the diamond; cut - quality of the cut (Fair, Good, Very Good, Premium, Ideal); color - diamond colour; clarity - a measurement of how clear the diamond is; x - length in mm; y - width in mm; z - depth in mm; depth = total depth; table - width of top of diamond relative to widest point"),
br(),
h4("Fuel economy data from 1999 and 2008 for 38 popular models of car. Dataset - mpg (ggplot2)"),
p("This dataset contains a subset of the fuel economy data that the EPA makes available on http://fueleconomy.gov. It contains only models which had a new release every year between 1999 and 2008 - this was used as a proxy for the popularity of the car. "),
p("A data frame with 234 rows and 11 variables - manufacturer; model - model name; displ - engine displacement, in litres; year - year of manufacture; cyl - number of cylinders; trans - type of transmission; drv - f = front-wheel drive, r = rear wheel drive, 4 = 4wd; cty - city miles per gallon; hwy - highway miles per gallon; fl - fuel type; class - type of car"),
br(),
h3("Online Resources and Data Studied and Adopted"),
tags$a(href="http://shiny.rstudio.com/gallery/plot-plus-three-columns.html", "Plot plus three columns"),
br(),
tags$a(href="http://shiny.rstudio.com/gallery/", "Shiny Gallary"),
br(),
tags$a(href="https://www.youtube.com/watch?v=_0ORRJqctHE&list=PL6wLL_RojB5xNOhe2OTSd-DPkMLVY9DfB", "R Shiny app tutorial"),
br(),
tags$a(href="https://shiny.rstudio.com/tutorial/lesson1/", "Shiny - The written tutorial"),

br(),
br()

    )
)))
```

server.R Code
========================================================

```r
library(shiny)
library(ggplot2)
library(datasets)

# DEFINING THE SERVER LOGIC FOR THE DATA
shinyServer(function(input, output) {
  #READING THE DATA
  datasetInput <- reactive({
    switch(input$dataset,
           "rock" = rock,
           "iris" = iris,
           "cars" = cars,
           "diamonds" = diamonds,
           "mpg" = mpg)
  })
  
  # DISPLAYING THE DATASETS
  #===================================
  output$view <- renderTable({
    head(datasetInput(), n = input$obs)
  })
  
  # DISPLAYING STRUCTURE OF THE DATASETS
  #===================================
  output$str <- renderPrint({
    dataset <- datasetInput()
    str(dataset)
  })
  
  
  # GENERATING THE SUMMARY OF DATASETS
  #===================================
  output$summary <- renderPrint({
  dataset <- datasetInput()
  summary(dataset)
  })
  
  # CODE TO DISPLAY DROP DOWN LISTS DYNAMICALLY
  #=============================================
  output$vx <- renderUI({
    selectInput("varix","Select X axis variable", choices = names(datasetInput()))
  })
  output$vy <- renderUI({
    selectInput("variy","Select Y axis variable", choices = names(datasetInput()))
  })
  
  output$colours <- renderUI({
    selectInput("colours","Select Suitable Colours Option", choices = c("None",names(datasetInput())))
  })
  
  # CODE FOR THE PLOT
  #======================
  
  output$p1 <- renderPlot({
    
   #attach(get(input$dataset))
   # plot(get(input$varix), get(input$variy), xlab = input$varix, ylab = input$variy, las = 1)
    
    #x <- get
    
    p1 <- ggplot(get(input$dataset), aes_string(x=input$varix, y=input$variy)) 
    
    if(input$box)
    p1 <- p1 + geom_boxplot()
    if(input$scatter)
      p1 <- p1 + geom_point()
    if(input$line)
      p1 <- p1 + geom_line()
    
    
 if (input$colours != "None")
     p1 <- p1 + aes_string(colour=input$colours)
 
if(input$x != "")
  p1 <- p1+labs(x = input$x)

if(input$y != "")
  p1 <- p1+labs(y = input$y)

if(input$title != "")
  p1 <- p1+labs(title = input$title)

  #  facets <- paste(input$facet_row, '~', input$facet_col)
   # if (facets != '. ~ .')
    #  p1 <- p1 + facet_grid(facets)
    
    if (input$jitter)
     p1 <- p1 + geom_jitter()
    
    if (input$smooth)
     p1 <- p1 + geom_smooth()
    
    print(p1)
    
  })
  
})
```

Thank You
===========================================
Visit Site - https://parthasarathighose.shinyapps.io/test_p1/