# Questions probables a l'oral -- strictement dans le perimetre des slides

Ce document est volontairement **resserre** :

- il suit le **meme perimetre** que les slides actuels ;
- il ne developpe **pas** les parties retirees du support oral ;
- il sert a preparer les **questions les plus probables du jury** sur ce que tu presentes reellement.

Le contenu suit donc uniquement :

1. local vol \(\to\) implicite : idee du chapitre ;
2. identite exacte de moyenne ponderee ;
3. approximation weak local vol ;
4. parametrisation locale pres du forward ;
5. skew et courbure implicites ;
6. loi de puissance pour \(\alpha(t)\) ;
7. resultat exact a courte maturite ;
8. liens Dupire / stoch vol / limites.

---

## 1) "Quel est l'objectif exact de cette section ?"

### Reponse detaillee

L'objectif est de comprendre comment une fonction de volatilite locale
\[
\sigma_{\mathrm{loc}}(t,S)
\]
engendre une surface de volatilite implicite
\[
\hat{\sigma}(K,T).
\]

Dupire fait le chemin implicite \(\to\) local vol.  
Ici, Bergomi fait le chemin inverse :
\[
\sigma_{\mathrm{loc}}(t,S)\longrightarrow \hat{\sigma}(K,T).
\]

Le point central est que l'implicite n'est pas une simple "lecture ponctuelle" de la local vol ; elle resulte d'une forme d'averaging de la volatilite locale.

### A dire absolument

- "C'est le probleme inverse de Dupire."
- "On cherche a interpreter mathematiquement la volatilite implicite produite par un modele local vol."

### Piege a eviter

Ne pas partir directement sur le skew sans avoir d'abord dit ce que cherche la section dans son ensemble.

---

## 2) "Quelle est l'identite fondamentale ?"

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

Le poids est un poids de type dollar gamma :
\[
w_t=e^{-rt}S_t^2\Gamma_t^{BS}.
\]

### A dire absolument

- "La formule exacte porte sur \(\hat{\sigma}^2\)."
- "Les poids sont des dollar gammas actualises."

### Piege a eviter

Ne pas dire : "l'implicite est exactement la moyenne de la local vol".  
Exactement, c'est la **variance implicite** qui moyenne la **variance locale**.

---

## 3) "Pourquoi parle-t-on de moyenne ponderee ?"

### Reponse detaillee

Parce que la formule s'ecrit sous la forme
\[
\hat{\sigma}^2
=
\frac{\mathbb E\left[\int_0^T w_t\,\sigma_{\mathrm{loc}}^2(t,S_t)\,dt\right]}
{\mathbb E\left[\int_0^T w_t\,dt\right]}.
\]

On voit donc :

- un **numerateur** = somme ponderee des variances locales ;
- un **denominateur** = somme totale des poids ;
- donc une **moyenne**.

Les zones ou l'option a un gamma fort comptent davantage.

### A dire absolument

- "Toutes les dates et tous les etats ne comptent pas pareil."
- "Le gamma determine l'importance relative de chaque contribution."

### Piege a eviter

Ne pas parler d'une moyenne uniforme dans le temps.

---

## 4) "Pourquoi le gamma apparait-il dans les poids ?"

### Reponse detaillee

Il apparait parce que la derivation de Bergomi repose sur un P&L de couverture delta.

Quand on couvre delta, on neutralise le terme lineaire en \(dS_t\).  
Ce qui reste vient du terme quadratique de la formule d'Itô :
\[
\frac12 \partial_{SS}P\,d\langle S\rangle_t.
\]

Comme
\[
d\langle S\rangle_t=\sigma_t^2 S_t^2dt,
\]
on obtient naturellement le facteur
\[
S_t^2\Gamma_t.
\]

### A dire absolument

- "Le delta neutralise le premier ordre."
- "L'erreur de volatilite apparait au second ordre, donc via le gamma."

### Piege a eviter

Ne pas confondre gamma et vega.

---

## 5) "Pourquoi l'identite exacte ne donne-t-elle pas directement une formule simple de \(\hat{\sigma}\) ?"

