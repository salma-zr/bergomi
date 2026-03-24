# Questions probables a l'oral — Bergomi 2.4 a 2.4.6

Ce document est different de `questions_reponses_bergomi_2_4.md`.

- Le fichier precedent est une **banque large de questions-reponses**.
- Ici, l'objectif est de te preparer a **ce qu'un enseignant peut te demander a l'oral**, avec :
  - une formulation plausible de la question ;
  - une reponse detaillee mais parlable ;
  - ce qu'il faut absolument dire ;
  - le risque classique a eviter.

Le niveau vise est : **M2 Probabilites & Finance / finance quantitative**, avec articulation entre **Bergomi** et **le cours**.

---

## 1) "Quel est exactement l'objectif de cette section 2.4 ?"

### Reponse detaillee

L'objectif de la section 2.4 est d'etudier le probleme inverse de Dupire.

Habituellement, on part de la surface de volatilite implicite de marche et, via la formule de Dupire, on reconstruit une fonction de volatilite locale \(\sigma_{\mathrm{loc}}(t,S)\).  
Ici, Bergomi suppose au contraire que la fonction de volatilite locale est donnee, et il cherche a comprendre :

1. quelle surface implicite cette fonction engendre ;
2. comment cette surface implicite reagit quand le spot bouge ;
3. pourquoi les dynamiques de smile produites par local vol sont structurellement contraintes.

Le point fort de la section est qu'elle ne s'arrete pas a une relation statique entre local vol et implicite : elle explique aussi la **dynamique du smile**.

### A dire absolument

- "C'est le probleme inverse de Dupire."
- "Le vrai enjeu est la dynamique du smile, pas seulement le calibrage statique."

### Piege a eviter

Ne pas dire seulement : "on exprime l'implicite en fonction de la local vol".  
Ce serait vrai, mais incomplet. Il faut mentionner **la dynamique induite**.

---

## 2) "Quelle est l'identite fondamentale de la section ?"

### Reponse detaillee

L'identite fondamentale est :
\[
\hat{\sigma}_{K,T}^{\,2}
=
\frac{
\mathbb E^{\mathrm{loc}}
\!\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}(\hat{\sigma}_{K,T})
\sigma_{\mathrm{loc}}^2(t,S_t)\,dt
\right]
}{
\mathbb E^{\mathrm{loc}}
\!\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}(\hat{\sigma}_{K,T})\,dt
\right]
}.
\]

Elle dit que la **variance implicite** est une **moyenne ponderee** de la **variance locale**.

Le poids est le dollar gamma actualise de l'option dans le modele de base Black-Scholes, pris le long des trajectoires du modele local vol.

Ce resultat est exact, mais il reste implicite puisque \(\hat{\sigma}_{K,T}\) apparait aussi dans les poids.

### A dire absolument

- "La formule exacte porte sur \(\hat{\sigma}^2\), pas directement sur \(\hat{\sigma}\)."
- "Les poids sont de type dollar gamma."

### Piege a eviter

Ne pas dire : "l'implicite est la moyenne de la local vol".  
**Exactement**, c'est la variance implicite qui est moyenne de **variance locale**.

---

## 3) "D'ou vient cette identite ?"

### Reponse detaillee

Elle vient d'un argument de couverture delta.

On choisit un modele de base \(P_1\), puis on regarde ce qui se passe si la vraie dynamique du sous-jacent suit un autre modele. En appliquant Itô a
\[
Q_t=e^{-rt}P_1(t,S_t),
\]
on obtient que la derivee de \(Q_t\) depend d'un terme proportionnel a
\[
\frac12 e^{-rt}S_t^2\Gamma_t\big(\sigma_{\text{vraie}}^2-\sigma_{\text{base}}^2\big).
\]

En integrant ce P\&L gamma/theta et en prenant l'esperance, on obtient la difference de prix entre les deux modeles.

Ensuite, si le modele de base est Black-Scholes avec volatilite egale a la volatilite implicite de l'option, alors les deux prix coïncident au temps 0, ce qui permet d'isoler \(\hat{\sigma}_{K,T}^2\).

### A dire absolument

- "C'est un argument de P\&L de couverture delta."
- "Le terme cle est le gamma/theta."

### Piege a eviter

Ne pas parler d'une simple "astuce de calcul".  
C'est une **identite de pricing** obtenue via une couverture.

---

## 4) "Pourquoi le gamma apparait-il naturellement ?"

### Reponse detaillee

