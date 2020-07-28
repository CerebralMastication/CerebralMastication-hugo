
knitr::knit_hooks$set(class = function(before, options, envir) {
  if(before){
    sprintf("<div class = '%s'>", options$class)
  }else{
    "</div>"
  }
})

# setting options(knitr.graphics.error = FALSE) in order to make knitr stop throwing errors in hugo blog posts with old style image links

options(knitr.graphics.error = FALSE) 
