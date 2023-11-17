## HyperLogLog: the analysis of a near-optimal cardinality estimation algorithm - 2007

[LINK](https://algo.inria.fr/flajolet/Publications/FlFuGaMe07.pdf) 

### Authors  
Philippe Flajolet and Éric Fusy and Olivier Gandouet and Frédéric Meunier

### Résumé

#### En 2 lignes


#### Préliminaires

Hyper log log algorithm : permet de compter rapidement et avec 2% d’erreur sur 109 éléments le nombre d’éléments uniques. (utilise peu de mémoire également)

#### Méthodes

**1ere intuition** : Flajolet-Martin (Flajolet, Philippe; Martin, G. Nigel (1985). “Probabilistic counting algorithms for data base applications”)

Hash chaque element sur 6 chiffres, et on garde en mémoire le nombre D max de leading zeros aperçu.  
( leading-zeros(001598) = 2,   leading-zeros(651598) = 0 )  
Lorsqu’on a vu 10 élements différents, on peut espérer avoir D = 1,  
100 éléments : D=2,  
10<sup>K</sup> éléments : D=K.  


Dans l’algo  Flajolet-Martin, le hash est réalisé en binaire pour avoir 2<sup>K</sup> éléments : D = K,  
si on a en mémoire D = 5 leading zeros au max, alors on a surement vu 2<sup>5</sup> = 32  éléments différents.

Avec facteur de correction pour éviter un biais prédictible :  
Cardinality Flajolet-Martin (nb éléments uniques) = 2<sup>K</sup> / ɸ  
ɸ ~= 0.77351 

Solution pour éviter que les outliers ruinent tout: utiliser une combinaison de m fonctions de hash (computationally more expensive)  



**2nde intuition** : Durand, Marianne; Flajolet, Philippe (2003). “[Loglog Counting of Large Cardinalities](http://algo.inria.fr/flajolet/Publications/DuFl03-LNCS.pdf)”  

On utilise les n premiers bits des hash pour indiquer l’adresse d’un bucket, et on réalise le comptage de leading zeros dans ce bucket, indépendamment des autres. On réalise à la fin la somme des buckets qu’on divise par le nombre de buckets (moyenne) 

![Alt text](/assets/hyploglog.png)


Cardinality LogLog = m ( 2<sup>K1 + K2 + … + Km</sup> / ɸ )   m = 2<sup>n</sup>, le nombre de buckets  
Optimisation: n’utiliser que 70% des valeurs des buckets, les 70% plus petites  

Cardinality<sub>SuperLogLog</sub> = 0.7m * ( 2<sup>avg (70% smaller values)</sup> / ɸ )


**3 : Hyper LogLog** : Flajolet, Philippe; Fusy, Éric; Gandouet, Olivier; Meunier, Frédéric (2007). [HyperLogLog: the analysis of a near-optimal cardinality estimation algorithm](http://algo.inria.fr/flajolet/Publications/FlFuGaMe07.pdf)

Au lieu d’utiliser une moyenne géométrique sur les 70% plus petites values, on réalise la moyenne harmonique 

moyenne harmonique de 1, 2, 4: 
3  /  (1/1 + 1/2 + 1/4)  =  3 / (1.75)  =  1.714

Elle permet de gérer les outliers : 

moyenne harmonique de 2, 4, 6, 100:
4 / (1/2 + 1/4 + 1/6 + 1/100) = 4.32


**Cardinality<sub>HyperLogLog</sub> = m * ( harmonic mean(2<sup>Li</sup>) / ɸ )**

Dans loglog, superloglog et hyperloglog, on utilise la position du leftmost 1 (1 + #leading zeros)