Le gamma apparait parce que l'erreur de modelisation de la volatilite ne se voit pas au premier ordre en \(dS_t\), mais au second ordre.

Quand on couvre delta, le risque lineaire en \(dS_t\) est neutralise. Ce qui reste vient du terme quadratique de la formule d'Itô :
\[
\frac12 \partial_{SS}P\, d\langle S\rangle_t.
\]

Comme
\[
d\langle S\rangle_t=\sigma_t^2 S_t^2 dt,
\]
on obtient naturellement un terme en
\[
S_t^2\Gamma_t.
\]

C'est donc la sensibilité de second ordre de l'option qui mesure l'impact d'un mauvais modele de volatilite instantanee.

### A dire absolument

- "Le delta neutralise le premier ordre."
- "Le mauvais choix de volatilite se voit dans le second ordre."

### Piege a eviter

Ne pas confondre gamma et vega ici.  
La derivation de Bergomi passe d'abord par **le gamma/theta**, pas par un raisonnement de vega.

---

## 5) "Quel est le lien avec le cours sur Itô et Feynman-Kac ?"

### Reponse detaillee

Le lien est direct.

1. **Itô** est utilise pour calculer la dynamique de \(e^{-rt}P(t,S_t)\).
2. **L'EDP de pricing** du modele de base permet de simplifier les termes de drift.
3. **Feynman-Kac** justifie que la solution de l'EDP coincide avec l'esperance risque-neutre du payoff.

Donc cette section est en fait une application tres naturelle du triptyque de cours :

- dynamique du sous-jacent ;
- formule d'Itô ;
- EDP de pricing / Feynman-Kac.

### A dire absolument

- "Bergomi reutilise exactement les outils centraux du cours."

### Piege a eviter

Ne pas presenter la section comme purement "heuristique de praticien".  
Elle repose sur une base mathematique standard du cours.

---

## 6) "Pourquoi l'identite exacte n'est-elle pas suffisante en pratique ?"

### Reponse detaillee

Parce qu'elle est **implicite**.

Le membre de droite depend lui-meme de \(\hat{\sigma}_{K,T}\), a travers le gamma Black-Scholes et le poids de l'averaging. Donc on n'obtient pas directement une formule fermee de l'implicite.

En plus, numeriquement, la dependance de la densite a la local vol est importante. Or une approximation trop naive peut mal capturer cette dependance, surtout sur les smiles equity qui sont assez marques.

Donc en pratique, la formule exacte est surtout utile pour :

- comprendre la structure du probleme ;
- construire des approximations analytiques ;
- interpreter la dynamique du smile.

### A dire absolument

- "La formule est exacte mais implicite."
- "Le vrai probleme pratique est la dependance de la densite a \(\sigma_{\mathrm{loc}}\)."

### Piege a eviter

Ne pas dire que la formule exacte est "inutilisable".  
Elle est tres utile conceptuellement, mais pas suffisante pour du pricing numerique precis.

---

## 7) "Que veut dire 'weakly local vol' ?"

### Reponse detaillee

Cela signifie que la dependance en spot de la volatilite locale est **faible**, au sens ou on peut ecrire
\[
\sigma_{\mathrm{loc}}(t,S)=\sigma_0+\delta\sigma(t,S),
\]
avec \(\delta\sigma\) petit, ou bien au niveau variance
\[
u(t,S)=u_0(t)+\delta u(t,S).
\]

On fait alors un developpement perturbatif au premier ordre en \(\delta\sigma\) ou \(\delta u\).  
L'idee est de geler la densite dans un modele de reference simple, en general Black-Scholes, pour obtenir une formule analytique exploitable.

### A dire absolument

- "C'est un developpement perturbatif."
- "On linearise autour d'un modele de reference."

### Piege a eviter

Ne pas dire que cela signifie "la volatilite locale est presque constante dans le temps".  
Le point important est surtout la **faible dependance en spot**.

---

## 8) "Quelle est la formule approchée principale obtenue ?"

### Reponse detaillee

Au premier ordre autour d'une volatilite constante \(\sigma_0\), Bergomi obtient :
\[
\hat{\sigma}_{K,T}
\approx
\frac1T\int_0^Tdt\int_{\mathbb R}\phi(y)\,
\sigma_{\mathrm{loc}}\!\left(
t,\,
F_t\exp\!\left(\frac{t}{T}x_K+\sigma_0\sqrt{\frac{(T-t)t}{T}}\,y\right)
\right)dy.
\]

