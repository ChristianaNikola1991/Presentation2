library(shiny)
library(readxl)

runApp(
  list(
    ui = fluidPage(
      titlePanel("Use readxl"),
      sidebarLayout(
        sidebarPanel(
          fileInput('file1', 'Choose xlsx file',
                    accept = c(".xlsx")
          )
        ),
        mainPanel(
          tableOutput('contents'))
      )
    ),
    server = function(input, output){
      output$contents <- renderTable({
        inFile <- input$file1
        
        if(is.null(inFile))
          return(NULL)
        file.rename(inFile$datapath,
                    paste(inFile$datapath, ".xlsx", sep=""))
        read_excel(paste(inFile$datapath, ".xlsx", sep=""), 1)
      })
    }
  )
)
