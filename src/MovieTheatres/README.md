
# SQL-practice

### SQL Practice №2 | Movie theatres

#### Relational Schema

![Sql_movie_theaters](https://user-images.githubusercontent.com/69513400/130349044-b76448a4-46e2-47cf-aba8-434afeaa4e8b.png)


#### Table creation code

``` sql 
 CREATE TABLE Movies (
   Code INTEGER PRIMARY KEY NOT NULL,
   Title TEXT NOT NULL,
   Rating TEXT 
 );
  
 CREATE TABLE MovieTheaters (
   Code INTEGER PRIMARY KEY NOT NULL,
   Name TEXT NOT NULL,
   Movie INTEGER  
     CONSTRAINT fk_Movies_Code REFERENCES Movies(Code)
 );
```

#### Sample dataset

``` sql
 INSERT INTO Movies (Code, Title, Rating) VALUES (9, 'Citizen King', 'G');
 INSERT INTO Movies (Code, Title, Rating) VALUES (1, 'Citizen Kane', 'PG');
 INSERT INTO Movies (Code, Title, Rating) VALUES (2, 'Singin'' in the Rain', 'G');
 INSERT INTO Movies (Code, Title, Rating) VALUES (3, 'The Wizard of Oz', 'G');
 INSERT INTO Movies (Code, Title, Rating) VALUES (4, 'The Quiet Man', NULL);
 INSERT INTO Movies (Code, Title, Rating) VALUES (5, 'North by Northwest', NULL);
 INSERT INTO Movies (Code, Title, Rating) VALUES (6, 'The Last Tango in Paris', 'NC-17');
 INSERT INTO Movies (Code, Title, Rating) VALUES (7, 'Some Like it Hot', 'PG-13');
 INSERT INTO Movies (Code, Title, Rating) VALUES (8, 'A Night at the Opera', NULL);
 
 INSERT INTO MovieTheaters (Code, Name, Movie) VALUES (1, 'Odeon', 5);
 INSERT INTO MovieTheaters (Code, Name, Movie) VALUES (2, 'Imperial', 1);
 INSERT INTO MovieTheaters (Code, Name, Movie) VALUES (3, 'Majestic', NULL);
 INSERT INTO MovieTheaters (Code, Name, Movie) VALUES (4, 'Royale', 6);
 INSERT INTO MovieTheaters (Code, Name, Movie) VALUES (5, 'Paraiso', 3);
 INSERT INTO MovieTheaters (Code, Name, Movie) VALUES (6, 'Nickelodeon', NULL);
```

#### Exercises

1. Select the title of all movies.

2. Show all the distinct ratings in the database.

3. Show all unrated movies.

4. Select all movie theaters that are not currently showing a movie.

5. Select all data from all movie theaters and, additionally, the data from the movie that is being shown in the theater (if one is being shown).

6. Select all data from all movies and, if that movie is being shown in a theater, show the data from the theater.

7. Show the titles of movies not currently being shown in any theaters.

8. Add the unrated movie "One, Two, Three".

9. Set the rating of all unrated movies to "G".

10. Remove movie theaters projecting movies rated "NC-17".

#### Answers

``` sql
1.SELECT title FROM movies;
2.SELECT DISTINCT rating FROM movies;
3.SELECT * FROM movies WHERE rating IS NULL;
4.SELECT * FROM movieTheaters WHERE movie IS NULL;
5.SELECT * FROM movieTheaters LEFT JOIN movies ON movieTheaters.movie = movies.code;
6.SELECT * FROM movieTheaters RIGHT JOIN movies ON movieTheaters.movie = movies.code;
7.SELECT title FROM movies WHERE code NOT IN (SELECT movie FROM movieTheaters WHERE movie IS NOT NULL);
8.INSERT INTO movies(code,title,rating) VALUES(10,'One, Two, Three', NULL);
9.UPDATE movies SET rating = 'G' WHERE rating IS NULL;
10.DELETE FROM movieTheaters WHERE movie IN (SELECT code FROM movies WHERE rating = 'NC-17');
```