Cette formule donne une interpretation geometrique tres forte :

- on moyenne la local vol sur des niveaux intermediaires \(S(t,y)\) ;
- ces niveaux joignent \(S_0\) au strike \(K\) ;
- la variable \(y\) correspond a une dispersion gaussienne autour du chemin central.

### A dire absolument

- "C'est une moyenne gaussienne de la local vol."
- "Le chemin central relie \(S_0\) a \(K\) en log-espace."

### Piege a eviter

Ne pas oublier de preciser qu'il s'agit d'une formule **d'ordre 1**.

---

## 9) "Comment interpretez-vous le chemin \(y=0\) ?"

### Reponse detaillee

Le chemin \(y=0\) est le chemin central, souvent interprete comme le **chemin le plus probable** dans cette approximation.

Si on ne garde que \(y=0\), on obtient une formule encore plus simple :
\[
\hat{\sigma}_{K,T}\approx \frac1T\int_0^T
\sigma_{\mathrm{loc}}\!\left(t,F_t e^{(t/T)x_K}\right)\,dt.
\]

Cette approximation est trop crude pour du calcul precis, mais elle est excellente pour l'intuition :

- l'implicite regarde essentiellement ce qui se passe le long d'une trajectoire qui part de \(S_0\) et finit en \(K\) ;
- ce n'est donc jamais une simple evaluation ponctuelle de \(\sigma_{\mathrm{loc}}\).

### A dire absolument

- "Le chemin \(y=0\) donne une intuition, pas une formule numeriquement fiable."

### Piege a eviter

Ne pas presenter le chemin \(y=0\) comme un resultat exact.

---

## 10) "Pourquoi developper la local vol autour du forward ?"

### Reponse detaillee

Parce que la zone la plus importante pour le smile est souvent le voisinage du **forward ATMF**.

On ecrit alors
\[
\sigma_{\mathrm{loc}}(t,S)=\bar{\sigma}(t)+\alpha(t)x+\frac{\beta(t)}{2}x^2,
\qquad x=\ln(S/F_t).
\]

Ce developpement permet de lire directement :

- le niveau local via \(\bar{\sigma}(t)\) ;
- le skew local via \(\alpha(t)\) ;
- la courbure locale via \(\beta(t)\).

Ensuite, en injectant cela dans la formule approchée, on obtient des expressions analytiques du skew implicite et de la courbure implicite.

### A dire absolument

- "On se place autour du forward car c'est le point naturel pour le smile ATMF."

### Piege a eviter

Ne pas oublier que c'est un developpement **local** : il ne decrit pas tout le smile loin du money.

---

## 11) "Quel est le resultat fondamental sur le skew implicite ?"

### Reponse detaillee

Le resultat fondamental est :
\[
\left.\frac{\partial\hat{\sigma}_{K,T}}{\partial\ln K}\right|_{K=F_T}
=
\frac1T\int_0^T\frac{t}{T}\alpha(t)\,dt.
\]

Cela signifie que le skew implicite ATMF est une **moyenne ponderee** du skew local instantane \(\alpha(t)\).

Le poids est \(t/T\), donc les instants proches de l'echeance comptent davantage.

Si \(\alpha(t)\) est constant, on retrouve :
\[
\left.\frac{\partial\hat{\sigma}_{K,T}}{\partial\ln K}\right|_{K=F_T}=\frac{\alpha}{2}.
\]

### A dire absolument

- "Le skew implicite ne reproduit pas le skew local point par point."
- "Il en prend une moyenne temporelle ponderee."

### Piege a eviter

Ne pas dire que le poids est uniforme.

---

## 12) "Pourquoi le poids est-il \(t/T\) ?"

### Reponse detaillee

Parce qu'une variation du strike final ne contraint pas la trajectoire de la meme facon a toutes les dates.

- Au debut de la vie de l'option, la contrainte terminale \(K\) est encore lointaine.
- A mesure qu'on se rapproche de \(T\), cette contrainte devient plus forte.

Le facteur \(t/T\) traduit exactement cette influence croissante du strike terminal au cours du temps.

### A dire absolument

- "Le strike se fait sentir de plus en plus a mesure qu'on approche de l'echeance."

### Piege a eviter

Ne pas donner une justification purement algebrique ; l'intuition temporelle est importante a l'oral.

---

## 13) "Quel est le resultat sur la reaction du smile a un mouvement du spot ?"

### Reponse detaillee

