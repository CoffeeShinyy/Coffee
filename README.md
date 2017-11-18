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
                  selected = "Yes"),
      
      selectInput("days",
                  label = "Days?",
                  choices = list("Monday",
                                 "Tuesday",
                                 "Wednesday",
                                 "Thursday",
                                 "Friday", 
                                 "Saturday",
                                 "Sunday")),
      sliderInput("hours",
                  label = "Hours?",
                  min = 0, max = 24, value = c(7, 20), step = 0.5),
      
      sliderInput("distance",
                  label = "Distance?",
                  min = 0, max = 10, value = 3, step = 0.5)
      
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
    ifelse(input$meals != "No Preference",
           data <- data,
           data <- data[data$meals == input$meals,])
    ifelse(input$parking != "No Preference",
           data <- data,
           data <- data[data$parking == input$parking,])
    ifelse(input$outdoor != "No Preference",
           data <- data,
           data <- data[data$outdoor == input$outdoor,])
    
    if (input$days == "Monday"){
      data <- data %>%select(name, meals, parking, outdoor, Monday_open, Monday_closed, walking_distance)
      data <- data %>% filter(data$Monday_open >= input$hours[1], data$Monday_closed <= input$hours[2])
      }
    if (input$days == "Tuesday"){
      data <- data %>% select(name, meals, parking, outdoor, Tuesday_open, Tuesday_closed, walking_distance)
      data <- data %>% filter(data$Tuesday_open >= input$hours[1], data$Tuesday_closed <= input$hours[2])}
    if (input$days == "Wednesday"){
      data <- data %>%select(name, meals, parking, outdoor, Wednesday_open, Wednesday_closed, walking_distance)
      data <- data %>% filter(data$Wednesday_open >= input$hours[1], data$Wednesday_closed <= input$hours[2])}
    if (input$days == "Thursday"){
      data <- data %>%select(name, meals, parking, outdoor, Thursday_open,Thursday_closed, walking_distance)
      data <- data %>% filter(data$Thursday_open >= input$hours[1], data$Thursday_closed <= input$hours[2])}
    if (input$days == "Friday"){
      data <- data %>%select(name, meals, parking, outdoor, Friday_open, Friday_closed,  walking_distance)
      data <- data %>% filter(data$Friday_open >= input$hours[1], data$Friday_closed <= input$hours[2])}
    if (input$days == "Saturday"){
      data <- data %>%select(name, meals, parking, outdoor, Saturday_open, Saturday_closed,  walking_distance)
      data <- data %>% filter(data$Saturday_open >= input$hours[1], data$Saturday_closed <= input$hours[2])}
    if (input$days == "Sunday"){
      data <- data %>%select(name, meals, parking, outdoor,Sunday_open, Sunday_closed,  walking_distance)
      data <- data %>% filter(data$Sunday_open >= input$hours[1], data$Sunday_closed <= input$hours[2])}
    
    data <- data %>% filter(data$walking_distance <= input$distance)
    data
    }))
  
}

# Run the app ----
shinyApp(ui = ui, server = server)
