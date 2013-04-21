#Guides 

REFERENCE

http://code.google.com/apis/ajax/playground/#the_hello_world_of_news_search
#Google News API

# Inicializações
###1."Raw Searcher" to handle search queries
```java
// This code generates a "Raw Searcher" to handle search queries. The Raw 
// Searcher requires you to handle and draw the search results manually.
google.load('search', '1');
```

###2. Create a News Search instance.
  
`newsSearch = new google.search.NewsSearch();` is implemented in the following methods, static methods, and properties:

# Pesquisas
## Procurar por String
Poderá servir para procurar por #tag.Exemplo:

` newsSearch.execute('Barack Obama');`

##Geo Restrição
```java

  // Set the geolocation to Australia
  var extendedArgs = google.search.Search.RESTRICT_EXTENDED_ARGS;
  newsSearch.setRestriction(extendedArgs, {'geo':'Australia'});

  // Add the searcher to the SearchControl
  searchControl.addSearcher(newsSearch);

  // tell the searcher to draw itself and tell it where to attach
  searchControl.draw(document.getElementById("content"));

  // Australian for...
  searchControl.execute('Fosters');
```

##News Edition Restriction
```java

  var extendedArgs = google.search.Search.RESTRICT_EXTENDED_ARGS;

  // Restrict to the UK news edition.
  newsSearch.setRestriction(extendedArgs, {'ned':'uk'});

  // Add the searcher to the SearchControl
  searchControl.addSearcher(newsSearch);

  // tell the searcher to draw itself and tell it where to attach
  searchControl.draw(document.getElementById("content"));

  // A good sport
  searchControl.execute('Football');
```

#Pesquisar por data

```Java
  // Search by date (instead of relevence)
  newsSearch.setRestriction(extendedArgs, {'scoring':'d'});

  // Add the searcher to the SearchControl
  searchControl.addSearcher(newsSearch);

  // tell the searcher to draw itself and tell it where to attach
  searchControl.draw(document.getElementById("content"));

  // A good sport
  searchControl.execute('President');
```




#Google Feeds API

##Load Feed

```JAVA
  // Create a feed instance that will grab Digg's feed.
  var feed = new google.feeds.Feed("http://www.digg.com/rss/index.xml");

  // Calling load sends the request off.  It requires a callback function.
  feed.load(feedLoaded);
```

##Result XML

```
// Request the results in XML
  feed.setResultFormat(google.feeds.Feed.XML_FORMAT)
```

##Feeds Control
```JAVA
// Add two feeds.
  feedControl.addFeed("http://www.digg.com/rss/index.xml", "Digg");
  feedControl.addFeed("http://feeds.feedburner.com/Techcrunch", "TechCrunch");

  // Draw it.
  feedControl.draw(document.getElementById("content"));
```

##Find Feed

```JAVA
function OnLoad() {
  // Query for president feeds on cnn.com
  var query = 'site:cnn.com president';
  google.feeds.findFeeds(query, findDone);
}
```

```JAVA
function findDone(result) {
  // Make sure we didn't get an error.
  if (!result.error) {
    // Get content div
    var content = document.getElementById('content');
    var html = '';

    // Loop through the results and print out the title of the feed and link to
    // the url.
    for (var i = 0; i < result.entries.length; i++) {
      var entry = result.entries[i];
      html += '<p><a href="' + entry.url + '">' + entry.title + '</a></p>';
    }
    content.innerHTML = html;
  }
}

```