### Reponse detaillee

Parce que la formule est **implicite**.

La quantite \(\hat{\sigma}_{K,T}\) apparait deja dans le membre de droite, a travers :

- le gamma Black-Scholes ;
- la loi utilisee dans l'esperance.

Donc la formule est mathematiquement tres importante, mais pas encore directement exploitable pour obtenir une expression fermee.

### A dire absolument

- "La formule exacte est conceptuelle, mais implicite."

### Piege a eviter

Ne pas dire qu'elle est inutile ; elle sert de point de depart a l'approximation weak local vol.

---

## 6) "Que veut dire weak local vol ?"

### Reponse detaillee

On suppose que la dependance en spot de la local vol est faible, par exemple
\[
\sigma_{\mathrm{loc}}(t,S)=\sigma_0+\delta\sigma(t,S),
\]
avec \(\delta\sigma\) petit.

On effectue alors un developpement perturbatif d'ordre 1 autour d'un modele de reference simple.

### A dire absolument

- "C'est un developpement perturbatif."
- "On linearise autour d'une volatilite de reference."

### Piege a eviter

Ne pas dire que cela signifie seulement "volatilite presque constante en temps".  
Le point essentiel est la faible dependance en **spot**.

---

## 7) "Quelle est la formule approchee principale obtenue ?"

### Reponse detaillee

On obtient :
\[
\hat{\sigma}_{K,T}
\approx
\frac{1}{T}
\int_0^T\!\!dt
\int_{\mathbb R}\phi(y)\,
\sigma_{\mathrm{loc}}\!\left(
t,\,
F_t\exp\!\Big(
\frac{t}{T}x_K+\sigma_0\sqrt{\frac{(T-t)t}{T}}\,y
\Big)
\right)dy.
\]

avec
\[
x_K=\ln\!\left(\frac{K}{F_T}\right),
\qquad
\phi(y)=\frac{e^{-y^2/2}}{\sqrt{2\pi}}.
\]

Cette formule donne une interpretation geometrique : l'implicite est une moyenne gaussienne de la local vol sur des trajectoires intermediaires.

### A dire absolument

- "La formule approchee porte cette fois sur \(\hat{\sigma}\), pas sur \(\hat{\sigma}^2\)."
- "Il s'agit d'une formule d'ordre 1."

### Piege a eviter

Ne pas oublier de dire que cette formule est **approchee**.

---

## 8) "Quel est le sens de la variable \(y\) ?"

### Reponse detaillee

\(y\) est une variable gaussienne standard qui parametre les deviations autour d'un chemin central.

Le terme
\[
F_t\exp\!\Big(
\frac{t}{T}x_K+\sigma_0\sqrt{\frac{(T-t)t}{T}}\,y
\Big)
\]
represente les niveaux intermediaires de spot explores dans l'approximation.

### A dire absolument

- "Le terme en \(y\) represente la dispersion autour d'un chemin central."

### Piege a eviter

Ne pas dire que \(y\) est un "nouveau spot" ; c'est un parametre gaussien d'integration.

---

## 9) "Pourquoi le chemin \(y=0\) est-il important ?"

### Reponse detaillee

Parce qu'il correspond au chemin central, souvent interprete comme le chemin le plus probable dans cette approximation.

Si on ne garde que \(y=0\), on voit l'implicite comme une moyenne de la local vol le long d'une trajectoire simple reliant \(S_0\) au strike final.

### A dire absolument

- "Le chemin \(y=0\) donne une bonne intuition geometrique."

### Piege a eviter

Ne pas dire que cette reduction est exacte : ce n'est qu'une lecture intuitive.

---

## 10) "Pourquoi developper la local vol pres du forward ?"

### Reponse detaillee

Parce qu'on s'interesse au smile pres du point le plus naturel pour les vanilles, le forward ATMF.

On ecrit :
\[
\sigma_{\mathrm{loc}}(t,S)=\bar{\sigma}(t)+\alpha(t)x+\frac{\beta(t)}{2}x^2,
\qquad
x=\ln\!\left(\frac{S}{F_t}\right).
\]

