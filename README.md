# shiny.info <a href="https://appsilon.github.io/shiny.info/"><img src="man/figures/logo.png" align="right" alt="Rhino logo" style="height: 140px;"></a>

> _Display simple information of the [shiny](https://shiny.rstudio.com) project in the user interface of the app._

<!-- badges: start -->
[![CRAN
status](https://www.r-pkg.org/badges/version/shiny.info)](https://cran.r-project.org/package=shiny.info)
[![R build
status](https://github.com/Appsilon/shiny.info/workflows/R-CMD-check/badge.svg)](https://github.com/Appsilon/shiny.info/actions?workflow=R-CMD-check)
[![Codecov test
coverage](https://codecov.io/gh/Appsilon/shiny.info/branch/master/graph/badge.svg)](https://codecov.io/gh/Appsilon/shiny.info?branch=master)
<!-- badges: end -->
## How to install shiny.info?

You can install shiny.info from CRAN repository:

```r
install.packages("shiny.info")
```

You can get the most recent version from this repo using
[remotes](https://github.com/r-lib/remotes).

```r
remotes::install_github("Appsilon/shiny.info")
```

## How to use shiny.info?

Just add one of the `shiny.info` functions to the UI of your app (some
features require also adding a little bit of code to the server
function). Check [features section](#basic-features) and
[documentation](https://cran.r-project.org/web/packages/shiny.info/shiny.info.pdf)
for more details.

<h3><a href="https://connect.appsilon.com/shiny_info_demo/">See live demo.</a></h3>

An example of a shiny app that uses `shiny.info` can be found in
`./examples` directory.

![](man/figures/example.gif)

## Basic features

- display a simple text message:

```r
shiny.info::display("Hello user!", position = "top right")
```

![](man/figures/display.png)

- show information about git branch, commit and changes:

```r
shiny.info::git_info()
```

![](man/figures/git.png)

- add “powered by” information with link:

```r
shiny.info::powered_by("Appsilon", link = "appsilon.com")
```

![](man/figures/powered.png)

- show app version:

```r
# global variable:
VERSION <- "1.2.2"

# in app ui
shiny.info::version()
```

![](man/figures/version.png)

- show a busy spinner when app is calculating:

```r
shiny.info::busy()
```

![](man/figures/busy.gif)

- group multiple messages in one panel:

```r
shiny.info::info_panel(
    shiny.info::git_info(),
    shiny.info::powered_by("Appsilon", link = "appsilon.com"),
    position = "bottom left"
)
```

![](man/figures/panel.png)

## Advanced features

- render value (eg. input, reactive value) from the server:

```r
# in app ui
shiny.info::info_value("test_info_value")

# in app server
some_value <- reactiveVal("a test value to display")
output$test_info_value <- shiny.info::render_info_value(some_value())
```

![](man/figures/info_value.png)

- render information about the session:

```r
# in app ui
shiny.info::info_value("session_info_value")

# in app server
output$session_info_value <- shiny.info::render_session_info()
```

![](man/figures/session.png)

- debug app using `browser()` function just by clicking a button:

```r
# in app ui
shiny.info::inspect_btn_ui()

# in app server
shiny.info::inspect_btn_server(input)
```

![](man/figures/inspect_button.png)

- toggle display with a key shortcut:

```r
shiny.info::toggle_info("Ctrl+Shift+K")
```

![](man/figures/shortcut.gif)

- show custom message using global variables:

```r
# in app global
VERSION = "1.2.2"
REPO = git2r::repository_head(repository("."))[[1]]
GIT_COMMIT_MESSAGE = git2r::commits(repository("."))[[1]]$message
GIT_COMMIT_HASH = git2r::commits(repository("."))[[1]]$sha

# in app ui
shiny.info::display(
    message = glue("I am running on repository {REPO}
    from [{GIT_COMMIT_HASH}]: {GIT_COMMIT_MESSAGE},
    and this is version: {VERSION}"),
    position = "top right",
    type = "custom_message"
)
```

![](man/figures/global_variables_custom_message.png)

- show custom message using reactive variables:

```r
# in app ui
shiny.info::info_value("test_info_value", position = "top right")

# in app server
a <- reactive({
  input$xcol
  rnorm(1,1)
})

output$test_info_value <- shiny.info::render_info_value(
  glue("a: {a()}, X Variable: {input$xcol}"),
  add_name = FALSE
)
```

![](man/figures/reactive_variables_custom_message.png)

## How can I contribute?

If you want to contribute to this project please submit a regular PR
once you’re done with your new feature or bug fix.

---

Developed at [Appsilon](https://appsilon.com).
Get in touch: <opensource@appsilon.com>.

Appsilon is a
[**Full Service Certified RStudio Partner**](https://www.rstudio.com/certified-partners/).

<a href = "https://appsilon.com/careers/" target="_blank"><img src="http://d2v95fjda94ghc.cloudfront.net/hiring.png" alt="We are hiring!"/></a>
