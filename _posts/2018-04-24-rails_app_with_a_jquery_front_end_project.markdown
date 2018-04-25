---
layout: post
title:      "Rails App with a jQuery Front End project"
date:       2018-04-25 01:47:20 +0000
permalink:  rails_app_with_a_jquery_front_end_project
---


For my forth project, I built on my BeerMe app, which I created for my Rails project. Overall, I found it to be a very different experience than my last three projects as I wasn’t starting something from scratch. Instead, I was able to tweak and expand upon on previous functionality I had developed in order to meet the requirements of this assessment. I think my favorite aspect was being able to move towards a user experience where actions occurred without a full page refresh. 

One of the obstacles I faced during the course of my project occurred when I was updating the `Breweries#Show` page to render the data via jQuery/AJAX. On that page those pages, you can see the list of associated beers and filter them by category. The issue I ran into was that when there were no beers for that particular brewery, none of the data was being returned. 

To fix this, I wound up refactoring how my category (and later on, brewery) filters worked. In my first iteration of this app, each controller and model was responsible for processing the filter and generating the results. On the `Breweries#Show` page the chosen category was passed as a param and then the Brewery model would handle the filtering.

I noticed how repetitive it felt when I barely had to change the code in order to implement the brewery filter on the `Categories#Show` page, but knew that it would require a bigger, more wide-spread change. So I put it on hold, but when I ran into the aforementioned obstacle, it forced me to review the issue. While investigating the issue I spent time reviewing the JSON that was being returned by the GET requests. It became apparent that the brewery data was nested inside of the beer data, which got me thinking that if a beer wasn’t returned by that API that the brewery data wouldn’t either even if it was available. So in order to decouple the data, I created a separate serializer to handle the brewery data required for the Show page and another one to handle when brewery data was required alongside a beer (e.g. `Beers#Index`). To make sure the correct ones were used, I specified the serializers in the different controller actions. Then I refactored the `getBeers()` function to always make a call to `/beers` which would in turn use the appropriate serializer for the data. 

*Before*
```
function getBeers(url, filters) {
   $.ajax({
     url: url,
     data: filters
   }).success(function(beerData){
     displayBeers(beerData);
   })
 }
```

*After*
```
function getBeers(filters) {
   $.ajax({
    url: "/beers.json",
    data: filters
  }).success(function(beerData){
    $('tbody').empty();
    if (Object.keys(beerData).length === 0) {
      $('tbody').append("<tr><td><em>No results</em></td><td></td><td></td><td></td></tr>");
    } else {
        beerData.forEach(function(data){
        let beer = new Beer(data);
        beer.beerListDisplay();
      })
    }
  })
}
```

In the end, not only did making two separate API calls solve my problem but it also allowed me to slim down my controller methods and make my code more manageable as the number of places that need to be updated if filter logic changes is minimalized. 

Other refactoring that I hope to do in the future includes: having the UserBeers pages render using jQuery and an Active Model Serialization JSON Backend and incorporating Handlebars and templates.

