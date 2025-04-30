# Exam-sql

1-Nom et année de naissance des artistes nés avant 1950.
1) Nom et année de naissanceSELECT nom, annéeNaiss
FROM artiste
WHERE annéeNaiss < 1950 ;

2-Titre de tous les drames.
2) SELECT titre
FROM film
WHERE genre = 'Drame'
ORDER BY titre;  --> Pas besoin du Order BY titre c'est que visuel

3-Quels rôles a joué Bruce Willis.
3) SELECT role.nomRôle
FROM role
JOIN artiste ON role.idActeur = artiste.idArtiste
WHERE artiste.nom = 'Willis' AND artiste.prénom = 'Bruce';




4-Qui est le réalisateur de Memento.

4) SELECT a.nom, a.prénom
FROM film f
JOIN artiste a ON f.idRéalisateur = a.idArtiste
WHERE f.titre = 'Memento';


![reponse 4](https://github.com/user-attachments/assets/f7fed55f-d025-40af-9bc2-b80f59f17e13)
5-Quelles sont les notes obtenues par le film Fargo
5) SELECT notation.note
FROM notation
JOIN film ON notation.idFilm = film.idFilm
WHERE film.titre = 'Fargo'
ORDER BY notation.note;   --> on l'obtient grace a l'id film 275 qu'il possede une note de 6 

6-Qui a joué le rôle de Chewbacca?
6) SELECT artiste.nom, artiste.prénom
FROM role
JOIN artiste ON role.idActeur = artiste.idArtiste
WHERE role.nomRôle = 'Chewbacca';   --> on a l'id acteur et nomRole dans la table role et on relie role a artiste a l'aide de JOIN

7-Dans quels films Bruce Willis a-t-il joué le rôle de John McClane ?
7)SELECT film.titre
FROM role
JOIN film ON role.idFilm = film.idFilm
JOIN artiste ON role.idActeur = artiste.idArtiste
WHERE artiste.nom = 'Willis' AND artiste.prénom = 'Bruce' AND role.nomRôle = 'John McClane';

8-Nom des acteurs de 'Sueurs froides'
8)SELECT a.nom, a.prénom
FROM film f
JOIN role r ON f.idFilm = r.idFilm
JOIN artiste a ON r.idActeur = a.idArtiste
WHERE f.titre = 'Sueurs froides';  

![reponse 8](https://github.com/user-attachments/assets/1326f216-d3c1-4a0a-936c-bad436d3e9d7)

9-Quelles sont les films notés par l'internaute Prénom 0 Nom0
9)   SELECT DISTINCT f.titre
FROM internaute i
JOIN notation n ON n.email = i.email
JOIN film f ON n.idFilm = f.idFilm

WHERE (i.prénom = 'Prénom0' AND i.nom = 'Nom0');

10- Films dont le réalisateur est Tim Burton, et l’un des acteurs
Johnny Depp.
10) 
SELECT f.titre
FROM film f
JOIN role r ON f.idFilm = r.idFilm
JOIN artiste a1 ON r.idActeur = a1.idArtiste
JOIN artiste a2 ON f.idRéalisateur = a2.idArtiste
WHERE a2.nom = 'Burton' AND a2.prénom = 'Tim'
AND a1.nom = 'Depp' AND a1.prénom = 'Johnny'
ORDER BY f.titre;


11-Titre des films dans lesquels a joué ́Woody Allen. Donner aussi
le rôle.
11) 
SELECT f.titre, r.nomRôle
FROM role r
JOIN film f ON r.idFilm = f.idFilm
JOIN artiste a ON r.idActeur = a.idArtiste
WHERE a.nom = 'Allen' AND a.prénom = 'Woody'
ORDER BY f.titre;

12-Quel metteur en scène a tourné dans ses propres films ? Donner
le nom, le rôle et le titre des films.
12)SELECT a.nom, a.prénom, r.nomRôle, f.titre
FROM film f
JOIN role r ON f.idFilm = r.idFilm
JOIN artiste a ON r.idActeur = a.idArtiste
WHERE f.idRéalisateur = a.idArtiste
ORDER BY a.nom, f.titre; -->order optionnel

13-Titre des films de Quentin Tarantino dans lesquels il n’a pas
joué
13)


