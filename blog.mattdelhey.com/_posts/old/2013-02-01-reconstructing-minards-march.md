---
layout: post
title: "Reconstructing Napoleon's Failed March to Russia"
category: old
date: 2013-02-01
excerpt: "Above is perhaps one the most famous statistical graphics ever created, found in nearly every history text-book (at least the ones I read...) that discusses Napoleon's failed invasions of Russia in 1812. The graphic was originally created by Charles Joseph Minard, a statistician ahead of his time."

---
![original_minard]({{ site.url }}/images/original_minard.jpg)

Above is perhaps one the most famous statistical graphics ever created, found in nearly every history text-book (at least the ones I read...) that discusses Napoleon's failed invasions of Russia in 1812. The graphic was originally created by [Charles Joseph Minard](http://en.wikipedia.org/wiki/Charles_Joseph_Minard), a statistician ahead of his time.

The reason this graphic is so good is that it tells a story for the viewer allowing us to visualize the march. By following along the graphic, we get a real feel for just how bad Napoleon's march must have been. My favorite observation is how the number of troops about halves every time the army crosses a river-- something that must of been pretty treacherous in the freezing temperatures. 

### Representing the march with ggplot2
We want to represent the above graphic using ggplot2's grammar of graphics. In order to do this, we need to determine the layers that we want to create, what data is required for each layer, and what geometries we need to represent the data like Minard. So let's break down each variable utilized in Minard's graphic and convert them to layers we can implement using ggplot2: 

*	Map of Russia, including names of cities. Created using the _map_ package & geom_polygon and a text file containing city names with location & geom_text. 
*	The number of troops. This is mapped to the size of the line using geom_path.
*	Direction of the army, represented by the tan/black colors and including information on the split and re-joining. Mapped using the color and lineend properties of geom_path.
*	__Missing:__ the temperature and dates of the troops as they made their way across Russia. This data could reasonably be added by superimposing geom_line and geom_point layers on the bottom of the plot.
*	__Also Missing:__ geographical information for the map of Russia, including Rivers, etc.

Luckily, the data needed for the non-missing variables is all easily obtainable. The troop and city data is provided by Hadley Wickham on his [stat405 website](http://stat405.had.co.nz/) (from which I got this example) and the map data is available from the maps package. 

### Putting it all in R
The hard part is over. Here's our graphic and how it's implemented using our grammar of graphics and ggplot2. While it doesn't look quite as good as Minard's plot, we gain a lot of power by being able to create the graphic automatically as opposed to by hand -- and it only took us a few minutes to create! 

![minard]({{ site.url }}/images/minard.png)

{% highlight r %}
library(ggplot2)
library(maps)
 
troops <- read.table("minard-troops.txt", header = T)
cities <- read.table("minard-cities.txt", header = T)
russia <- map_data("world", region = "USSR")
 
ggplot(troops, aes(x = long, y = lat)) +
  geom_polygon(aes(x = long, y = lat, group = group), data = russia, fill = "white") +
  geom_path(aes(size = survivors, color = direction, group = group), lineend = "round") +
  geom_text(aes(label = city), size = 3, data = cities) +
  xlab(NULL) + ylab(NULL) + coord_equal(xlim = c(23, 38), ylim = c(50, 60)) +
  scale_colour_manual(values = c("bisque2","grey10"))
{% endhighlight %}