Pour un strike fixe, Bergomi obtient :
\[
\left.\frac{\partial\hat{\sigma}_{K,T}}{\partial\ln S_0}\right|_{K=F_T}
=
\frac1T\int_0^T\left(1-\frac{t}{T}\right)\alpha(t)\,dt.
\]

Cette fois, le poids est \(1-t/T\), donc ce sont les temps courts qui dominent.

L'interpretation est naturelle : un choc de spot agit d'abord au debut de la trajectoire, avant que la contrainte terminale du strike ne prenne le dessus.

### A dire absolument

- "Le poids complementaire \(1-t/T\) s'oppose au poids \(t/T\) du skew en strike."

### Piege a eviter

Ne pas confondre cette quantite avec la dynamique ATMF.

---

## 14) "Comment obtient-on la dynamique de la vol ATMF ?"

### Reponse detaillee

Il faut utiliser la regle de chaine, car le strike ATMF depend lui-meme du spot :
\[
K=F_T(S_0).
\]

Donc
\[
\frac{d\hat{\sigma}_{F_T,T}}{d\ln S_0}
=
\left.\frac{\partial\hat{\sigma}_{K,T}}{\partial\ln S_0}\right|_{K=F_T}
+
\left.\frac{\partial\hat{\sigma}_{K,T}}{\partial\ln K}\right|_{K=F_T}.
\]

En remplaçant les deux formules precedentes, on obtient :
\[
\frac{d\hat{\sigma}_{F_T,T}}{d\ln S_0}
=
\frac1T\int_0^T\alpha(t)\,dt.
\]

Donc la dynamique de la vol ATMF est gouvernee par la **moyenne temporelle uniforme** du skew local.

### A dire absolument

- "Pour l'ATMF, toutes les dates comptent avec le meme poids."

### Piege a eviter

Ne pas oublier le terme venant de la dependance de \(K\) en \(S_0\).

---

## 15) "Pourquoi dit-on que \(\alpha(t)\) est le parametre central ?"

### Reponse detaillee

Parce que \(\alpha(t)\) gouverne simultanement :

1. le skew implicite ATMF :
   \[
   \left.\partial_{\ln K}\hat{\sigma}\right|_{ATMF};
   \]
2. la reaction a spot fixe :
   \[
   \left.\partial_{\ln S_0}\hat{\sigma}\right|_{ATMF};
   \]
3. la dynamique de la vol ATMF :
   \[
   \frac{d\hat{\sigma}_{F_T,T}}{d\ln S_0};
   \]
4. le ratio \(R_T\).

Autrement dit, la structure par terme du skew local impose la structure par terme de la dynamique du smile implicite.

### A dire absolument

- "Toute la dynamique de smile passe essentiellement par \(\alpha(t)\)."

### Piege a eviter

Ne pas sur-vendre \(\beta(t)\) : il joue surtout sur la courbure, pas sur la dynamique principale ATMF.

---

## 16) "Comment definir \(R_T\) et comment l'interpreter ?"

### Reponse detaillee

On definit
\[
R_T=
\frac{
\dfrac{d\hat{\sigma}_{F_T,T}}{d\ln S_0}
}{
\left.\dfrac{\partial\hat{\sigma}_{K,T}}{\partial\ln K}\right|_{K=F_T}
}.
\]

Il mesure de combien la vol ATMF bouge, **exprimee en unites de skew ATMF**.

Si on remplace par les formules precedentes :
\[
R_T=
\frac{\int_0^T \alpha(t)\,dt}
{\int_0^T \frac{t}{T}\alpha(t)\,dt}.
\]

Donc \(R_T\) quantifie la **rigidite du smile**.

### A dire absolument

- "C'est une mesure de smile dynamics."
- "Il compare mouvement de l'ATMF et pente du smile."

### Piege a eviter

Ne pas donner seulement la formule : il faut dire ce qu'elle mesure.

---

## 17) "Que signifient sticky-strike et sticky-delta dans ce cadre ?"

### Reponse detaillee

- **Sticky-strike** : les volatilites implicites a strike absolu fixe ne bougent pas quand le spot bouge.  
  Dans ce cas, la vol ATMF se deplace exactement du montant donne par la pente du smile, donc \(R_T=1\).

- **Sticky-delta** : les volatilites restent fixes pour une moneyness conservee.  
  Dans ce cas, la vol ATMF ne bouge pas quand le spot bouge, donc \(R_T=0\).

