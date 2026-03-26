# Questions-réponses sur Bergomi 2.4 -- strictement centrées sur le contenu des slides

Ce document suit **strictement** le périmètre actuel de la présentation :

1. question inverse de Dupire ;
2. identité exacte comme moyenne pondérée de variance locale ;
3. approximation weak local vol ;
4. développement local près du forward ;
5. skew et courbure implicites ;
6. loi de puissance pour le skew ;
7. résultat exact de courte maturité ;
8. lien avec Dupire, lien avec stoch vol, limites.

---

## 1) Quel est l’objectif du passage de la volatilité locale à la volatilité implicite ?

**Réponse.**  
L’objectif est de comprendre, à partir d’une fonction de volatilité locale
\[
\sigma_{\mathrm{loc}}(t,S),
\]
quelle surface de volatilité implicite
\[
\hat\sigma(K,T)
\]
elle engendre.  
Dupire fait le chemin implicite \(\to\) local vol. Ici, Bergomi étudie le chemin inverse.

---

## 2) Quelle est l’identité exacte fondamentale du chapitre ?

**Réponse.**  
L’identité exacte est
\[
\hat\sigma_{K,T}^{\,2}
=
\frac{
\mathbb E^{\mathrm{loc}}
\!\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}(\hat\sigma_{K,T})
\sigma_{\mathrm{loc}}^2(t,S_t)\,dt
\right]
}{
\mathbb E^{\mathrm{loc}}
\!\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}(\hat\sigma_{K,T})\,dt
\right]
}.
\]
Elle exprime la **variance implicite** comme une **moyenne pondérée de la variance locale**.

---

## 3) Pourquoi faut-il insister sur le fait que la formule porte sur \(\hat\sigma^2\) et non sur \(\hat\sigma\) ?

**Réponse.**  
Parce que, exactement, on moyenne des **variances locales** et non des volatilités locales.  
L’idée “implied vol = weighted average of local vol” n’est rigoureusement vraie qu’au niveau approximatif, après développement au premier ordre.  
Le résultat exact porte bien sur
\[
\hat\sigma^2.
\]

---

## 4) Pourquoi dit-on que c’est une moyenne pondérée ?

**Réponse.**  
Parce qu’on peut réécrire la formule sous la forme
\[
\hat\sigma^2=\frac{\mathbb E[\int_0^T w_t\,\sigma_{\mathrm{loc}}^2(t,S_t)\,dt]}
{\mathbb E[\int_0^T w_t\,dt]},
\qquad
w_t=e^{-rt}S_t^2\Gamma_t^{BS}.
\]
Les poids sont donc les dollar gammas actualisés.

---

## 5) Pourquoi les poids sont-ils des dollar gammas ?

**Réponse.**  
Parce que l’identité vient d’un raisonnement de couverture delta. Après neutralisation du terme en \(dS_t\), le terme qui reste dans le P&L résiduel est un terme du second ordre, donc proportionnel à
\[
S_t^2\Gamma_t.
\]
Le gamma mesure donc la sensibilité du P&L à une erreur sur la variance instantanée.

---

## 6) Pourquoi la formule exacte n’est-elle pas directement exploitable ?

**Réponse.**  
Parce qu’elle est **implicite** : la quantité \(\hat\sigma_{K,T}\) apparaît aussi dans les poids, via \(\Gamma^{BS}(\hat\sigma_{K,T})\).  
On n’a donc pas une formule fermée simple pour \(\hat\sigma\).

---

## 7) Que signifie l’hypothèse “weak local vol” ?

**Réponse.**  
Cela signifie que la dépendance en spot de la volatilité locale est faible. On écrit par exemple
\[
\sigma_{\mathrm{loc}}(t,S)=\sigma_0+\delta\sigma(t,S),
\]
avec \(\delta\sigma\) petit.  
On effectue alors un développement perturbatif à l’ordre 1 autour du modèle de référence \(\sigma_0\).

---

## 8) Quelle formule approchée obtient-on dans le cadre weak local vol ?

