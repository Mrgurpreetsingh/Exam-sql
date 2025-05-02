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
13)SELECT f.titre
FROM film f
JOIN artiste a ON f.idRéalisateur = a.idArtiste
WHERE a.nom = 'Tarantino' AND a.prénom = 'Quentin'
AND f.idFilm NOT IN (
    SELECT r.idFilm
    FROM role r
    WHERE r.idActeur = a.idArtiste
)
ORDER BY f.titre;

14-Quel metteur en scène a tourné ́en tant qu’acteur ? Donner le
nom, le rôle et le titre des films dans lesquels cet artiste a joué.
14)SELECT DISTINCT a.nom, a.prénom, r.nomRôle, f.titre
FROM role r
JOIN film f ON r.idFilm = f.idFilm
JOIN artiste a ON r.idActeur = a.idArtiste
WHERE a.idArtiste IN (
    SELECT f.idRéalisateur
    FROM film f
)
ORDER BY a.nom, f.titre;


15-Exo 15 Donnez les films de Hitchcock sans James Stewart
15)SELECT f.titre
FROM Film f
JOIN Artiste a ON f.idRéalisateur = a.idArtiste
WHERE a.nom = 'Hitchcock'
  AND NOT EXISTS (
    SELECT 1
    FROM Role r
    JOIN Artiste a2 ON r.idActeur = a2.idArtiste
    WHERE r.idFilm = f.idFilm
      AND a2.nom = 'Stewart'
  )
16-Exo 16 Dans quels films le réalisateur a-t-il le même prénom que l’un
des interprètes ? (titre, nom du réalisateur, nom de l’interprète). Le
réalisateur et l’interprète ne doivent pas être la même personne.
16)SELECT 
    f.titre, 
    a1.nom AS nom_realisateur, 
    a1.prénom AS prénom_realisateur,
    a2.nom AS nom_acteur, 
    a2.prénom AS prénom_acteur
FROM Film f
JOIN Artiste a1 ON f.idRéalisateur = a1.idArtiste
JOIN Role r ON f.idFilm = r.idFilm
JOIN Artiste a2 ON r.idActeur = a2.idArtiste
WHERE a1.prénom = a2.prénom
  AND a1.idArtiste != a2.idArtiste;

![reponse 16](https://github.com/user-attachments/assets/ab68ea1e-075e-432c-a7d2-84343405155e)


17-Les films sans rôle
17)SELECT titre
FROM film
WHERE idFilm NOT IN  (SELECT idFilm  FROM role);
![reponse 17](https://github.com/user-attachments/assets/5483ebbc-8b56-4332-adbd-c85fb5e583bb)

18-Quelles sont les films non notés par l'internaute Prénom1 Nom1
18)SELECT titre
FROM film
WHERE idFilm NOT IN (
  SELECT idFilm FROM notation
  WHERE email = 'prenom1.nom1@example.com'
);


19-Quels acteurs n’ont jamais réalisé de film ?
19)SELECT DISTINCT a.nom, a.prénom
FROM artiste a
JOIN role r ON a.idArtiste = r.idActeur
WHERE a.idArtiste NOT IN (
  SELECT idRéalisateur FROM film
);
![reponse 19](https://github.com/user-attachments/assets/dedeeaff-5054-4e8f-aea7-fb3c39365af3)


20-Quelle est la moyenne des notes de Memento

20)SELECT AVG(note) AS moyenne
FROM notation n
JOIN film f ON n.idFilm = f.idFilm
WHERE f.titre = 'Memento';


![reponse 20](https://github.com/user-attachments/assets/f14aa1c9-12dc-4e99-81d9-99292458939a)



21- id, nom et prénom des réalisateurs, et nombre de films qu’ils
ont tournés.

21)SELECT a.idArtiste, a.nom, a.prénom, COUNT(f.idFilm) AS nb_films
FROM artiste a
JOIN film f ON a.idArtiste = f.idRéalisateur
GROUP BY a.idArtiste;


22-Nom et prénom des réalisateurs qui ont tourné au moins deux
films. 

22)SELECT a.nom, a.prénom
FROM artiste a
JOIN film f ON a.idArtiste = f.idRéalisateur
GROUP BY a.idArtiste
HAVING COUNT(f.idFilm) >= 2
ORDER BY a.nom, a.prénom; 

23-Quels films ont une moyenne des notes supérieure à 7
23)SELECT f.titre, AVG(n.note) AS moyenne
FROM film f
JOIN notation n ON f.idFilm = n.idFilm
GROUP BY f.titre
HAVING AVG(n.note) > 7;







