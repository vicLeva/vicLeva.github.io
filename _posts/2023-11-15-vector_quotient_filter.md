## Vector Quotient Filters: Overcoming the Time/Space Trade-Off in Filter Design - 2021

[LINK](https://dl.acm.org/doi/10.1145/3448016.3452841) 

### Authors  
    Prashant Pandey, Alex Conway, Joe Durie, Michael A. Bender, Martin Farach-Colton, Rob Johnson

### Résumé

#### En 2 lignes


#### Préliminaires

intro : résumé des filters et leurs applications


#### Méthodes


no resizable  
10% + gros que le quotient filter  
\- rapide que le cuckoo pour query et delete  

mais
ne subit pas le slow down du high load factor

In power-of-two-choice hashing, items are hashed to two blocks and placed in the emptier block  
blocks never overflows so no kicks out of blocks  


VQF = array of k blocks  
1 block = 1 minifilter of capacity s (64, 48 or 28)  

insert x: compute b1(x) et b2(x) with hash function then insert x into emptier block between b1(x) & b2(x)  
both blocks full = insertion fails   

then hash x = h(x), and divide h(x) into 
  + f(x) (remainder, r-bit fingerprint) and   
	+ B(x) (logb-bit bucket index)  
to insert f(x) into bucket B(x)  

![Alt text](/assets/vqf.png)

Ici x va etre inséré dans le block 5 car c’est le + vide
il y a 4 buckets : vert, jaune, rouge et bleu, chaque 0 représente 1 élément inséré dans le bucket correspondant 
Il y a 6 slots donc dans les metadata, 6 bits qui seront des 0 et 4 bits qui seront des 1, représentant les buckets (b+s metadata bits)

stop lecture à 3.3


#### Résultats
