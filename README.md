# Coffee

library(shiny)

# Define UI ----
ui <- fluidPage(
  titlePanel("censusVis"),
  
  sidebarLayout(
    sidebarPanel(
      
      selectInput("var", 
                  label = "Choose a variable to display",
                  choices = list("Monday", 
                                 "Tuesday",
                                 "Wednesday", 
                                 "Thursday",
                                 "Friday",
                                 "Saturday",
                                 "Sunday"),
                  selected = "Monday"),
      
      sliderInput("range", 
                  label = "Open Hour",
                  min = 0, max = 2400, value = c(0, 1000))
      ),
    
    mainPanel()
  )
  )

# Define server logic ----
server <- function(input, output) {
  
}

# Run the app ----
shinyApp(ui = ui, server = server)
