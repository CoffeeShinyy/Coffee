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
