## Pebblescout : Indexing and searching petabyte-scale nucleotide resources - 2023

[LINK](https://www.biorxiv.org/content/10.1101/2023.07.09.547343v1) 

### Authors  
Sergey A. Shiryev, Richa Agarwala

### Résumé

#### En 2 lignes

Série d'outils déjà disponible sur le ncbi à cette adresse : [https://pebblescout.ncbi.nlm.nih.gov/](https://pebblescout.ncbi.nlm.nih.gov/). 

#### Préliminaires

"subject" : genre de token, ce qui est indexé
  + "metagenomic runs" de SRA : subject
  + 1000 Genome project :
    * subject peut etre simple run aussi
    * ou tous les runs d'un BioSample sont un subject

index + metadata d'un grp de subjects : *database*

##### Réseau de feistel : 

![feistelcipher](/assets/feistelcipher.png)

Plain text encodé en ASCII puis divisé en 2 parties : R et L. Plusieurs blocks de chiffrement, pendant lesquels R restera inchangé, mais deviendra L au prohain block. L quand à lui sera le résultat d'un XOR entre L et la sortie d'une fonction *f* définie prenant en entrée R et une clef K extérieure. A la fin de n blocks, R et L sont concaténés pour obtenir un texte chiffré. Meme principe pour déchiffrement, avec ciphertext en entrée au lieu du plain text.  
--> + de rounds = + secure mais plus long à process


##### B+ tree : 

variante du Btree (balanced tree) où ce sont uniquement les feuilles qui contiennent l'info (les pointeurs vers l'info). Les noeuds internes sont là uniquement pour guider la recherche de manière efficace. Les B+ tree sont efficaces pour représenter un dataset avec des acces calmes à l'information, fast queries.  
--> internal nodes for indexing & leaves for storage



#### Méthodes

3 main stages : 
  1. récupération + hash de 25-mer avec réseau de feistel. (chaque 25-mer du subjet) 
    + parmis les 18 25-mers d'un 42-mer du subject, on garde uniquement celui qui a le hash le + petit (**minimiser**)
    + les minimisers choisis sont appelés *sampled*

  2. agrégation des 25-mers
    + subjects ids sont encodés avec huffmann code (encodage plus efficace pour les plus récurrents / ceux qui ont le plus de 25-mers différents dans la partie harvest)
    + pour chaque 25-mer, on save la liste de tous les subjects (ids) dans lesquels le 25-mer a été trouvé (inverted index?)

  3. 2 layers B+ tree to index aggregated 25-mers. le haut de l'arbre est stocké en local tandis que les feuilles (storage) sont dans un système de stockage type *random access*


#### Résultats
