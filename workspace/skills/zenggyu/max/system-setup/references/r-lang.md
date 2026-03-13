# R

## Official Documentation
- Installation guide: https://cran.r-project.org/bin/linux/ubuntu/
- Configuration guide: https://blog.zenggyu.com/posts/en/2018-06-02-setting-up-r/
- Troubleshooting: https://cran.r-project.org/manuals.html

## User Preferences

### Installation Method
- **Source**: Use official CRAN repository (not Universe)
- **Key**: Michael Rutter marutter_pubkey.asc
- **Repo format**: `deb [arch=amd64 signed-by=/etc/apt/trusted.gpg.d/marutter_pubkey.gpg] https://cloud.r-project.org/bin/linux/ubuntu <codename>-cran40/`

### Packages to Install
- `r-base` - Core R
- `r-base-dev` - Required for compiling packages from source

### System Dependencies (tidyverse)
```
libcurl4-openssl-dev libssl-dev libxml2-dev libfontconfig1-dev libharfbuzz-dev
libfribidi-dev libfreetype6-dev libpng-dev libtiff5-dev libjpeg-dev
```

### System Dependencies (Database)
```
unixodbc-dev libpq-dev libmariadb-dev
```

### System Dependencies (Other packages)
```
xclip wl-clipboard libv8-dev libgsl-dev libsodium-dev libmbedtls-dev cmake
```

## Definition of Functional
Running `R --version` returns exit code 0 and outputs version string.

## Post-Installation Configuration

### ~/.bashrc
```bash
alias R='R --no-save --no-restore-data'
```

### ~/.Rprofile
```r
options(repos = c(default = "https://cloud.r-project.org/"))

if (interactive()) {
  suppressMessages(require(devtools))
}
```

### Core R Packages to Install
```r
install.packages(c(
  # Meta
  "tidyverse", "tidymodels", "devtools",
  
  # ML: Tree-based
  "randomForest", "xgboost", "lightgbm", "bonsai",
  
  # ML: Deep learning
  "torch", "luz", "brulee", "tabnet",
  
  # Time series
  "tsibble", "fable", "feasts", "fabletools",
  
  # ML: Other
  "censored", "tidyclust", "embed", "themis", "textrecipes",
  "stacks", "probably", "earth", "e1071",
  
  # Viz
  "DT", "gt", "gganimate", "ggwordcloud", "ggraph",
  
  # DB
  "odbc", "DBI", "RPostgres", "RMariaDB",
  
  # Shiny
  "shiny", "bslib", "htmltools", "fontawesome", "crosstalk",
  "shinydashboard", "bsicons", "thematic",
  
  # AI
  "ellmer", "btw",
  
  # Misc
  "data.table", "dtplyr", "slider", "caTools", "bitops",
  "remotes", "bundle", "writexl", "quarto", "corrr",
  
  # Addins
  "miniUI", "datapasta", "markdown",
  
  # Other
  "reticulate", "httr2", "plumber2"
))

torch::install_torch()
```

### TinyTeX Setup
```r
tinytex::install_tinytex(
  repository = "http://mirrors.tuna.tsinghua.edu.cn/CTAN/",
  bundle = "TinyTeX"
)
tinytex::tlmgr_repo("http://mirrors.tuna.tsinghua.edu.cn/CTAN/")
tinytex::tlmgr_install("xecjk")
```

### RStudio Settings

**General:**
- Restore .RData into workspace at startup: **OFF**
- Save workspace to .RData on exit: **Never**
- Always save history: **OFF**

**Code → Editing:**
- Use native pipe operator `|>`: **ON**
- Soft-wrap R source files: **ON**

**Appearance:**
- Font: Ubuntu Mono, size 12
- Theme: Solarized Light

**R Markdown:**
- Show output inline for all R Markdown documents: **OFF**

**Keyboard Shortcuts:**
| Action | Shortcut |
|--------|----------|
| Redo | Ctrl+Y |
| Reindent Selection | Ctrl+Shift+I |
| Reformat Current Selection | Ctrl+Shift+F |
| Show Help for Current Function | Ctrl+E |
