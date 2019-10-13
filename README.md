---
title: "Le chi²"
author: "Stéphane Jaubert"
date: ""
output:
  html_document: default
  pdf_document: default
---

Le $\chi²$ (synonymes X², Chi2, Khi2, Chi², Khi²) permet de comparer des répartitions d'effectifs. 
    On distingue :
le $\chi²$ de conformité qui permet de comparer un effectif à une valeur théorique attendue.
les $\chi²$ d'homogénéité et d'indépendance qui permettent de voir si un effectif peut être dû au seul hasard.

** $\chi²$ de conformité**
   

Définition du $\chi²$ de conformité
    
Le Khi2 de conformité permet de savoir si il y a correspondance entre la théorie et une répartition observée. Le test du Khi-deux permet donc de voir si un échantillon est conforme à la théorie ou s'il en diffère significativement.
- L'hypothése nulle (H0) est : la théorie diffère de l'observation. Le résultat du $\chi²$ de conformité permet de rejeter ou non H0.
- Résultats a p-value donne la probabilité de non-validation de H0 - la probabilité de voir une conformité entre la théorie et l'observation.
- Plus p-value est petite, plus la théorie et l'observation diffèrent.
X-square, cette valeur classique renvoyée par un test de Khi2 permet de retrouver manuellement la p-value en s'aidant d'un tableau disponible dans tout bon livre de statistiques.
            
## Exemple de $\chi²$ de conformité
            
```{r}
# Descendance observée
descendance <- c(7,32)
# probabilités théoriques : d'un point de vue théorique, 
#on devrait avoir 3/4 et 1/4 soit 75 et 25%
proba <- c(0.25,0.75)
# Réalisation du test de khi-deux
chisq.test(descendance,p=proba)
# On récupère la valeur du Khi2 1.03 et aussi la probabilité d'avoir une #telle situation p=0.3 ==> Plus de 30%  de situation oé l'hypothèse serait #rejetée à tort. Evènement similaire à H0
```

Ainsi, la p-value ici nous permet d'en déduire que les résultats observés sont conformes à la théorie. 

Ou du moins, on ne peut affirmer qu'ils ne sont pas conformes à la théorie



** $\chi²$ d'indépendance**
- Le Khi2 d'indépendance permet de savoir si il y a indépendance entre 2 critères susceptibles de créer une différence de répartition.

- L'hypothèe nulle (H0) est : le fait de connaître  l'appartenance d'un individu à une population (selon un critère) ne donne aucun indice sur la caractéristique qui le défini selon l'autre critère.

Par exemple : est-ce que le fait de connaître la couleur des yeux de quelqu'un me permet de supposer sur son sexe ? Réponse : non.

Par exemple : est-ce que le fait de connaître la taille de quelqu'un permet de supposer sur son sexe ? Réponse : oui, plus un individu est petit, plus il y a des chances que ce soit une femme.

Résultats
La p-value donne la probabilité de validation de H0 - la probabilité de ne voir aucun lien entre les critères. Plus p-value est petite, plus il y a un lien entre les critères (et donc pas d'indépendance).

X-square, cette valeur classique renvoyée par un test de Khi2 permet de retrouver manuellement la p-value en s'aidant d'un tableau disponible dans tout bon livre de statistiques.


```{r}
# Créations des vecteurs correspondant aux 2 catégories :
hommes = c(50,70,110,60)
femmes = c(80,75,100,30)
# Création d'une matrice comparative :
tableau = matrix(c(hommes, femmes),2,4,byrow=T) # (2 : nombre de lignes et 4 nombres de colonnes (tranches salariales))
tableau
# Réalisation du test khi-deux - les résultats sont sauvegardés dans "khi_test"
khi_test = chisq.test(tableau)
khi_test # affiche le résultat du test
```


Ainsi, la p-value ici est de 0.0005 : il y a donc un lien statistique entre le sexe et la tranche salariale car la p-value est très petite.



** $\chi²$ d'homogénéité**

Le $\chi²$ d'homogénéité est un khi d'indépendance. Il est seulement réalisé dans un but différent.  

Le Khi2 d'homogénéité permet de vérifier que les répartitions de différents effectifs sont équivalentes.

Ce test repose ainsi sur 2 hypothèses :

- H0 : il n'y a pas de différence significative dans la répartition des 3 groupes étudiés

- H1 : il y a une différence - cette hypothèse est a affiner en fonction du cas étudié (cf. exemple ci-dessous)

```{r}
# Créations des vecteurs correspondant aux 2 catégories :
pedago1 = c(51, 29)
pedago2 = c(38, 12)
pedago3 = c(86, 34)
# Création d'une matrice comparative :
tableau = matrix(c(pedago1,pedago2,pedago3),3,2,byrow=T) # (3 : nombre de lignes et 2 nombres de colonnes)
# autre écriture de la ligne précédente 
tableau = rbind(pedago1,pedago2,pedago3)

# Réalisation du test khi-deux - les résultats sont sauvegardés dans "khi_test"
khi_test = chisq.test(tableau)
khi_test # affiche le résultat du test
```
```{r}
###########################################################################################
#Tableau de Contingence

tableau<-matrix(c(1768,807,189,47,946,1387,746,53,115,438,288,16),ncol = 4,byrow = T)
tableau
rownames(tableau)<-c("bleu","gris ou vert","marron")
colnames(tableau)<-c("blond","chatain","noir","roux")
tableau
ni.<-margin.table(tableau,1)
ni.
n.j<-margin.table(tableau,2)
n.j
sum(ni.)
sum(n.j)
n..<-sum(ni.)
n..
addmargins(tableau)

(freqabs<-round(tableau/n..*100,2))

res<-chisq.test(tableau)

#####################################################################
```

