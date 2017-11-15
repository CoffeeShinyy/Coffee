library(shiny)

# Define UI ----
ui <- fluidPage(
  titlePanel("Coffee!!!"),
  
  sidebarLayout(
    sidebarPanel(
      
      selectInput("Meals",
                  label = "Meals?",
                  choices = list("Yes", 
                                 "No",
                                 "No Preference"),
                  selected = "Yes"),
      
      selectInput("Parking", 
                  label = "Parking?",
                  choices = list("Yes", 
                                 "No",
                                 "No Preference"),
                  selected = "Yes"),
      
      selectInput("Outdoor Seating",
                  label = "Outdoor Seating?",
                  choices = list("Yes", 
                                 "No",
                                 "No Preference"),
                  selected = "Yes")
    ), 
    
    mainPanel()
  )
)

# Define server logic ----
server <- function(input, output) {
  
}

# Run the app ----
shinyApp(ui = ui, server = server)


library(tidyverse)
#------------------------------------------------------------------------------#
# Read in the coffee data
#------------------------------------------------------------------------------#

coffee_data <- read_csv("coffee.csv")

shinyServer(
  function(input, output) {
    
    # Filter data based on selections
    output$table <- DT::renderDataTable(DT::datatable({
      data <- coffee_data
      if (input$meals != "No Preference") {
        data <- data[data$meals == input$meals,]
      }
      if (input$parking != "No Preference") {
        data <- data[data$parking == input$parking,]
      }
      if (input$outdoor != "No Preference") {
        data <- data[data$outdoor == input$outdoor,]
      }
      data
    }))
    
  }
)