**Réponse.**  
On obtient
\[
\hat{\sigma}_{K,T}
\approx
\frac{1}{T}
\int_0^T\!\!dt
\int_{\mathbb{R}}\phi(y)\,
\sigma_{\mathrm{loc}}\!\left(
t,\,
F_t\exp\!\Big(
\frac{t}{T}x_K+\sigma_0\sqrt{\frac{(T-t)t}{T}}\,y
\Big)
\right)dy,
\]
avec
\[
x_K=\ln\!\left(\frac{K}{F_T}\right),
\qquad
\phi(y)=\frac{e^{-y^2/2}}{\sqrt{2\pi}}.
\]

---

## 9) Quelle est l’interprétation de cette formule approchée ?

**Réponse.**  
Elle dit que l’implicite est approximativement une moyenne gaussienne de la local vol le long de trajectoires intermédiaires joignant \(S_0\) au strike.  
Le terme en \(y\) décrit la dispersion autour du chemin central.

---

## 10) Que représente le chemin central \(y=0\) ?

**Réponse.**  
Le cas \(y=0\) correspond au chemin “le plus probable” dans cette approximation.  
Si on ne garde que cette contribution, on obtient une image simplifiée où l’implicite est moyenne de la local vol le long d’un chemin déterministe en log-espace.

---

## 11) Pourquoi développe-t-on la local vol près du forward ?

**Réponse.**  
Parce que le chapitre cherche à décrire le smile **près du forward**, donc près de la zone ATMF.  
On écrit
\[
\sigma_{\mathrm{loc}}(t,S)=\bar{\sigma}(t)+\alpha(t)x+\frac{\beta(t)}{2}x^2,
\qquad
x=\ln\!\left(\frac{S}{F_t}\right).
\]
Ce développement permet d’identifier clairement les contributions du niveau, du skew et de la courbure.

---

## 12) Quelle est la signification de \(\alpha(t)\) et \(\beta(t)\) ?

**Réponse.**  
\(\alpha(t)\) est le **skew local instantané**.  
\(\beta(t)\) est la **courbure locale instantanée**.  
Ce sont les coefficients du développement local de \(\sigma_{\mathrm{loc}}\) autour du forward.

---

## 13) Quelle est la formule approchée du smile implicite près du forward ?

**Réponse.**  
À l’ordre 1,
\[
\hat{\sigma}_{K,T}
\approx
\frac{1}{T}\int_0^T \bar{\sigma}(t)\,dt
\;+\;
\left(
\frac{1}{T}\int_0^T \frac{t}{T}\alpha(t)\,dt
\right)x_K
\;+\;
\frac{1}{2}
\left(
\frac{1}{T}\int_0^T \Big(\frac{t}{T}\Big)^2\beta(t)\,dt
\right)x_K^2.
\]
Le coefficient de \(x_K\) donne le skew implicite, et celui de \(x_K^2\) la courbure implicite.

---

## 14) Quelle est la formule du skew implicite près du forward ?

