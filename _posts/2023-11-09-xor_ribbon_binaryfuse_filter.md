## Xor Filter (2020), Ribbon Filter (2021) & Binary Fuse Filter (2022)

## Xor Filter (2020)

[LINK](https://arxiv.org/abs/1912.08258) 

### Authors  
THOMAS MUELLER GRAF and DANIEL LEMIRE

### Résumé

#### En 2 lignes

Data structure type bit array, slots de taille L bits et AMQ.   

#### Méthodes

structure : bloom filter avec L bits / slot au lieu de 1

item x
utilise 3 hashs sur x => adresses de 3 slots puis réalise le XOR des valeurs des 3 slots
=> *table code* de x = *h1(x)  xor  *h2(x)  xor  *h3(x)

last point : the fingerprinting hash function f(x) : x → L-bits number

query de x : check if table_code(x) == f(x)

3 cache misses / call

FP rate depend de L, égale à la proba que 2 nombre de L bits soit égaux (L = log<sub>2</sub> ε<sup>-1</sup> pour FPrate = ε)


Pour construire la table à partir de n elements à insérer, table de taille 1.23n.  
Ensuite récursivement insert(data) :
  + choisir un slot k qui n'a qu'un seul élément x qui hash vers lui, remove x de la liste des elements à insérer.  
  + insert(data - x)
  + k prendra une valeur déterminée de telle façon que le xor des 3 slots soit égale à f(x) (réalisable facilement en XOR-ant les 2 autres slots avec f(x) et insérant le résultat dans k)

Structure figée, ne peut pas ajouter des éléments au fur et à mesure comme le BF, a besoin de connaître tous les élément à la construction.

#### Résultats

\+ rapide et space efficient (à FPR égal) que le bloom filter mais nécessite de remplir le tableau en amont d’insérer et n’est pas dynamique




## Ribbon Filter (2021)

[LINK](https://arxiv.org/abs/2103.02515) 

### Authors  
Peter C. Dillinger, Stefan Walzer

### Résumé  

#### Méthodes

Use a hash function b(x) to transform keys into fixed length bitvectors.  
Now we want to build a function b'(x) such that b'(x) = b(x) for any keys in our set, and such that b'(x) is a random value for other keys. Checking if b' and b agree will be our membership test.  
They construct b'(x) as h(x)·Z, where h is a vector with binary entries and Z is a matrix with binary entries.  
By taking all the elements we want and computing h(x) for each of them we get a bunch of vectors that we can arrange into a matrix H. Z can be found by gaussian elimination on that matrix.  
The name ribbon filter comes from the fact that H is constructed in a particular way that gives it a ribbon (ruban) shape. That shape makes the whole thing efficient in time and space.   




## Binary Fuse Filter (2022)

[LINK](https://arxiv.org/abs/2201.01174) 

### Authors  
THOMAS MUELLER GRAF and DANIEL LEMIRE

### Résumé

#### En 2 lignes

Better XOR, by using hash distribution differently

#### Méthodes

from xor filter mais résoud le problème du temps de construction du xor filter  
xor filter : choisis troi valeur de hash de facon random uniform sur tout l'array
binary fuse filter : 
  + choisir une window de taille w, random au sein de l'array
  + choisir 3 adresses aléatoires au sein de cette window (en gardant un peu d'espace entre ces 3 adresses)


passage de 3 hash functions à 4 for free car on a improve la locality


Intuitively, because of how we assigned hashes, the slots closest to the two ends of the array are the ones most likely to only have one item hash to them. Why? Because the only way you can have collisions near the ends is if you have two items whose windows are both very close to the ends and that happen to pick slots within those windows that are far to the sides of the windows. That’s pretty unlikely, and as a result the leftmost and rightmost items are likely to be peeled.

But once those items are peeled, by the same logic, the items that are now on the far left or far right are likely to be peelable. This is where the name “fuse” comes from - it’s like lighting a fuse at both ends and watching it burn in toward the center.

The fact that there’s some predictability here about where items will be peelable means that we need fewer table slots than if the hashes were assigned randomly across the table. The paper cites a space usage of around 1.13n table slots needed, a big improvement over the 1.23n needed for an XOR filter, and it’s done purely by changing the strategy for assigning hashes. Pretty neat! (from [here](https://stackoverflow.com/questions/73410580/what-is-a-binary-fuse-filter))


#### Résultats

\+ better Xor filter, space and speed wise