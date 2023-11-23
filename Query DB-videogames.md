**SELECT**

1- Selezionare tutte le software house americane (3)

    Query:

    ``` sql

        SELECT * 
        FROM 'software_houses' 
        WHERE country LIKE 'united states';

    ```
2- Selezionare tutti i giocatori della cittÃ di 'Rogahnland' (2)

    Query:

    ``` sql

        SELECT * 
        FROM 'players' 
        WHERE city LIKE 'Rogahnland';

    ```

3- Selezionare tutti i giocatori il cui nome finisce per "a" (220)

    Query:

    ``` sql

        SELECT * 
        FROM 'players'
        WHERE name LIKE '%a';

    ```

4- Selezionare tutte le recensioni scritte dal giocatore con ID = 800 (11)

    Query:

    ``` sql

        SELECT * 
        FROM 'reviews'
        WHERE player_id = 800;

    ```

5- Contare quanti tornei ci sono stati nell'anno 2015 (9)

    Query:

    ``` sql

        SELECT * 
        FROM 'tournaments'
        WHERE year = 2015

    ```

6- Selezionare tutti i premi che contengono nella descrizione la parola 'facere' (2)

    Query:

    ``` sql

        SELECT * 
        FROM 'awards' 
        WHERE description LIKE '% facere %'

    ```


