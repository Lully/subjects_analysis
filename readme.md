# subjects_analysis : projet d'analyse statistique de l'indexation sujet dans des notices MARC

## Projet

Initialement, le projet vise à se doter d'outils méthodologiques, statistiques et techniques pour évaluer les différences entre une indexation manuelle et une indexation obtenue automatiquement, comme c'est le cas par exemple [à la Deutsche national Bibliothek](http://library.ifla.org/2213/)

Traditionnellement, on évalue la pertinence d'une indexation avec [la mesure du rappel et de la précision](https://fr.wikipedia.org/wiki/Pr%C3%A9cision_et_rappel) : pour faire simple, sur une requête donnée (utilisant un ou plusieurs mots clés), on évalue si tous les résultats pertinents remontent (rappel, qui mesure le silence) et si seuls des résultats pertinents remontent (précision, qui mesure le bruit).

Ces outils statistiques permettent généralement d'évaluer le choix des mots-clés (bien choisis ou non) au regard d'une base de données.

Si on veut évaluer les résultats d'une indexation automatique en utilisant rappel et précision, cela implique d'indexer en parallèle un échantillon d'ouvrages manuellement, et de comparer les résultats de l'automatisation et du manuel.

L'objectif de ce projet est d'avoir une autre approche, et de considérer l'indexation comme corpus construisant un graphe permettant de naviguer entre les documents.

## Des outils méthodologiques, statistiques et techniques

Cette affirmation est très prétentieuse : en réalité il s'agit simplement de concevoir un script Python qui réalise une extraction dans un corpus donné, pour produire des indicateurs sur la manière dont les ouvrages sont indexés, indépendamment des mots clés : en gros, l'indexation automatique indexe-t-elle "de la même manière" que l'indexation manuelle. Et si ce n'est pas le cas, quels en sont les écarts *et comment les mesurer*.

Le script *subjects_analysis* permet de :
- lancer une requête dans un SRU (en s'appuyant sur [SRUextraction](https://github.com/Lully/bnf-sru/blob/master/SRUextraction.py) (et un peu sur [stdf](https://github.com/Lully/bnf-sru/blob/master/stdf.py), qui définit quelques fonctions d'ouverture/écriture de fichiers)
- récupérer les zones d'indexation
- en extraire des données statistiques, sous forme de données brutes (décompte) et de graphique
- en extraire les données brutes d'un graphe (relations entre concepts, ou relations notices-concepts)

## Limites (en fait, ça ne va pas vraiment)

Pour l'instant, rien n'est concluant : l'outil fonctionne, mais je ne suis pas sûr de savoir quelles questions méritent d'être posées.

*Le script permet déjà de connaître, pour un corpus donné :*
- le nombre de zones d'indexation par notice (distribution), donnant à voir plutôt la "quantité" d'indexation
- le nombre de concepts par notice (distribution), évaluant également la quantité
- le nombre de concepts par zone (distribution), permettant plutôt d'évaluer la "complexité" de l'indexation

Il fournit aussi la liste des pairs de concepts (2 concepts sont liés à partir du moment où ils apparaissent dans la même notice). Mais il n'est pas certain que ce graphe soit le bon... : peut-être qu'utiliser les notices comme rebonds entre concepts serait plus pertinent ?

On pourrait aussi envisager :
- de calculer la densité de graphe ? (à condition de définir ce que c'est)
- Nombre moyen/médian de liens pour un concept. 
- Distribution (nombre de concepts liés par concept)

Tout cela n'a de sens que si on dispose de 2 corpus à comparer : en soi, aucun de ces indicateurs n'aurait de signification propre.

Je vais donc mettre en ligne 4 jeux de données pour commencer, tels que produits par ce script :
- une extraction des notices du Dépôt légal de la BnF (requête SRU utilisée, avec limite à 100.000 résultats :  http://catalogue.bnf.fr/api/SRU?version=1.2&operation=searchRetrieve&query=bib.doctype%20all%20%22a%22%20and%20bib.recordtype%20all%20%22mon%22%20and%20bib.publisher%20all%20%22DL%22%20and%20bib.publicationdate%20any%20%222016%202017%202018%22%20and%20bib.language%20all%20%22fre%22&recordSchema=unimarcxchange&maximumRecords=20&startRecord=1 -- zones 600, 606, 607)
- une extraction de l'indexation des notices de [100.000 ebooks de la BnF](https://bibliotheques.wordpress.com/2018/07/09/des-milliers-de-ebooks-et-de-liens-dans-le-catalogue-de-la-bnf/) (requête SRU : http://catalogue.bnf.fr/api/SRU?version=1.2&operation=searchRetrieve&query=bib.anywhere%20all%20%22acqnum%22&recordSchema=unimarcxchange&maximumRecords=20&startRecord=1 -- mêmes zones : 600, 606, 607)
- une extraction de l'indexation de notices récentes à la DNB (donc, logiquement, indexées automatiquement) (à faire)
- une extraction de notices plus anciennes de la DNB (donc indexées manuellement) (à faire)

Je suis preneur de toute suggestion d'ajout ou d'amélioration d'indicateurs.

## Pour plus tard

Aujourd'hui, à ma connaissance, aucune bibliothèque francophone n'utilise de mécanisme d'indexation automatique. Mais ça me semblerait intéressant, si jamais on progresse dans cette voie pour la langue française, de concevoir par avance une méthodologie d'analyse pour évaluer les résultats d'un tel processus...