L’algorithme BM25 est une version améliorée du TF-IDF prenant compte de la longueur des documents lors du classement. Cet algorithme étant déjà très efficace sur le corpus, il est compliqué d’améliorer ses performances.

Parmi les différentes approches testées (différent style de tokenisation, modification de BM25, vectorisation des mots), une seule s’est réellement montrée convaincante et a eu des performances supérieures à BM25.

Cette approche est inspirée d’un article scientifique [1] démontrant l’efficacité de l’association du modèle BERT et de l’algorithme dans certains cas. Les résultats des deux algorithmes sont combinées à l’aide de la formule suivante : 
	

Cette formule est utilisée pour calculer le score final s(p) d'un passage p en interpolant deux scores différents : le score normalisé donné par le modèle BM25 sBM25(p) et le score donné par un modèle BERT sBERT(p).

Il a donc fallu implémenter BERT, convertir le corpus et les requêtes en vecteurs grâce au word embedding et créer la fonction de  calcul de score qui est une similarité cosinus.

Ci-dessous, le ndcg sur l'entièreté du corpus en fonction du paramètre alpha.

![téléchargé](https://github.com/runpaul/nlp_esilv/assets/115584547/df5332d7-8651-4002-aa60-5a3bdfad5d9d)


Lorsque alpha est à 0, cela signifie que seuls les scores BERT sont utilisés pour le classement. À mesure qu'alpha augmente, la contribution des scores BM25 devient plus importante.
On observe que le NDCG augmente avec alpha jusqu'à un certain point, indiquant qu'introduire les scores BM25 améliore la qualité des résultats de classement par rapport à l'utilisation exclusive de BERT.
Cependant, à un certain niveau d'alpha (autour de 0.4 à 0.5), l'augmentation du NDCG ralentit et semble atteindre un plateau, ce qui suggère qu'au-delà de ce point, l'ajout de davantage de poids au score BM25 n'améliore pas significativement la qualité du classement.
Ce plateau peut indiquer qu'un équilibre optimal entre les scores BERT et BM25 est atteint pour le NDCG. Le NDCG maximum est atteint pour alpha = 0.9 et est de 0.47. 



Conclusion
Lien de notre Google Collab : https://colab.research.google.com/drive/14H_Q5CsSoXo7Vflo1omwPSgdd3IxEMJ7?usp=sharing


Niveau performance sur le premier code fourni (BM25) :
Sur l’ensemble des documents (nb_docs=3192), on a un socre ndcg de 0.46

Niveau de performance suite aux modifications :
Sur l’ensemble des documents (nb_docs=3192), on a un socre ndcg de 0.47

Notre code introduit donc une légère avancée par rapport au système fourni qui se basait uniquement sur le modèle BM25 pour le classement des documents. En intégrant les embeddings BERT, nous avons enrichi le processus de récupération d'informations en y ajoutant une dimension de contexte. Les embeddings BERT offrent des représentations vectorielles qui capturent les nuances sémantiques et les relations entre les mots, permettant ainsi d'aller au-delà de la simple fréquence des termes qu’on avait avec BM25. 
Ce calcul est désormais fondé sur une combinaison linéaire des scores BM25 et des scores issus de BERT, avec le paramètre alpha servant à ajuster finement leur contribution relative. Ce paramètre est crucial car il permet de trouver l'équilibre optimal entre la précision de la correspondance des termes de BM25 et la compréhension du langage naturel apportée par BERT.

En conclusion, notre méthode vise à exploiter à la fois la fiabilité de BM25 pour la correspondance exacte des termes et l'aptitude de BERT à évaluer la pertinence dans un contexte donné. Cette combinaison offre une approche de classement des documents plus nuancée et permet une meilleure performance.




REFERENCES

[1] Shuai Wang, Shengyao Zhuang, and Guido Zuccon. 2021. BERT-based Dense Retrievers Require Interpolation with BM25 for Effective Passage Retrieval. In Proceedings of the 2021 ACM SIGIR International Conference on Theory of Information Retrieval (ICTIR '21). 