Le modele local vol produit souvent un comportement plus rigide, intermediaire ou meme au-dessus de sticky-strike, typiquement avec \(R_T\approx 2\) a courte maturite.

### A dire absolument

- "Sticky-delta correspond a \(R_T=0\)."
- "Sticky-strike correspond a \(R_T=1\)."

### Piege a eviter

Ne pas inverser les deux regimes.

---

## 18) "Pourquoi parle-t-on de la regle \(R=2\) ?"

### Reponse detaillee

Si \(\alpha(t)\) est a peu pres constante sur les petites maturites, alors
\[
\frac{d\hat{\sigma}_{F_T,T}}{d\ln S_0}\approx \alpha,
\qquad
\left.\frac{\partial\hat{\sigma}_{K,T}}{\partial\ln K}\right|_{K=F_T}\approx \frac{\alpha}{2}.
\]

Par consequent,
\[
R_T\approx 2.
\]

Cela signifie qu'en local vol, la vol ATMF bouge environ **deux fois plus vite que le skew ATMF**, en un sens normalise.

C'est un resultat tres connu car il montre que local vol tend a produire un smile **trop rigide** par rapport a ce qui est souvent observe sur les marches actions.

### A dire absolument

- "La regle \(R=2\) est un signal de rigidite excessive du smile sous local vol."

### Piege a eviter

Ne pas presenter \(R=2\) comme une loi universelle absolue.  
C'est un resultat de ce cadre et un comportement typique a courte maturite.

---

## 19) "Quel est l'interet du cas loi de puissance pour \(\alpha(t)\) ?"

### Reponse detaillee

Il permet de decrire la structure par terme du skew.

Si
\[
\alpha(t)\sim \alpha_0\left(\frac{\tau_0}{t}\right)^\gamma,
\]
alors le skew implicite ATMF se comporte comme
\[
\left.\frac{\partial\hat{\sigma}_{K,T}}{\partial\ln K}\right|_{K=F_T}
\sim
\frac{1}{2-\gamma}\alpha_0\left(\frac{\tau_0}{T}\right)^\gamma.
\]

Donc :

- le skew implicite decroit avec le **meme exposant** \(\gamma\) que le skew local ;
- \(\alpha(t)\) ne controle pas seulement le niveau du skew, mais aussi sa **structure par terme**.

### A dire absolument

- "L'exposant de decroissance se transmet du local vers l'implicite."

### Piege a eviter

Ne pas oublier le cutoff \(\tau_0\), necessaire pour regulariser le comportement pres de 0.

---

## 20) "Quel est le resultat exact de courte maturite ?"

### Reponse detaillee

Quand \(T\to 0\), Bergomi obtient un resultat exact :
\[
\frac{1}{\hat{\sigma}(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm{loc}}(0,S)}.
\]

Donc l'inverse de l'implicite est la moyenne harmonique de l'inverse de la local vol, prise en variable logarithmique entre \(S_0\) et \(K\).

Ce resultat est tres fort, car il n'est plus perturbatif : ce n'est pas un developpement d'ordre 1, c'est une asymptotique exacte.

### A dire absolument

- "A courte maturite, la bonne moyenne est harmonique."

### Piege a eviter

Ne pas dire que c'est une moyenne de \(\sigma\) ou de \(\sigma^2\).

---

## 21) "Pourquoi la moyenne harmonique est-elle naturelle ici ?"

### Reponse detaillee

Elle est naturelle parce qu'a tres courte maturite, on ne fait presque plus de moyenne temporelle.  
Le probleme devient essentiellement **spatial**.

En outre, si la local vol s'annule sur une region separant \(S_0\) de \(K\), alors le spot ne peut pratiquement pas traverser cette region en temps tres court. L'implicite doit donc lui aussi tendre vers zero.

La moyenne harmonique respecte exactement cette propriete, contrairement a une moyenne arithmetique naive.

### A dire absolument

- "A courte maturite, c'est la geometrie spatiale qui domine."

### Piege a eviter

Ne pas donner une justification purement formelle ; l'argument de franchissement est tres parlant a l'oral.

---

## 22) "Quel lien faire avec Dupire dans une question de cours ?"

### Reponse detaillee

Le lien le plus propre est le suivant :

- **Dupire** : de la surface implicite vers la local vol ;
- **Bergomi 2.4** : de la local vol vers l'implicite et surtout vers la dynamique du smile.

Donc ces deux approches sont complementaires.

Le message a faire passer est qu'un modele local vol peut etre parfaitement calibre aujourd'hui grace a Dupire, tout en produisant demain une dynamique de smile qui n'est pas realiste.

