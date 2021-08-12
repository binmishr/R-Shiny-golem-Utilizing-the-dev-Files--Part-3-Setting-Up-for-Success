# R-Shiny-golem-Utilizing-the-dev-Files--Part-3-Setting-Up-for-Success

Of the dev files, 02_dev.R may be considered the most important since we will continuously go back to it while we build the app. For example, with usethis::use_package() in 02_dev.R, we can enter a package name and it will be automatically documented in our description file. This is a vital step because the Shiny app will not work if this is not done for every package used in development. Golem also has recommended package dependencies which can be added with golem::use_recommended_deps() and is located in the 01_start.R file.

With golem::add_module( name = “name_of_module1” ), an r script is created in the R folder that includes a server and ui section. Modules are great because they allow us to break apart a messy and lengthy ui/server, resulting in clean and distinct code. Based on post #2, we will create a module for each of the tabs in our app. When naming modules, as well as functions, it’s key to have concise and consistent naming conventions. For instance, we will initiate the sales overview and ratings overview pages with:

golem::add_module( name = "sales_overview" )
golem::add_module( name = "ratings_overview" )

These naming conventions make our files easily distinguishable and orderly. Additionally, this method will cascade down to functions, helpers, utils and more. For instance, if we want to create plot functions that only pertain to sales then we can initiate that file with:

golem::add_fct( "sales_plots" ) 

The modules that we just created contain a server and UI section. Most importantly, there is code generated at the very bottom that must be inserted into our main server and ui, app_server.R and app_ui.R . For example, we can see this at the bottom of our sales_overview mod:

## To be copied in the UI
# mod_sales_overview_ui("sales_overview_ui_1")
## To be copied in the server
# callModule(mod_sales_overview_server, "sales_overview_ui_1")

Based on our preliminary application design, we can begin to build our UI and simply put the callModule statements in app_server.R . We will be using the {shinydashboard} package to set up our tabs in the UI. Let’s go ahead and add that package with:

usethis::use_package( "shinydashboard" )

Check out the bottom of this post for all of the app_ui.R code. Note: this step can be tedious but only needs to be done once!

Once this step is complete we can run all of the lines in the run_dev.R file and we are greeted with the new structure of our app!
image1.png

Do not forget to put every single callModule in the app_server.R . If one is missing then the corresponding page/tab will be blank. This would be an issue when we actually have code in the mods but it’s easier to just get it out of the way now.

The key points to take away from this post are:

Document every package utilized with: usethis::use_package()

Modules organize our code

Naming conventions make files (and code) easily identifiable

Lastly, add every mod to app_server.R with callModule

The tips and tricks discussed in this post immediately set our project up for success. Stay tuned for our next post where we design more of the ui and begin looking at methods for tests. If you haven’t already, run golem::use_recommended_tests(), which is included in the 01_start.R file. This will create a folder where we can create and run tests on our application. See you next time!

    “If I don’t have some cake soon, I might die.” 

— Stanley Hudson

# Creating modules in 02_dev.R:
golem::add_module( name = "sales_overview" )
golem::add_module( name = "sales_seasons" )
golem::add_module( name = "ratings_overview" ) 
golem::add_module( name = "ratings_seasons" ) 
golem::add_module( name = "ratings_episodes" ) 
golem::add_module( name = "ratings_characters" )
golem::add_module( name = "ratings_writers" ) 
golem::add_module( name = "ratings_directors" ) 
golem::add_module( name = "script_analysis" ) 

# Integrating modules in app_ui.R:
app_ui <- function(request) {
  tagList(
    # Leave this function for adding external resources
    golem_add_external_resources(),
    # List the first level UI elements here
    shinydashboard::dashboardPage(
      skin = "black",
      header = shinydashboard::dashboardHeader(
        title = "The Office Analysis"
      ),
      
      shinydashboard::dashboardSidebar(
        shinydashboard::sidebarMenu(
          shinydashboard::menuItem("Sales Analysis", tabName = "sales_analysis", icon = icon("dashboard"),
                                   shinydashboard::menuSubItem("Overview", tabName = "sales_overview", icon = icon("dashboard")),
                                   shinydashboard::menuSubItem("Seasons", tabName = "sales_seasons", icon = icon("users-cog"))),
          
          shinydashboard::menuItem("Ratings Analysis", tabName = "ratings_analysis", icon = icon("dashboard"),
                                   shinydashboard::menuSubItem("Overview", tabName = "ratings_overview", icon = icon("dashboard")),
                                   shinydashboard::menuSubItem("Seasons", tabName = "ratings_seasons", icon = icon("dashboard")),
                                   shinydashboard::menuSubItem("Episodes", tabName = "ratings_episodes", icon = icon("dashboard")),
                                   shinydashboard::menuSubItem("Characters", tabName = "ratings_characters", icon = icon("dashboard")),
                                   shinydashboard::menuSubItem("Writers", tabName = "ratings_writers", icon = icon("dashboard")),
                                   shinydashboard::menuSubItem("Directors", tabName = "ratings_directors", icon = icon("dashboard"))),
          shinydashboard::menuItem("Script Analysis", tabName = "script_analysis", icon = icon("dashboard"))
        )
      ),
      
      shinydashboard::dashboardBody(
        
        shinydashboard::tabItems(
          shinydashboard::tabItem("sales_overview", mod_sales_overview_ui("sales_overview_ui_1")),
          shinydashboard::tabItem("sales_seasons", mod_sales_seasons_ui("sales_seasons_ui_1")),
          shinydashboard::tabItem("ratings_overview", mod_ratings_overview_ui("ratings_overview_ui_1")),
          shinydashboard::tabItem("ratings_seaons",   mod_ratings_seasons_ui("ratings_seasons_ui_1")),
          shinydashboard::tabItem("ratings_episodes",  mod_ratings_episodes_ui("ratings_episodes_ui_1")),
          shinydashboard::tabItem("ratings_characters",  mod_ratings_characters_ui("ratings_characters_ui_1")),
          shinydashboard::tabItem("ratings_writers",  mod_ratings_writers_ui("ratings_writers_ui_1")),
          shinydashboard::tabItem("ratings_directors",  mod_ratings_directors_ui("ratings_directors_ui_1")),
          shinydashboard::tabItem("script_analysis", mod_script_analysis_ui("script_analysis_ui_1"))
        )
      )
    )
  )
}


# Adding modules to app_server.R:
app_server <- function( input, output, session ) {
  # List the first level callModules here
  callModule(mod_sales_overview_server, "sales_overview_ui_1")
  callModule(mod_sales_seasons_server, "sales_seasons_ui_1")
  callModule(mod_ratings_overview_server, "ratings_overview_ui_1")
  callModule(mod_ratings_seasons_server, "ratings_seasons_ui_1")
  callModule(mod_ratings_episodes_server, "ratings_episodes_ui_1")
  callModule(mod_ratings_characters_server, "ratings_characters_ui_1")
  callModule(mod_ratings_writers_server, "ratings_writers_ui_1")
  callModule(mod_ratings_directors_server, "ratings_directors_ui_1")
  callModule(mod_script_analysis_server, "script_analysis_ui_1")
  
}
