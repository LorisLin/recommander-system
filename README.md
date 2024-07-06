# Système de recommendation

## Introduction

L'objectif de ce projet est de fournir un système de recommandation sur les films pour deux utilisateurs en couple. Pour ce faire, on va utiliser deux databsets "MovieLens" et "IMBb Dataset" afin de connaître les goûts de chaque utilisateurs et également pour avoir plus d'informations sur les différents films que l'on va recommander.
Ainsi, notre travail sera de concevoir un modèle et un algorithme pouvant recommander des films et satisfaire les goûts des deux amoureux. Il sera donc important de combiner les préférances de chaque couple.

## Data Collection and Preprocessing

### Import MovieLens

Dans le jeux de données "MovieLens", nous possédons 3 fichiers :
- ratings.dat
- movies.dat
- users.dat

Cependant j'ai pris que les fichiers "ratings.dat" et "movies.dat" dans mon code, car j'ai choisi de négliger la profession, l'âge, le sex et le lieu d'habitation de l'utilisateur. Selon moi, le goût de chaque utilisateurs sur les films n'est pas directement lié à ces facteurs.
Donc dans ce jeu de données, on va surtout se focaliser sur le genre de chaque film que les utilisateurs ont noté.

### Import IMDb

Dans le jeux de données "IMDb dataset", nous possédons 7 fichiers :
- name.basics.tsv
- title.crew.tsv
- title.basics.tsv
- title.akas.tsv
- title.episode.tsv
- title.principals.tsv
- title.ratings.tsv

Cependant j'ai pris que les fichiers "title.basics.tsv" et "title.crew.tsv" dans mon code.
En effet, comme nous recommandons que des films, le fichier title.episode.tsv ne sert à rien.

Dans notre projet, j'ai choisi de prendre en compte que les genres et les directeurs des films, donc prendre les deux fichiers évoqués précedemment suffit.
Mais si je prennais en compte le lieux d'habitation des utilisateurs ou bien les acteurs des films, on aurait pu prendre "title.akas.tsv" pour l'origine (pays) du film et "title.principals.tsv" les acteurs jouées dans les films notés. 
De plus, "title.rating.tsv" aurait pu fournir plus d'avis sur la note des films dans notre jeux de données.
Néanmoins si je voulais connaître les noms des directeurs en question, j'aurais dû prendre en compte "name.basic.tsv", mais ici l'information sur l'identifiant suffit pour faire faire la recommandation.

### Merging data

Pour relié les deux jeux de données "MovieLens" et "IMDb Dataset", on va surtout utilisé les "Title" et "Year", car les identifiants pour chaque film diffèrent entre eux.
Pour relier les jeux de données "MovieLens", la jointure se fait par "MovieID" et pour "IMDB Dataset" la jointure se fait par l'élément "tconst".

## Feature Engineering

On va extraire les genres, les directeurs, les notes, l'année de production, le titre et l'identifiant de chaque films à partir de la jointure évoqué précédemment. puis on va fusionner les genres entre le jeux de données "MovieLens" et "IMDb Dataset".

## Model Development

### SVD

Notre algorithme de recommandation se base sur une technique de factorisation de matrice appelée SVD (Singular Value Decomposition). Ce modèle est implémenté à l'aide de la bibliothèque surprise en Python, qui est spécialement conçue pour les tâches de filtrage collaboratif.
Le filtrage collaboratif est une méthode populaire dans les systèmes de recommandation, permettant de prédire les préférences d'un utilisateur en se basant sur les préférences d'autres utilisateurs similaires.

### Comparaison avec les autres technique de factorisation de matrice

Le modèle SVD fournit de bonnes performances pour la prédiction des notes données par les utilisateurs, contrairement au modèle de clustering par k-means qui n'est pas spécifiquement adapté pour les systèmes de recommandation basés sur la prédiction de notes.

De plus il prend en considération différents facteurs de préférences contrairement au modèle k-NN (k-Nearest Neighbors) qui se base uniquement sur la similarité explicite entre les utilisateurs ou les objets.

Ce modèle est peu couteux en terme de complexité computationnelle contrairement au réseau de neuronnes qui peuvent être plus exigeants en ressources.

On aurait également pu prendre l'algorithme ALS (Alternating Least Squares) pour notre algorithme de recommandation. Cependant, comme Netflix utilise déjà cet algorithme, nous avons choisi d'utiliser le SVD afin de nous démarquer de cette grande entreprise.

## Recommendation Algorithme


