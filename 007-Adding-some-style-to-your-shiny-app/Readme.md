# Adding some style to your shiny app

<style>
#wrap { width: 600px; height: 676px; padding: 0; overflow: hidden; }
#frame { width: 800px; height: 1040px; border: 0px solid black; }
#frame {
 -ms-zoom: 0.65;
        -moz-transform: scale(0.65);
        -moz-transform-origin: 0 0;
        -o-transform: scale(0.65);
        -o-transform-origin: 0 0;
        -webkit-transform: scale(0.65);
        -webkit-transform-origin: 0 0;

}
</style>

Fisrt of all, there are a good [post](http://shiny.rstudio.com/articles/css.html) in Rstudio's blog where they explain how add style in a shiny app. But that is only the beginning.

Time ago, searching for js libraries and I found [amcharts](http://www.amcharts.com/), but the most important thing is their [*inspirational*](http://www.amcharts.com/inspiration/chalk/) page, which obviously inspired me. So I decide take the theme (backgorund included), search for some educational data and make something with shiny and [here](http://r-shiny-apps.jkunst.com/shiny-apps/cl-educ/) is the result (patience please beacuse it's a little slow):

<div id="wrap">
<iframe id="frame" src="http://r-shiny-apps.jkunst.com/shiny-apps/cl-educ/" frameborder="0" allowfullscreen></iframe>
</div>

The app is in in spanish (país: country, region: it's like a subdivsión, colegio: school, acerca: about) but lenguage doesnt matter to show how awesome can be a shiny app.

Ggplot2 and rChart (highcharts in particular) plots are very customizable so it wasnt a hard task to emulate the style and you can check the [repo](https://github.com/jbkunst/cl-educ) if you need more detail.