Cela permet de separer :

- le niveau local \(\bar{\sigma}(t)\) ;
- le skew local \(\alpha(t)\) ;
- la courbure locale \(\beta(t)\).

### A dire absolument

- "C'est un developpement local en moneyness autour du forward."

### Piege a eviter

Ne pas oublier que ce developpement est local, donc surtout valable pres du forward.

---

## 11) "Que representent \(\alpha(t)\) et \(\beta(t)\) ?"

### Reponse detaillee

- \(\alpha(t)\) est le **skew local instantane** ;
- \(\beta(t)\) est la **courbure locale instantanee**.

Ils decrivent la forme locale de la fonction \(\sigma_{\mathrm{loc}}(t,S)\) en fonction de la moneyness.

### A dire absolument

- "\(\alpha(t)\) pilote la pente."
- "\(\beta(t)\) pilote la convexite."

### Piege a eviter

Ne pas leur donner directement une interpretation dynamique plus large que celle visible dans les slides.

---

## 12) "Quelle est la formule du skew implicite pres du forward ?"

### Reponse detaillee

On obtient :
\[
\left.\frac{\partial \hat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
=
\frac{1}{T}\int_0^T \frac{t}{T}\alpha(t)\,dt.
\]

Donc le skew implicite pres du forward est une moyenne ponderee du skew local \(\alpha(t)\).

### A dire absolument

- "Le poids est \(t/T\)."
- "Le skew implicite ne recopie pas instantanement \(\alpha(t)\), il en fait une moyenne."

### Piege a eviter

Ne pas confondre cette formule avec une dynamique en spot : ici on parle bien d'une derivee en strike.

---

## 13) "Quelle est la formule de la courbure implicite ?"

### Reponse detaillee

On a :
\[
\left.\frac{\partial^2 \hat{\sigma}_{K,T}}{\partial (\ln K)^2}\right|_{K=F_T}
=
\frac{1}{T}\int_0^T \Big(\frac{t}{T}\Big)^2\beta(t)\,dt.
\]

Donc la courbure implicite est une moyenne ponderee de la courbure locale.

### A dire absolument

- "La courbure implicite depend de \(\beta(t)\)."

### Piege a eviter

Ne pas attribuer a \(\beta(t)\) le role principal dans les conclusions globales ; dans les slides, le message principal porte davantage sur le skew.

---

## 14) "Si \(\alpha(t)\) est constant, pourquoi trouve-t-on \(\alpha/2\) ?"

### Reponse detaillee

Si \(\alpha(t)\equiv \alpha\), alors
\[
\left.\frac{\partial \hat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
=
\frac{1}{T}\int_0^T \frac{t}{T}\alpha\,dt
=
\alpha\int_0^1 u\,du
=
\frac{\alpha}{2}.
\]

### A dire absolument

- "Le facteur \(1/2\) vient simplement de l'integration du poids \(t/T\)."

### Piege a eviter

Ne pas dire que l'implicite "divise toujours le skew local par deux" ; c'est vrai ici quand \(\alpha(t)\) est constant.

---

## 15) "Que montre la loi de puissance pour \(\alpha(t)\) ?"

### Reponse detaillee

Si
\[
\alpha(t)=\alpha_0\left(\frac{\tau_0}{t}\right)^\gamma
\quad (t>\tau_0),
\]
alors a grande maturite
\[
\left.\frac{\partial \hat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
\sim
\frac{1}{2-\gamma}\alpha_0\left(\frac{\tau_0}{T}\right)^\gamma.
\]

Le point important est que le skew implicite herite du **meme exposant** \(\gamma\) que le skew local.

### A dire absolument

- "Le modele transmet la structure par terme du skew local vers le skew implicite."

### Piege a eviter

Ne pas oublier le role du cutoff \(\tau_0\), qui evite la divergence a courte maturite.

---

## 16) "Quel est le resultat exact de courte maturite ?"

### Reponse detaillee

Quand \(T\to 0\),
\[
\frac{1}{\hat{\sigma}(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm{loc}}(0,S)}.
\]

Donc l'inverse de l'implicite est une moyenne harmonique de l'inverse de la local vol.

### A dire absolument

- "Le resultat est exact."
- "La moyenne est harmonique, pas arithmetique."

### Piege a eviter

Ne pas melanger ce resultat exact avec l'approximation weak local vol, qui est d'une autre nature.

---

## 17) "Pourquoi la moyenne est-elle harmonique a courte maturite ?"

### Reponse detaillee

Parce qu'a tres courte maturite il n'y a presque plus d'averaging temporel.

La relation entre local vol et implicite devient alors essentiellement spatiale : il faut "traverser" l'intervalle entre \(S_0\) et \(K\), et c'est l'inverse de la volatilite qui s'additionne naturellement dans cette limite.

### A dire absolument

- "A courte maturite, la structure temporelle s'efface au profit d'une structure spatiale."

### Piege a eviter

Ne pas dire simplement "c'est une autre moyenne". Il faut expliquer pourquoi le regime \(T\to 0\) change la nature du probleme.

---

## 18) "Quel est le lien avec Dupire dans cette presentation ?"

### Reponse detaillee

Dupire fait le chemin
\[
\hat{\sigma}(K,T)\longrightarrow \sigma_{\mathrm{loc}}(t,S).
\]

Ici, Bergomi regarde le chemin inverse
\[
\sigma_{\mathrm{loc}}(t,S)\longrightarrow \hat{\sigma}(K,T).
\]

Donc les deux points de vue sont complementaires.

### A dire absolument

- "Dupire construit la local vol depuis la surface."
- "Bergomi etudie la surface implicite produite par une local vol donnee."

### Piege a eviter

Ne pas dire que Bergomi "inverse explicitement" Dupire partout ; il obtient d'abord une identite exacte implicite, puis une approximation exploitable.

---

## 19) "Pourquoi Bergomi parle-t-il des limites de l'approximation ?"

### Reponse detaillee

Parce que l'approximation weak local vol est une approximation d'ordre 1.

Elle est tres utile pour comprendre :

- le skew ;
- la courbure ;
- l'intuition geometrique.

Mais elle n'est pas assez precise pour reproduire parfaitement des niveaux de volatilite implicite sur des smiles equity realistes.

### A dire absolument

- "Tres utile pour comprendre, pas suffisante pour du pricing de precision."

### Piege a eviter

Ne pas dire que l'approximation est mauvaise en tout ; elle est excellente conceptuellement.

---

## 20) "Quel lien fais-tu avec les modeles de volatilite stochastique ?"

### Reponse detaillee

Dans cette presentation, le lien reste simple :

- le modele local vol donne une interpretation analytique claire des smiles ;
- mais il peut etre trop contraint pour reproduire toute la richesse observee sur les marches ;
- d'ou l'interet des modeles de volatilite stochastique pour mieux decrire certaines dynamiques de surface.

### A dire absolument

- "Local vol est tres bon pour relier statiquement la surface et la fonction locale."
- "Mais il ne suffit pas toujours pour toute la dynamique observee."

### Piege a eviter

Ne pas partir dans une comparaison trop large si le jury ne te la demande pas ; dans tes slides, ce point reste une conclusion.

---

## 21) "Quel est le message final de la presentation ?"

### Reponse detaillee

Le message final est :

1. il existe une identite exacte liant variance implicite et variance locale ;
2. une approximation d'ordre 1 permet de lire la local vol comme une moyenne geometrique/gaussienne ;
3. pres du forward, le skew implicite est une moyenne ponderee de \(\alpha(t)\) ;
4. a courte maturite, on retrouve un resultat exact harmonique ;
5. tout cela explique comment une local vol impose une forme de smile implicite.

### A dire absolument

- "Le mot-cle est : averaging."
- "Le deuxieme mot-cle est : structure pres du forward."

### Piege a eviter

Ne pas finir sur un detail technique ; il faut finir sur l'idee generale.