### A dire absolument

- "Dupire traite le calibrage statique ; Bergomi eclaire la dynamique induite."

### Piege a eviter

Ne pas laisser croire que Dupire suffit a valider un modele du point de vue dynamique.

---

## 23) "Pourquoi le local vol calibre bien les vanilles mais pas forcement les exotiques ?"

### Reponse detaillee

Parce que calibrer les vanilles aujourd'hui contraint seulement la **loi marginale** de \(S_T\) pour chaque maturite, ou equivalentement la surface de prix des options europeennes.

Mais les exotiques dependent generalement de la **dynamique entiere du chemin** :

- barrieres ;
- cliquets ;
- autocalls ;
- produits sensibles aux deplacements futurs du smile.

Or le modele local vol impose une dynamique tres particuliere du smile.  
Donc un calibrage parfait des vanilles a la date 0 n'implique pas une bonne modelisation des exotiques.

### A dire absolument

- "Calibration statique parfaite ne veut pas dire dynamique correcte."

### Piege a eviter

Ne pas dire que local vol est "mauvais".  
Il est excellent pour certaines taches, mais limite pour d'autres.

---

## 24) "Pourquoi les modeles de volatilite stochastique sont-ils souvent preferes pour la dynamique du smile ?"

### Reponse detaillee

Parce qu'ils introduisent des facteurs de variance futurs aleatoires.

Dans un modele local vol, toute la dynamique future de la surface implicite est determinee par la seule variable d'etat \(S_t\).  
Dans un modele de volatilite stochastique, on a en plus un ou plusieurs facteurs de volatilite/variance, ce qui donne davantage de flexibilite.

Du point de vue smile dynamics :

- local vol est souvent trop rigide ;
- stochastic vol reproduit souvent mieux des regimes proches de sticky-delta ou intermediaires.

### A dire absolument

- "Le facteur supplementaire de variance donne la souplesse qui manque a local vol."

### Piege a eviter

Ne pas opposer caricaturalement les deux modeles : local vol reste tres utile comme modele de calibration et d'analyse.

---

## 25) "Quel est selon vous le message central a retenir pour conclure ?"

### Reponse detaillee

Le message central est le suivant :

> Un modele de volatilite locale ne se contente pas de reproduire une surface implicite statique ; il impose une dynamique tres precise du smile, largement gouvernee par la structure par terme du skew local \(\alpha(t)\).

En particulier :

- le skew implicite est une moyenne ponderee de \(\alpha(t)\) ;
- la dynamique ATMF est donnee par la moyenne uniforme de \(\alpha(t)\) ;
- le ratio \(R_T\) met en evidence la rigidite typique du smile sous local vol ;
- a courte maturite, la relation entre local vol et implicite devient une moyenne harmonique exacte.

Donc cette section explique tres bien pourquoi un bon calibrage statique n'est pas une garantie de bonne dynamique.

### A dire absolument

- "Le coeur du chapitre, c'est la dynamique du smile."

### Piege a eviter

Ne pas terminer sur une simple liste de formules.  
Il faut finir par une idee directrice.

---

# Reponses tres courtes si on te coupe vite

## "Le message en une phrase ?"

Sous local vol, le smile implicite futur est entierement contraint par la geometrie de \(\sigma_{\mathrm{loc}}(t,S)\), surtout via \(\alpha(t)\).

## "La formule cle ?"

\[
\hat{\sigma}_{K,T}^{\,2}
=
\frac{
\mathbb E\!\left[\int_0^T w_t\,\sigma_{\mathrm{loc}}^2(t,S_t)\,dt\right]
}{
\mathbb E\!\left[\int_0^T w_t\,dt\right]
},
\qquad
w_t=e^{-rt}S_t^2\Gamma_t.
\]

## "Le resultat cle sur le skew ?"

\[
\left.\frac{\partial\hat{\sigma}_{K,T}}{\partial\ln K}\right|_{K=F_T}
=
\frac1T\int_0^T \frac{t}{T}\alpha(t)\,dt.
\]

## "Le resultat cle sur l'ATMF ?"

\[
\frac{d\hat{\sigma}_{F_T,T}}{d\ln S_0}
=
\frac1T\int_0^T\alpha(t)\,dt.
\]

## "Le resultat cle de courte maturite ?"

\[
\frac{1}{\hat{\sigma}(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm{loc}}(0,S)}.
\]
