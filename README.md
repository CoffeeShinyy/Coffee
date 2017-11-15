library(shiny)
library(tidyverse)

# Define UI ----
ui <- fluidPage(
  titlePanel("Coffee!!!"),
  
  sidebarLayout(
    sidebarPanel(
      
      selectInput("meals",
                  label = "Meals?",
                  choices = list("Yes", 
                                 "No",
                                 "No Preferences"),
                  selected = "Yes"),
      
      selectInput("parking", 
                  label = "Parking?",
                  choices = list("Yes", 
                                 "No",
                                 "No Preferences"),
                  selected = "Yes"),
      
      selectInput("outdoor",
                  label = "Outdoor Seating?",
                  choices = list("Yes", 
                                 "No",
                                 "No Preferences"),
                  selected = "Yes")
    ), 
    
    mainPanel(
      fluidRow(
        DT::dataTableOutput("table")
    )
  )
  )
)



coffee_data <- read_csv("coffee.csv")

# Define server logic ----
server <- function(input, output) {
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

# Run the app ----
shinyApp(ui = ui, server = server)
