## Pufferfish2: Spectrum Preserving Tilings Enable Sparse and Modular Reference Indexing - 2023

[LINK](https://link.springer.com/chapter/10.1007/978-3-031-29119-7_2) 

### Authors  
Jason Fan, Jamshed Khan, Giulio Ermanno Pibiri & Rob Patro

### Résumé

#### En 2 lignes
alignement et non pseudoalignement, on cherche la position. Usage d'une nouvelle structure similaire au SPSS : le SPT (spectrum preserving tiling) (kmer -> tile) & (tile -> occurence (ref+pos)) 

#### Préliminaires
S'inscrit dans "reference guided analysis" -> known references contrairement au *de novo*\
Sert pour assemblage guidé de génome, variant calling, idendification de variants structuraux (=/= SNPs & indels), dans ces cas il est usuel de trouver une séquence seed dans le génome avant d'analyser la partie du génome en question. -> trouver kmer dans génome (w/ position)

SPT : encode où et comment les k-mers d'un SPSS apparaissent dans le texte reference, remplace dBG\
interface en amont des indexers. vient créer une couche d'abstraction entre les kmers et leurs positions dans la collec de références. Il faut ensuite map kmer -> tile et tile -> position. (rapport avec les super-kmers ?)

full-text indexes (BWT) : space vs hashing methods (dBG) : exact query time

MRP query : la mapped reference query d'un kmer x retourne une liste de (ref, pos) indiquant pour chaque reference où trouver le kmer x


#### Méthodes

1 SPT = (U, T, S, W, L)
  + U = ensemble des tiles U<sub>i</sub>, c'est un SPSS (chaque kmer du dataset sera présent dans un U<sub>i</sub>), tile = découpage d'unitig, unitig pouvant être scindé sur plusieurs tiles 
    - pufferfish(1) avait des unitigs en tant que tile, W et L étaient useless car les positions de départ etaient 0 et les lengths étaient |U<sub>i</sub>|
  + Tn fait correspondre tile et ref n (Rn)n chaque Tn,m = 1 U<sub>i</sub> dans U
  + L : séquences de longueur Ln = {Ln,1 ... Ln,m} = taille d'une seq d'une ref dans une tile
  + W : offsets, int sequences Wn = {Wn,1 ... Wn,m} = debut d'une seq d'une ref dans une tile
  + S : start pos, int sequences Sn = {Sn,1 ... Sn,m} = début des tiles
--> Chaque séquence Rn doit pouvoir être reconstruit en gluant des substrings de tiles aux offsets Wn avec des longueurs Ln

remplace dBG mais est construit à partir d'un cdBG

![figure1](/assets/pufferfish1.png)

1 small SPSS : compacte l'info de présence d'un k-mer\
1 small SPT : compacte l'info de la position d'un k-mer

2 queries :  
  + kmer-to-tile : retourne id d'une tile + la position dans la tile où le kmer apparait **k2tile(x) = (i, p)** 
    - pourrait etre fait avec SSHash
    - ou avec BWT based method (spectral BWT, BOSS)
  + tile-to-occurency : **tile2occ(i, r) = (n,s,w,l)** means que la r<sup>ème</sup> occurence de la tile U<sub>i</sub> apparait sur R<sub>n</sub> à la position (s+w) avec la séquence U<sub>i</sub>\[w:w+l\]

Algo : 
  + k2tile() = (i, p)
  + r from 0 to number_occurences of U<sub>i</sub>
  + check if kmer present by the tile-occurence given by tile2occ(i, r) 


Pufferfish2 reprend l'i


#### Résultats
