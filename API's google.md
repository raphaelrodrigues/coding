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
  // Create a NewsSearcher
  var newsSearch = new google.search.NewsSearch();
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



