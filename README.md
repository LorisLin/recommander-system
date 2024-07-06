# Système de recommendation

## Introduction

L'objectif de ce projet est de fournir un système de recommandation sur les films pour deux utilisateurs en couple. Pour ce faire, on va utiliser deux databsets "MovieLens" et "IMBb Dataset" afin de connaître les goûts de chaque utilisateurs et également pour avoir plus d'informations sur les différents films que l'on va recommander.
Ainsi, notre travail sera de concevoir un modèle et un algorithme pouvant recommander des films et satisfaire les goûts des deux amoureux. Il sera donc important de combiner les préférances de chaque couple.

## Collecte et prétraitement des données

### Importation de MovieLens

Dans le jeux de données "MovieLens", nous possédons 3 fichiers :
- ratings.dat
- movies.dat
- users.dat

Cependant j'ai pris que les fichiers "ratings.dat" et "movies.dat" dans mon code, car j'ai choisi de négliger la profession, l'âge, le sex et le lieu d'habitation de l'utilisateur. Selon moi, le goût de chaque utilisateurs sur les films n'est pas directement lié à ces facteurs.
Donc dans ce jeu de données, on va surtout se focaliser sur le genre de chaque film que les utilisateurs ont noté.

### Importation de IMDb

Dans le jeux de données "IMDb dataset", nous possédons 7 fichiers :
- name.basics.tsv
- title.crew.tsv
- title.basics.tsv
- title.akas.tsv
- title.episode.tsv
- title.principals.tsv
- title.ratings.tsv

J'ai choisi d'inclure seulement les fichiers "title.basics.tsv" et "title.crew.tsv" dans mon projet. Étant donné que notre système de recommandation se concentre uniquement sur les films, le fichier "title.episode.tsv" n'est pas pertinent car il concerne les épisodes de séries, qui ne sont pas notre objectif principal.

Dans notre approche, nous nous sommes limités aux genres et aux réalisateurs des films, ce qui est couvert par les fichiers mentionnés précédemment. Ces données sont suffisantes pour nos besoins de recommandation.

Cependant, si j'avais choisi d'inclure d'autres aspects tels que le lieux d'habitation des utilisateurs ou bien les acteurs des films, on aurait pu prendre "title.akas.tsv" pour l'origine (pays) du film et "title.principals.tsv" les acteurs jouées dans les films notés. 

De plus, "title.rating.tsv" aurait pu fournir plus d'avis sur la note des films dans notre jeux de données.
Néanmoins si je voulais connaître les noms des réalisateurs en question, j'aurais dû prendre en compte "name.basic.tsv", mais ici l'information sur l'identifiant suffit pour faire faire la recommandation.

### Fusion des données

Pour relié les deux jeux de données "MovieLens" et "IMDb Dataset", on va surtout utilisé les "Title" et "Year", car les identifiants pour chaque film diffèrent entre eux.
Pour relier les jeux de données "MovieLens", la jointure se fait par "MovieID" et pour "IMDB Dataset" la jointure se fait par l'élément "tconst".

## Ingénierie des caractéristiques

On va extraire les genres, les réalisateurs, les notes, l'année de production, le titre et l'identifiant de chaque films à partir de la jointure évoqué précédemment. puis on va fusionner les genres entre le jeux de données "MovieLens" et "IMDb Dataset".

## Développement du modèle

### SVD

Notre algorithme de recommandation se base sur une technique de factorisation de matrice appelée SVD (Singular Value Decomposition). Ce modèle est implémenté à l'aide de la bibliothèque surprise en Python, qui est spécialement conçue pour les tâches de filtrage collaboratif.
Le filtrage collaboratif est une méthode populaire dans les systèmes de recommandation, permettant de prédire les préférences d'un utilisateur en se basant sur les préférences d'autres utilisateurs similaires.

### Comparaison avec les autres techniques de factorisation de matrice

Le modèle SVD fournit de bonnes performances pour la prédiction des notes données par les utilisateurs, contrairement au modèle de clustering par k-means qui n'est pas spécifiquement adapté pour les systèmes de recommandation basés sur la prédiction de notes.

De plus il prend en considération différents facteurs de préférences contrairement au modèle k-NN (k-Nearest Neighbors) qui se base uniquement sur la similarité explicite entre les utilisateurs ou les objets.

Ce modèle est peu couteux en terme de complexité computationnelle contrairement au réseau de neuronnes qui peuvent être plus exigeants en ressources.

On aurait également pu prendre l'algorithme ALS (Alternating Least Squares) pour notre algorithme de recommandation. Cependant, comme Netflix utilise déjà cet algorithme, nous avons choisi d'utiliser le SVD afin de nous démarquer de cette grande entreprise.

### Paramètres du modèle

Pour lire les données de notation, nous avons inséré un paramètre "rating_scale" pour avoir fixer un intervalle de notation.

reader = Reader(rating_scale=(1, 5))

Dans le modèle SVD, j'ai choisi de ne pas mettre de paramètre et laisser les valeurs par défauts.
Soumettre un nombre trop élevé par exemple de facteurs latents (n_factors) pour la factorisation, peut provoquer un risque d'overfitting.

## Algorithme de recommandation

### Préférences utilisateurs 

Les préférences des utilisateurs pour les genres et les réalisateurs sont construites en fonction de leurs notations. Seules les notations supérieures ou égales à 4 sont considérées pour identifier les fortes préférences.

### Recommandations pour les Couples

Le système recommande des films aux couples en combinant leurs préférences. Les préférences combinées sont utilisées pour filtrer et classer les films en fonction des notes prédites.

## Evaluation

La performance du modèle est évaluée en utilisant l'erreur quadratique moyenne (RMSE) sur le jeu de données de test.

## Conclusion

À partir de notre système de recommandation de films utilisant l'algorithme SVD et les préférences utilisateur construites, nous avons générées des résultats prometteurs.

En effet, observons que les films recommandés correspondaient aux genres préférés des utilisateurs, tels que le drame, l'action, l'aventure, la comédie et le thriller. Les prédictions de notation étaient en général très élevées, avec des films comme "American Graffiti", "Amistad", et "Indiana Jones and the Temple of Doom" recevant des prédictions de notation autour de 4.3 à 4.8, ce qui indique une forte probabilité d'appréciation par les utilisateurs.

De plus, le RMSE obtenu est d'environ 0.872, indiquant une précision de prédiction raisonnable.

## Perspectives d'Amélioration

Pour améliorer encore le système, il pourrait être bénéfique d'intégrer des fonctionnalités supplémentaires telles que les acteurs principaux ou bien la région d'où provient le film ou les utilisateurs, et d'élargir la base de données de films pour inclure une plus grande diversité de genres et de styles cinématographiques comme évoqué dans la partie précédente "Collecte et prétraitement des données".