**Réponse.**  
On obtient
\[
\left.\frac{\partial \hat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
=
\frac{1}{T}\int_0^T \frac{t}{T}\alpha(t)\,dt.
\]
Le skew implicite est donc une moyenne pondérée du skew local \(\alpha(t)\).

---

## 15) Pourquoi le poids est-il \(t/T\) ?

**Réponse.**  
Parce que l’effet du strike terminal sur la trajectoire n’est pas uniforme dans le temps.  
Au début, la contrainte imposée par le strike final est faible ; près de la maturité, elle devient dominante.  
Le facteur \(t/T\) traduit cette importance croissante.

---

## 16) Quelle est la formule de la courbure implicite ?

**Réponse.**  
La courbure implicite au voisinage du forward est
\[
\left.\frac{\partial^2 \hat{\sigma}_{K,T}}{\partial (\ln K)^2}\right|_{K=F_T}
=
\frac{1}{T}\int_0^T \Big(\frac{t}{T}\Big)^2\beta(t)\,dt.
\]

---

## 17) Si \(\alpha(t)=\alpha\) est constant, que vaut le skew implicite ?

**Réponse.**  
Alors
\[
\left.\frac{\partial \hat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
=
\frac{1}{T}\int_0^T \frac{t}{T}\alpha\,dt
=
\frac{\alpha}{2}.
\]
Donc le skew implicite vaut la moitié du skew local constant.

---

## 18) Si \(\beta(t)=\beta\) est constant, que vaut la courbure implicite ?

**Réponse.**  
Alors
\[
\left.\frac{\partial^2 \hat{\sigma}_{K,T}}{\partial (\ln K)^2}\right|_{K=F_T}
=
\frac{1}{T}\int_0^T \Big(\frac{t}{T}\Big)^2\beta\,dt
=
\frac{\beta}{3}.
\]

---

## 19) Pourquoi la loi de puissance sur \(\alpha(t)\) est-elle importante ?

**Réponse.**  
Parce qu’elle permet d’expliquer la structure par terme du skew implicite.  
Si
\[
\alpha(t)=\alpha_0\left(\frac{\tau_0}{t}\right)^\gamma
\quad (t>\tau_0),
\]
alors on obtient
\[
\left.\frac{\partial \hat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
\sim
\frac{1}{2-\gamma}\,
\alpha_0\left(\frac{\tau_0}{T}\right)^\gamma.
\]
Le skew implicite hérite donc du même exposant \(\gamma\).

---

## 20) Pourquoi introduit-on un cutoff \(\tau_0\) ?

**Réponse.**  
Parce que la loi \(t^{-\gamma}\) peut diverger au voisinage de \(0\).  
Le cutoff \(\tau_0\) permet de régulariser la formule et de garder une structure réaliste.

---

## 21) Quel est le résultat exact de courte maturité ?

**Réponse.**  
Quand \(T\to 0\),
\[
\frac{1}{\hat{\sigma}(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm{loc}}(0,S)}.
\]
L’inverse de l’implicite est donc la moyenne harmonique de l’inverse de la local vol entre \(S_0\) et \(K\), en variable logarithmique.

---

## 22) Pourquoi ce résultat est-il surprenant ?

**Réponse.**  
Parce qu’on pourrait attendre une moyenne de \(\sigma\) ou de \(\sigma^2\).  
En réalité, à très courte maturité, il n’y a presque plus de moyenne temporelle. La structure correcte devient spatiale, et elle fait apparaître une moyenne harmonique.

---

## 23) Quelle est l’intuition financière de cette moyenne harmonique ?

**Réponse.**  
Si la local vol devient très faible sur une zone à traverser entre \(S_0\) et \(K\), alors le spot a du mal à atteindre cette zone à très courte maturité. L’implicite doit donc refléter cette “résistance” spatiale. La moyenne harmonique capte précisément cela.

---

## 24) Quel est le lien avec Dupire ?

**Réponse.**  
Dupire permet de reconstruire la local vol à partir de la surface implicite.  
Ici, Bergomi étudie le problème inverse : à partir de la local vol, quelle implicite obtient-on ?  
Les deux démarches sont donc duales.

---

## 25) Pourquoi Bergomi insiste-t-il sur les limites de l’approximation weak local vol ?

**Réponse.**  
Parce que cette approximation est d’ordre 1.  
Elle est très utile pour comprendre :
- le skew,
- la courbure,
- les dépendances qualitatives.

Mais elle n’est pas assez précise pour obtenir de bons niveaux absolus sur des smiles equity réalistes.

---

## 26) Quel lien peut-on faire avec les modèles de volatilité stochastique ?

**Réponse.**  
La slide finale rappelle que les modèles de volatilité stochastique sont souvent plus adaptés pour la dynamique du smile.  
Le local vol donne un cadre très structuré et très instructif analytiquement, mais il impose une dynamique souvent trop contrainte.

---

## 27) Quel est le message central à retenir pour l’oral ?

**Réponse.**  
Le message central est :

> dans un modèle de volatilité locale, l’implicite n’est pas un objet libre ; elle est construite de façon très spécifique à partir de la géométrie spatio-temporelle de \(\sigma_{\mathrm{loc}}(t,S)\), et près du forward son comportement est gouverné principalement par \(\alpha(t)\).

---

## 28) Quelle phrase finale peut-on dire au jury ?

**Réponse.**  
Une bonne phrase de conclusion est :

> Bergomi montre que la local vol peut être comprise comme un générateur de smile implicite : exactement au niveau de la variance implicite, approximativement au niveau de la volatilité elle-même, et de façon particulièrement transparente près du forward et à courte maturité.