7- Selezionare tutti i videogame che hanno la categoria 2 (FPS) o 6 (RPG), mostrandoli una sola volta (del videogioco vogliamo solo l'ID) (287)

    Query:

    ``` sql

        SELECT DISTINCT videogame_id 
        FROM 'category_videogame'
        WHERE category_id = 2 OR category_id = 6

    ```

8- Selezionare tutte le recensioni con voto compreso tra 2 e 4 (2947)

    Query:

    ``` sql

        SELECT * 
        FROM `reviews` 
        WHERE rating BETWEEN 2 AND 4

    ```


9- Selezionare tutti i dati dei videogiochi rilasciati nell'anno 2020 (46)

    Query:

    ``` sql

        SELECT * 
        FROM `videogames` 
        WHERE YEAR(release_date)=2020

    ```



10- Selezionare gli id videogame che hanno ricevuto almeno una recensione da 5 stelle, mostrandoli una sola volta (443)
   
    Query:

    ``` sql

        SELECT DISTINCT videogame_id 
        FROM `reviews` 
        WHERE rating >=5
        GROUP BY videogame_id
        HAVING COUNT(rating >=5) > 0

    ```

*** BONUS ***

11- Selezionare il numero e la media delle recensioni per il videogioco con ID = 412 (review number = 12, avg_rating = 3.16 circa)

    Query:

    ``` sql

        SELECT videogame_id,
        COUNT(id) as total,
        AVG (rating) as average
        FROM `reviews` 
        WHERE videogame_id = 412

    ```

12- Selezionare il numero di videogame che la software house con ID = 1 ha rilasciato nel 2018 (13)

    Query:

    ``` sql

        SELECT software_house_id,
        COUNT(id)
        FROM `videogames`
        WHERE YEAR(release_date)=2018 AND software_house_id = 1

    ```

**GROUP BY**

1- Contare quante software house ci sono per ogni paese (3)



    Query:

    ``` sql

        SELECT country,
        COUNT(id)
        FROM `software_houses` 
        GROUP BY `country`

    ```


2- Contare quante recensioni ha ricevuto ogni videogioco (del videogioco vogliamo solo l'ID) (500)

    Query:

    ``` sql

        SELECT `videogame_id`,
        COUNT(id)
        FROM `reviews` 
        GROUP BY `videogame_id`

    ```

3- Contare quanti videogiochi hanno ciascuna classificazione PEGI (della classificazione PEGI vogliamo solo l'ID) (13)

    Query:

    ``` sql
    
        SELECT `pegi_label_id` ,
        COUNT(`videogame_id`)
        FROM `pegi_label_videogame`
        GROUP BY `pegi_label_id`

    ```

4- Mostrare il numero di videogiochi rilasciati ogni anno (11)

    Query:

    ``` sql

        SELECT YEAR(`release_date`),
        COUNT(id)
        FROM `videogames` 
        GROUP BY YEAR(`release_date`)

    ```

5- Contare quanti videogiochi sono disponbiili per ciascun device (del device vogliamo solo l'ID) (7)

    Query:

    ``` sql

        SELECT `device_id`,
        COUNT(`videogame_id`) 
        FROM `device_videogame`
        GROUP BY `device_id`

    ```

6- Ordinare i videogame in base alla media delle recensioni (del videogioco vogliamo solo l'ID) (500)

    Query:

    ``` sql

        SELECT `videogame_id`,
        FORMAT(AVG(rating),2) AS average
        FROM `reviews` 
        GROUP BY `videogame_id`
        ORDER BY average

    ```

**JOIN**

1- Selezionare i dati di tutti giocatori che hanno scritto almeno una recensione, mostrandoli una sola volta (996)

    Query:

    ``` sql

        SELECT players.* 
        FROM `players`
        JOIN reviews
            ON players.id = reviews.player_id
        GROUP BY players.id

    ```

2- Sezionare tutti i videogame dei tornei tenuti nel 2016, mostrandoli una sola volta (226)

    Query:

    ``` sql

        SELECT videogames.* 
        FROM `videogames` 
        JOIN tournament_videogame
            ON videogames.id = tournament_videogame.videogame_id
        JOIN tournaments
            ON tournament_videogame.tournament_id = tournaments.id
        WHERE tournaments.year = 2016
        GROUP BY videogames.id

    ```

3- Mostrare le categorie di ogni videogioco (1718)

    Query:

    ``` sql

        SELECT  videogames.*, categories.name as category_name
        FROM `videogames` 
        JOIN category_videogame
            ON videogames.id = category_videogame.videogame_id
        JOIN categories
            ON category_videogame.category_id = categories.id
        ORDER BY videogames.id

    ```

4- Selezionare i dati di tutte le software house che hanno rilasciato almeno un gioco dopo il 2020, mostrandoli una sola volta (6)


    Query:

    ``` sql

        SELECT software_houses.* , COUNT(videogames.id) as games_after_2020
        FROM `software_houses`
        JOIN videogames
            ON software_houses.id = videogames.software_house_id
        WHERE YEAR(videogames.release_date) > 2020 
        GROUP BY software_houses.id
        HAVING games_after_2020 > 0

    ```

5- Selezionare i premi ricevuti da ogni software house per i videogiochi che ha prodotto (55)


    Query:

    ``` sql

        SELECT software_houses.name AS sftwr_name, videogames.name AS game_name, awards.name AS award_name
        FROM `software_houses`
        JOIN videogames
            ON software_houses.id = videogames.software_house_id
        JOIN award_videogame
            ON videogames.id=award_videogame.videogame_id
        JOIN awards
            ON award_videogame.award_id=awards.id
        ORDER BY software_houses.name

    ```


6- Selezionare categorie e classificazioni PEGI dei videogiochi che hanno ricevuto recensioni da 4 e 5 stelle, mostrandole una sola volta (3363)

   Query:

    ``` sql

        SELECT DISTINCT videogames.id, categories.name AS category, pegi_labels.name AS pegi
        FROM `videogames` 
        JOIN category_videogame
            ON videogames.id = category_videogame.videogame_id
        JOIN categories
            ON category_videogame.category_id = categories.id
        JOIN pegi_label_videogame
            ON videogames.id = pegi_label_videogame.videogame_id
        JOIN pegi_labels
            ON pegi_label_videogame.pegi_label_id = pegi_labels.id
        JOIN reviews
            ON videogames.id = reviews.videogame_id
        WHERE reviews.rating = 4 OR reviews.rating = 5

    ```

7- Selezionare quali giochi erano presenti nei tornei nei quali hanno partecipato i giocatori il cui nome inizia per 'S' (474)

   Query:

    ``` sql

        SELECT DISTINCT videogames.*
        FROM `videogames` 
        JOIN tournament_videogame
            ON videogames.id = tournament_videogame.videogame_id
        JOIN tournaments
            ON tournament_videogame.tournament_id = tournaments.id
        JOIN player_tournament
            ON tournaments.id = player_tournament.tournament_id
        JOIN players
            ON player_tournament.player_id = players.id
            
        WHERE players.name LIKE 's%'

    ```

8- Selezionare le cittÃ in cui Ã¨ stato giocato il gioco dell'anno del 2018 (36)


   Query:

    ``` sql

        SELECT DISTINCT tournaments.city
        FROM `videogames` 
        JOIN award_videogame
        	ON videogames.id= award_videogame.videogame_id
        JOIN awards
        	ON award_videogame.award_id = awards.id
        JOIN tournament_videogame
            ON videogames.id = tournament_videogame.videogame_id
        JOIN tournaments
            ON tournament_videogame.tournament_id = tournaments.id
        WHERE awards.id = 1 AND YEAR(videogames.release_date)=2018

    ```

9- Selezionare i giocatori che hanno giocato al gioco piÃ¹ atteso del 2018 in un torneo del 2019 (3306)


   Query:

    ``` sql

    -- Da consegna sarebbe DISTINCT ma...
   SELECT players.*
        FROM `videogames` 
        JOIN award_videogame
        	ON videogames.id= award_videogame.videogame_id
        JOIN awards
        	ON award_videogame.award_id = awards.id
        JOIN tournament_videogame
            ON videogames.id = tournament_videogame.videogame_id
        JOIN tournaments
            ON tournament_videogame.tournament_id = tournaments.id
        JOIN player_tournament
            ON tournaments.id = player_tournament.tournament_id
        JOIN players
            ON player_tournament.player_id = players.id
            
       WHERE awards.id = 5 AND YEAR(videogames.release_date)=2018 AND tournaments.year = 2019

    ```

*** BONUS ***

10- Selezionare i dati della prima software house che ha rilasciato un gioco, assieme ai dati del gioco stesso (software house id : 5)


   Query:

    ``` sql

        SELECT software_houses.*, videogames.*
        FROM software_houses
        JOIN videogames ON software_houses.id = videogames.software_house_id
        ORDER BY videogames.release_date
        LIMIT 1

    ```

11- Selezionare i dati del videogame (id, name, release_date, totale recensioni) con piÃ¹ recensioni (videogame id : potrebbe uscire 449 o 398, sono entrambi a 20)


   Query:

    ``` sql

        SELECT DISTINCT videogames.id, COUNT(reviews.videogame_id) as reviewsnum
        FROM `videogames` 
        JOIN reviews
            ON videogames.id = reviews.videogame_id
        GROUP BY videogames.id
        ORDER BY reviewsnum DESC
        LIMIT 1

    ```

12- Selezionare la software house che ha vinto piÃ¹ premi tra il 2015 e il 2016 (software house id : potrebbe uscire 3 o 1, sono entrambi a 3)


   Query:

    ``` sql

        SELECT DISTINCT software_houses.name AS sftwr_name, COUNT(award_videogame.id)
        FROM `software_houses`
        JOIN videogames
            ON software_houses.id = videogames.software_house_id
        JOIN award_videogame
            ON videogames.id=award_videogame.videogame_id
        JOIN awards
            ON award_videogame.award_id=awards.id
        WHERE YEAR(videogames.release_date) BETWEEN 2015 AND 2016
        GROUP BY software_houses.name
        ORDER BY software_houses.name DESC
        LIMIT 1

    ```

13- Selezionare le categorie dei videogame i quali hanno una media recensioni inferiore a 2 (10)

   Query:

    ``` sql

        SELECT DISTINCT categories.name AS category
        FROM videogames
        JOIN category_videogame 
            ON videogames.id = category_videogame.videogame_id
        JOIN categories 
            ON category_videogame.category_id = categories.id
        JOIN reviews 
            ON videogames.id = reviews.videogame_id
        GROUP BY categories.name, videogames.id
        HAVING AVG(reviews.rating)<2

    ```