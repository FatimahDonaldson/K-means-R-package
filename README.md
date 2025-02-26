# K-means-R-package
library(dplyr)
library(shiny)
library(dslabs)
data(murders)
murders<-mutate(murders, rate=total/(population/100000))
murders<-mutate(murders, rank=rank(-rate))
murder<-select(murders, region, rank)

ui <- fluidPage(
  
  # Put a titlePanel here
  titlePanel("k-means clustering"),
  
  sidebarLayout(
    # Sidebar. Put your inputs inside the sidebarPanel
    sidebarPanel(
      selectInput('xcol', 'X Variable', names(murder)),
      selectInput('ycol', 'Y Variable', names(murder),
                  selected=names(murder)[[2]]),
      numericInput('clusters', 'Cluster count', 4,
                   min = 1, max = 9)
    ),
    
    # Main panel. put your output plot here
    mainPanel(
      plotOutput('plot1')
    )
  )
)

server <- function(input, output, session) {
  output$plot1 <- renderPlot({
    plot(murders[,c(input$xcol,input$ycol)])
  })
  
}

shinyApp(ui = ui, server = server)