## Aufgabe MongoDB-1_1

Beginne mit den folgenden Schritten:

- Installiere mongodb shell!
- Nutze die ZIP-Datei (siehe Code - Snippets)
- Entpacke diese Datei in einen neuen Ordner.
- Öffne das Terminal und navigiere zu diesem Ordner.
- Überprüfe mit 'pwd' und 'ls', ob du im richtigen Ordner bist.
- Wenn du im richtigen Ordner bist, starte die Mongo Shell.
- Verbinde dich mit deinem Cluster und nutze den Befehl 'load('loadMovieDetailsDataset.js')' um die Datei in deine Datenbank zu laden.
- Lade alternativ die Datei über die GUI von Mongo Atlas hoch.
- Überprüfe mit 'show dbs', dass die Datenbank geladen wurde.

->

```
load('loadMovieDetailsDataset.js')
show dbs
show collections

```

//

### Rufe alle Filme ab, bei denen der Regisseur (director) Steven Spielberg ist und gib nur das Feld 'Titel' aus.

```
video> let result = db.movieDetails.find({'director':'Steven Spielberg'});

video> result.forEach((doc) => {print(doc.title);});

The Adventures of Tintin
Raiders of the Lost Ark
The Lost World: Jurassic Park
Catch Me If You Can
A.I. Artificial Intelligence
E.T. the Extra-Terrestrial
```

//

### Rufe alle Filme ab, bei denen die Anzahl der Benutzerbewertungen bei Rotten Tomatoes mehr als 40000 ist. Beschränke die Suche auf 20 Filme und sortiere sie absteigend nach Benutzerbewertungen.

```
video> let result = db.movieDetails.find({'tomato.userReviews':{$gt:40000}}).sort({'tomato.userRating': -1}).limit(20);

video> result.forEach((doc) => { print(doc.title, doc.tomato.userRating) });

Life Is Beautiful 4.3
Toy Story 3 4.3
The Martian 4.3
Toy Story 3 4.3
Big Hero 6 4.3
Once Upon a Time in the West 4.3
The Godfather: Part II 4.3
Dawn of the Planet of the Apes 4.2
Star Trek Into Darkness 4.2
Love Me If You Dare 4.2
All About My Mother 4.2
Dr. Strangelove or: How I Learned to Stop Worrying and Love the Bomb 4.2
Iron Man 4.2
Star Trek 4.1
Star Wars: Episode IV - A New Hope 4.1
Life of Pi 4.1
The Hobbit: An Unexpected Journey 4.1
Gone Girl 4.1
Star Wars: Episode V - The Empire Strikes Back 4.1
The Girl with the Dragon Tattoo 4.1
```

//

### Rufe alle Filme ab, die zwischen 2000 und 2005 gedreht wurden (beide Jahre eingeschlossen) und gib nur die Felder 'Titel' und 'Jahr' aus.

```
video> let result = db.movieDetails.find({'year':{$gte: 2000, $lte: 2005}});

video> result.forEach((doc) => { print(doc.title, doc.year) });

Star Wars: Episode III - Revenge of the Sith 2005
Star Wars: Episode II - Attack of the Clones 2002
Love Actually 2003
Punch-Drunk Love 2002
Zathura: A Space Adventure 2005
Space Cowboys 2000
2001: A Space Travesty 2000
The Adventures of Sharkboy and Lavagirl 3-D 2005
The Adventures of Rocky & Bullwinkle 2000
Best in Show 2000
...
```

//

### Rufe alle Filme ab, die eine Rotten Tomatoes Benutzerbewertung von mindestens 4 haben und nach 2010 entstanden sind. Gib nur die Felder 'Titel' und 'Regisseur' (director) aus.

```
video> let result = db.movieDetails.find({'tomato.userRating':{$gte: 4}, 'year':{$gt: 2010}});

video> result.forEach((doc) => { print(`${doc.title} - ${doc.director}`) });

West of Memphis - Amy Berg
Star Trek Into Darkness - J.J. Abrams
Rise of the Planet of the Apes - Rupert Wyatt
Dawn of the Planet of the Apes - Matt Reeves
Wild Tales - Damián Szifrón
...
```

//

### Rufe alle Filme ab, die weniger als 1000 Benutzer-Rezensionen bei Rotten Tomatoes haben und vor dem Jahr 2005 gedreht wurden. Sortiere sie aufsteigend nach der Anzahl der Benutzer-Rezensionen und beschränke die Suche auf 10 Filme.

```
video> let result = db.movieDetails.find({'tomato.userReviews':{$lt:1000}, 'year':{$lt: 2005} }).sort({'tomato.userReviews': 1}).limit(10);

video> result.forEach((doc) => { print(`${doc.title} - ${doc.tomato.userReviews} - ${doc.year}`) });

The Life Aquatic with Steve Zissou - 121 - 2004
Bounce Ko Gals - 144 - 1997
OT: Our Town - 154 - 2002
```

//

### Rufe alle Filme ab, die das Feld 'Rotten Tomatoes' nicht enthalten.

```
video> let result = db.movieDetails.find({'tomato':{$exists: false}});

video> result.forEach((doc) => { print(`${doc.title}`) });

West Side Story
An American Tail: Fievel Goes West
Red Rock West
...
```

//

### Rufe alle Filme ab, die mindestens 100 IMDb-Stimmen, aber weniger als 1000 haben und gib nur die Felder 'Titel' und 'IMDb Bewertung' aus.

```
video> let result = db.movieDetails.find({'imdb.votes':{$gte: 100, $lte: 1000} });

video> result.forEach((doc) => { print(`${doc.title} - ${doc.imdb.votes}`) });

Where the Trail Ends - 951
Western - 938
Total western - 585
Western Spaghetti - 532
The Cowboy and the Lady - 762
Quick Gun Murugun: Misadventures of an Indian Cowboy - 656
```

//

### Sortiere alle Filme absteigend nach ihrer IMDb-Bewertung.

```
db.movieDetails.find().sort({'imdb.votes': -1})
```

//

### Rufe die 10 Filme mit der höchsten IMDb-Bewertung ab, sortiert in absteigender Reihenfolge.

```
video> let result = db.movieDetails.find().sort({'imdb.votes': -1}).limit(10);

video> result.forEach((doc) => { print(`${doc.title} - ${doc.imdb.votes}`) });
Star Wars: Episode IV - A New Hope - 822849
Star Wars: Episode V - The Empire Strikes Back - 739949
The Godfather: Part II - 736658
Shutter Island - 720127
Terminator 2: Judgment Day - 697755
Iron Man - 641369
Star Wars: Episode VI - Return of the Jedi - 623252
Raiders of the Lost Ark - 616230
The Truman Show - 613430
The Hobbit: An Unexpected Journey - 599715
```

//

### Rufe alle Filme mit den Genres 'Crime' und 'Drama' ab und gib nur die Felder 'Titel' und 'Genre' aus. Sortiere sie aufsteigend nach ihrer IMDb-Bewertung.

```
video> let result = db.movieDetails.find({ 'genres': { $all: ['Crime', 'Drama'] } }).sort({'imdb.votes': 1});

video> result.forEach((doc) => { print(`${doc.title} - ${doc.genres.join(', ')}`) });

BL.A.CK - Short, Crime, Drama
Ox Films Collection - Adventure, Crime, Drama
IG Farben - Short, Crime, Drama
Clipped Wings They Do Fly Music CD - Documentary, Crime, Drama
Mr. Id - Crime, Drama, Thriller
...


```
