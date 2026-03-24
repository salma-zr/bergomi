# Bergomi 2.4 -- De la volatilite locale a la volatilite implicite

## Objet du document

Ce document couvre, de facon rigoureuse et pedagogique, le contenu de la section 2.4 du chapitre 2 de Bergomi, c'est-a-dire :

- 2.4.1 Implied volatilities as weighted averages of instantaneous volatilities
- 2.4.2 Approximate expression for weakly local volatilities
- 2.4.3 Expanding around a constant volatility
- 2.4.4 Discussion
- 2.4.5 The smile near the forward
- 2.4.6 An exact result for short maturities

J'ajoute egalement, parce que c'est central pour un oral sur les dynamiques de smile :

- la dynamique de la volatilite ATMF quand le spot bouge ;
- la definition et l'interpretation de
  \[
  R_T=\frac{d\hat\sigma_{F_T,T}/d\ln S_0}{\mathcal S_T},
  \]
  qui est le ratio de rigidite du skew ;
- les liens avec Dupire, la projection de Gyongy et les modeles de volatilite stochastique ;
- les limites mathematiques et financieres de l'approximation weak local vol.

Le niveau vise est M2 probabilites-finance / preparation d'examen theorique.

---

## 0. Cadre, notations et point de depart

On se place sous la mesure risque-neutre. Sous un modele de volatilite locale,
\[
dS_t=(r-q)S_t\,dt+\sigma_{\mathrm{loc}}(t,S_t)S_t\,dW_t.
\]

On note :

- \(C(K,T)\) le prix d'un call europeen de strike \(K\), maturite \(T\) ;
- \(F_T=S_0 e^{(r-q)T}\) le forward a maturite \(T\) ;
- \(y=\ln(K/F_T)\) le log-moneyness ;
- \(\hat\sigma(K,T)\) la volatilite implicite Black-Scholes associee au call ;
- \(w(T,y)=T\hat\sigma(T,y)^2\) la variance implicite totale.

Le chapitre 2.3 allait de la surface implicite vers la volatilite locale, via Dupire.
Le chapitre 2.4 pose le probleme inverse :

> si la fonction de volatilite locale \(\sigma_{\mathrm loc}(t,S)\) est donnee, que peut-on dire de \(\hat\sigma(K,T)\) ?

Le message profond de Bergomi est :

> **la variance implicite n'est pas la valeur ponctuelle de la variance locale ; c'est une moyenne ponderee de la variance locale sur le temps et sur les trajectoires pertinentes pour l'option.**

---

## 1. Lien avec Dupire

### 1.1 Formule exacte de Dupire en variance totale

En variables \((T,y)\), la formule de Dupire s'ecrit
\[
\sigma_{\mathrm{loc}}^2(T,y)
=
\frac{\partial_T w(T,y)}
{
\left(1-\frac{y}{2w} \partial_y w\right)^2
-\frac14\left(\frac14+\frac1w\right)(\partial_y w)^2
+\frac12 \partial_{yy}w
}.
\]

Comme \(w(T,y)=T\hat\sigma(T,y)^2\), on a
\[
\partial_T w = \hat\sigma^2 + 2T\hat\sigma\,\partial_T \hat\sigma,
\qquad
\partial_y w = 2T\hat\sigma\,\partial_y \hat\sigma,
\qquad
\partial_{yy}w = 2T\Big((\partial_y\hat\sigma)^2+\hat\sigma\,\partial_{yy}\hat\sigma\Big).
\]

Donc la formule precedente est equivalente a l'ecriture de Bergomi :
\[
\sigma_{\mathrm{loc}}^2(T,y)
=
\frac{\hat\sigma^2+2T\hat\sigma\,\hat\sigma_T}
{
\left(1-y\frac{\hat\sigma_y}{\hat\sigma}\right)^2
+T\left(\hat\sigma_y^2+\hat\sigma \hat\sigma_{yy}\right)
-\left(\frac14+\frac{1}{T\hat\sigma^2}\right)T^2\hat\sigma^2\hat\sigma_y^2
}.
\]

Ici \(\hat\sigma_T=\partial_T\hat\sigma\), \(\hat\sigma_y=\partial_y\hat\sigma\), etc.

### 1.2 Pourquoi 2.4 est necessaire

Dupire donne \(\sigma_{\mathrm loc}\) en fonction de \(\hat\sigma\), donc **dans le sens implicite \(\to\) local**.

Mais si l'on veut comprendre :

- la dynamique du smile dans un modele local vol,
- la sensibilite de l'implicite au spot,
- l'interpretation des skews et des courbures,

il faut des formules approximatives ou asymptotiques allant dans le sens **local \(\to\) implicite**.

C'est exactement l'objet de 2.4.

---

## 2. Section 2.4.1 -- L'implicite comme moyenne ponderee des volatilites instantanees

### 2.1 Identite generale entre deux modeles

On considere :

- un modele I, avec prix de vanille \(P_1(t,S)\), verifiant une PDE de pricing avec volatilite \(\sigma_1(t,S)\) ;
- un modele II, sous lequel le spot suit
  \[
  dS_t=(r-q)S_t\,dt+\sigma_{2,t}S_t\,dW_t,
  \]
  ou \(\sigma_{2,t}\) est pour l'instant un processus arbitraire.

On definit
\[
Q_t=e^{-rt}P_1(t,S_t).
\]

Par la formule d'Itô, sous le modele II,
\[
dQ_t
=
e^{-rt}
\left[
\left(-rP_1+\partial_tP_1\right)dt
+\partial_S P_1\,dS_t
+\frac12 \partial_{SS}P_1\,d\langle S\rangle_t
\right].
\]

Or
\[
d\langle S\rangle_t=\sigma_{2,t}^2 S_t^2\,dt.
\]

Donc
\[
dQ_t
=
e^{-rt}
\left[
\left(-rP_1+\partial_tP_1\right)dt
+\partial_S P_1\,dS_t
+\frac12 \sigma_{2,t}^2 S_t^2 \partial_{SS}P_1\,dt
\right].
\]

En prenant l'esperance conditionnelle sous le modele II, le terme en \(dW_t\) disparait et on obtient
\[
\mathbb E_2[dQ_t\mid \mathcal F_t]
=
e^{-rt}
\left[
-rP_1+\partial_tP_1+(r-q)S_t\partial_S P_1
+\frac12 \sigma_{2,t}^2 S_t^2 \partial_{SS}P_1
\right]dt.
\]

Comme \(P_1\) verifie la PDE du modele I,
\[
\partial_tP_1+(r-q)S\partial_S P_1+\frac12 \sigma_1(t,S)^2 S^2 \partial_{SS}P_1-rP_1=0,
\]
on remplace le crochet et il reste
\[
\mathbb E_2[dQ_t\mid \mathcal F_t]
=
e^{-rt}\frac12 S_t^2 \partial_{SS}P_1(t,S_t)
\Big(\sigma_{2,t}^2-\sigma_1(t,S_t)^2\Big)\,dt.
\]

En integrant de \(0\) a \(T\),
\[
\mathbb E_2[Q_T]
=
Q_0
+
\mathbb E_2\left[
\int_0^T
e^{-rt}\frac12 S_t^2 \partial_{SS}P_1(t,S_t)
\Big(\sigma_{2,t}^2-\sigma_1(t,S_t)^2\Big)\,dt
\right].
\]

Mais \(Q_T=e^{-rT}f(S_T)\), donc \(\mathbb E_2[Q_T]=P_2(0,S_0)\), d'ou
\[
P_2(0,S_0)
=
P_1(0,S_0)
+
\mathbb E_2\left[
\int_0^T
e^{-rt}\frac12 S_t^2 \partial_{SS}P_1(t,S_t)
\Big(\sigma_{2,t}^2-\sigma_1(t,S_t)^2\Big)\,dt
\right].
\tag{2.30}
\]

### 2.2 Interpretation financiere

Cette identite formalise une intuition de desk :

- on vend l'option ;
- on la delta-couvre avec le modele I ;
- si le monde reel suit le modele II, le PnL d'erreur de couverture provient du gamma/theta ;
- ce PnL cumule est proportionnel a
  \[
  \frac12 S_t^2 \Gamma_t \big(\sigma_{2,t}^2-\sigma_1^2\big).
  \]

Donc :

> **le prix dans le modele II = le prix dans le modele I + la correction d'erreur de couverture gamma/theta attendue sous le modele II.**

### 2.3 Cas particulier : choix du modele I egal au Black-Scholes implicite

On choisit comme modele I un Black-Scholes de volatilite constante egale a l'implicite de l'option consideree, notee \(\hat\sigma_{KT}\). Alors, par definition de l'implicite :
\[
P_1(0,S_0)=P_2(0,S_0).
\]

Dans (2.30), le terme de gauche et le premier terme de droite s'annulent. Il reste
\[
0=
\mathbb E_2\left[
\int_0^T
e^{-rt}\frac12 S_t^2 \Gamma_t^{BS}(\hat\sigma_{KT})
\Big(\sigma_{2,t}^2-\hat\sigma_{KT}^2\Big)\,dt
\right].
\]

En regroupant les termes en \(\hat\sigma_{KT}^2\),
\[
\hat\sigma_{KT}^2
=
\frac{
\mathbb E_2\left[
\int_0^T e^{-rt}S_t^2 \Gamma_t^{BS}(\hat\sigma_{KT})\,\sigma_{2,t}^2\,dt
\right]
}
{
\mathbb E_2\left[
\int_0^T e^{-rt}S_t^2 \Gamma_t^{BS}(\hat\sigma_{KT})\,dt
\right]
}.
\tag{2.31}
\]

### 2.4 Cas local vol : identite cle du chapitre

Si le modele II est lui-meme un modele local vol, c'est-a-dire
\[
\sigma_{2,t}=\sigma_{\mathrm{loc}}(t,S_t),
\]
alors
\[
\hat\sigma_{KT}^2
=
\frac{
\mathbb E_{\sigma_{\mathrm{loc}}}\left[
\int_0^T e^{-rt}S_t^2 \Gamma_t^{BS}(\hat\sigma_{KT})\,
\sigma_{\mathrm{loc}}(t,S_t)^2\,dt
\right]
}
{
\mathbb E_{\sigma_{\mathrm{loc}}}\left[
\int_0^T e^{-rt}S_t^2 \Gamma_t^{BS}(\hat\sigma_{KT})\,dt
\right]
}.
\tag{2.32}
\]

C'est **le resultat fondamental** :

> \[
> \boxed{\hat\sigma_{KT}^2=\text{moyenne ponderee de }\sigma_{\mathrm{loc}}(t,S)^2}
> \]
> ou les poids sont donnes par le dollar gamma de l'option, puis moyennes sur les trajectoires pertinentes.

### 2.5 Interpretation du poids

Le poids instantane est
\[
e^{-rt}S_t^2 \Gamma_t^{BS}(\hat\sigma_{KT}).
\]

Commentaires :

1. Pour une vanille convexe, ce poids est positif.
2. Il est fort quand l'option est la plus "gamma-sensitive".
3. Ce n'est pas une moyenne purement temporelle : on moyenne aussi sur les chemins de \(S_t\).
4. La variance implicite regarde principalement les zones de l'espace et du temps ou l'option a du gamma.

Autrement dit :

> l'implicite ne voit pas toute la surface locale avec le meme poids ; il voit surtout la region qui compte pour la replication gamma.

### 2.6 Variante symetrique

Si l'on choisit au contraire comme modele I le modele local vol et comme modele II le Black-Scholes implicite, on obtient
\[
\hat\sigma_{KT}^2
=
\frac{
\mathbb E_{\hat\sigma_{KT}}\left[
\int_0^T e^{-rt}S_t^2 \Gamma_t^{\mathrm{loc}}\,\sigma_{\mathrm{loc}}(t,S_t)^2\,dt
\right]
}
{
\mathbb E_{\hat\sigma_{KT}}\left[
\int_0^T e^{-rt}S_t^2 \Gamma_t^{\mathrm{loc}}\,dt
\right]
}.
\tag{2.33}
\]

Cette ecriture est mathematiquement equivalente mais moins pratique pour les calculs explicites.

---

## 3. Section 2.4.2 -- Approximation "weak local volatility"

Le probleme avec (2.32) est qu'il s'agit d'une formule implicite : \(\hat\sigma_{KT}\) apparait deja dans les poids et dans la loi de \(S_t\).

Pour obtenir une formule exploitable, Bergomi suppose que la volatilite locale est "faiblement locale".

### 3.1 Decomposition de la variance locale

On pose
\[
u(t,S)=\sigma_{\mathrm{loc}}(t,S)^2
=u_0(t)+\delta u(t,S),
\]
ou :

- \(u_0(t)=\sigma_0(t)^2\) est une variance de reference purement temporelle ;
- \(\delta u\) est une perturbation petite.

Si \(\delta u=0\), on est dans un Black-Scholes a volatilite deterministe \(\sigma_0(t)\), et l'implicite est simplement
\[
\hat\sigma_{0,T}^2=\frac1T\int_0^T \sigma_0(t)^2\,dt.
\]

### 3.2 Pourquoi la correction de densite disparait a l'ordre 1

La subtilite est la suivante : dans (2.32), \(u\) intervient :

1. explicitement, dans le numerateur ;
2. implicitement, parce que la loi de \(S_t\) depend aussi de \(u\).

Ecrivons abstraitement
\[
R(\varepsilon)=
\frac{\mathbb E_\varepsilon[(u_0+\varepsilon h)G]}
{\mathbb E_\varepsilon[G]},
\qquad \delta u = \varepsilon h.
\]

On developpe
\[
\mathbb E_\varepsilon[G]
=
\mathbb E_0[G]+\varepsilon \Delta_G + o(\varepsilon),
\]
\[
\mathbb E_\varepsilon[(u_0+\varepsilon h)G]
=
u_0\mathbb E_0[G]+\varepsilon \Big(u_0\Delta_G+\mathbb E_0[hG]\Big)+o(\varepsilon).
\]

Donc
\[
R(\varepsilon)
=
\frac{u_0\mathbb E_0[G]+\varepsilon \big(u_0\Delta_G+\mathbb E_0[hG]\big)}
{\mathbb E_0[G]+\varepsilon \Delta_G}
+o(\varepsilon).
\]

En faisant le quotient au premier ordre,
\[
R(\varepsilon)
=
u_0+\varepsilon \frac{\mathbb E_0[hG]}{\mathbb E_0[G]}+o(\varepsilon).
\]

Le terme de correction provenant de la densite, \(\Delta_G\), **s'annule** entre numerateur et denominateur.

C'est exactement le contenu du passage de Bergomi vers
\[
\delta(\hat\sigma_{KT}^2)
=
\frac{
\mathbb E_{\sigma_0}\left[
\int_0^T e^{-rt}\delta u(t,S_t) S_t^2 \Gamma_t^{BS}(\sigma_0)\,dt
\right]
}
{
\mathbb E_{\sigma_0}\left[
\int_0^T e^{-rt} S_t^2 \Gamma_t^{BS}(\sigma_0)\,dt
\right]
}.
\tag{2.35}
\]

### 3.3 Denominateur : martingale du dollar gamma actualise

Dans le Black-Scholes a volatilite deterministe \(\sigma_0(t)\), le processus
\[
M_t=e^{-rt}S_t^2 \Gamma_t^{BS}(\sigma_0)
\]
est une martingale. Par consequent,
\[
\mathbb E_{\sigma_0}[M_t]=M_0.
\]

En integrant sur \(t\in[0,T]\),
\[
\mathbb E_{\sigma_0}\left[\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}(\sigma_0)\,dt\right]
=
T\,S_0^2 \Gamma_0^{BS}(\sigma_0).
\tag{2.36}
\]

### 3.4 Numerateur : insertion de la densite lognormale

On note
\[
\omega_t=\int_0^t \sigma_0(\tau)^2\,d\tau.
\]

Sous le modele de reference, la densite de \(S_t\) s'ecrit en variable
\[
x=\ln(S/F_t),
\qquad F_t=S_0 e^{(r-q)t},
\]
sous la forme gaussienne
\[
\rho_{\sigma_0}(t,S)=
\frac{1}{\sqrt{2\pi\omega_t}\,S}
\exp\left(-\frac{(x+\omega_t/2)^2}{2\omega_t}\right).
\tag{2.38}
\]

De meme, le dollar gamma de l'option residuelle de maturite \(T-t\) est explicite :
\[
S^2 \Gamma_t^{BS}
=
S\frac{F_T}{F_t}e^{-r(T-t)}
\frac{1}{\sqrt{2\pi(\omega_T-\omega_t)}}
\exp\left(
-\frac{\big(-x_K+x+(\omega_T-\omega_t)/2\big)^2}{2(\omega_T-\omega_t)}
\right),
\tag{2.39}
\]
ou
\[
x_K=\ln(K/F_T).
\]

Le numerateur de (2.35) devient alors
\[
\int_0^T dt \int_{\mathbb R} dx\;
\delta u(t,F_t e^x)\times
\text{(produit de deux gaussiennes)}.
\]

Le produit de ces deux gaussiennes se combine en une gaussienne unique. Le changement de variable
\[
x
=
\frac{\omega_t}{\omega_T}x_K
+
\sqrt{\frac{(\omega_T-\omega_t)\omega_t}{\omega_T}}\;y
\]
centre et normalise cette gaussienne.

On obtient ainsi
\[
\delta(\hat\sigma_{KT}^2)
=
\frac1T\int_0^T dt\int_{\mathbb R}\phi(y)\,
\delta u\!\left(
t,
F_t \exp\left(
\frac{\omega_t}{\omega_T}x_K
+
\sqrt{\frac{(\omega_T-\omega_t)\omega_t}{\omega_T}}\,y
\right)\right)dy,
\]
avec
\[
\phi(y)=\frac{e^{-y^2/2}}{\sqrt{2\pi}}.
\]

Comme
\[
\hat\sigma_{KT}^2
=
\frac1T\int_0^T u_0(t)\,dt
+\delta(\hat\sigma_{KT}^2),
\]
on obtient finalement
\[
\hat\sigma_{KT}^2
=
\frac1T\int_0^T dt\int_{\mathbb R}\phi(y)\,
u\!\left(
t,
F_t \exp\left(
\frac{\omega_t}{\omega_T}x_K
+
\sqrt{\frac{(\omega_T-\omega_t)\omega_t}{\omega_T}}\,y
\right)\right)dy.
\tag{2.40}
\]

### 3.5 Interpretation de (2.40)

La formule (2.40) dit :

- on moyenne la **variance locale** ;
- le point de l'espace visite a la date \(t\) depend du strike final \(K\) ;
- le coeur de cette moyenne est une famille de trajectoires lognormales "pontes" entre le spot initial et le strike.

Si \(u\) ne depend que de \(t\), la formule devient exacte, car l'integrale en \(y\) vaut 1 :
\[
\hat\sigma_{KT}^2
=
\frac1T\int_0^T u(t)\,dt.
\]

Donc :

> **quand la volatilite est purement temporelle, la variance implicite est exactement la moyenne temporelle de la variance instantanee.**

---

## 4. Section 2.4.3 -- Developpement autour d'une volatilite constante

On specialise a
\[
\sigma_0(t)\equiv \sigma_0,
\qquad
\omega_t=\sigma_0^2 t.
\]

On ecrit
\[
\sigma_{\mathrm{loc}}(t,S)=\sigma_0+\delta\sigma(t,S),
\qquad
\delta u(t,S)=2\sigma_0\,\delta\sigma(t,S)+O(\delta\sigma^2).
\]

Comme
\[
\hat\sigma_{KT}^2=\sigma_0^2+2\sigma_0\,\delta\hat\sigma_{KT}+O(\delta\sigma^2),
\]
la formule precedente donne
\[
\delta\hat\sigma_{KT}
=
\frac1T\int_0^T dt\int_{\mathbb R}\phi(y)\,
\delta\sigma\!\left(
t,
F_t \exp\left(
\frac{t}{T}x_K
+
\sigma_0\sqrt{\frac{(T-t)t}{T}}\,y
\right)\right)dy.
\tag{2.41}
\]

En rajoutant \(\sigma_0\), on obtient la formule phare utilisee dans la suite :
\[
\hat\sigma_{KT}
\approx
\frac1T\int_0^T dt\int_{\mathbb R}\phi(y)\,
\sigma_{\mathrm{loc}}\!\left(
t,
F_t \exp\left(
\frac{t}{T}x_K
+
\sigma_0\sqrt{\frac{(T-t)t}{T}}\,y
\right)\right)dy.
\tag{2.42}
\]

Cette formule est exacte a l'ordre 1 en \(\delta\sigma\).

---

## 5. Section 2.4.4 -- Discussion et intuition geometrique

### 5.1 Pourquoi \(t=0\) et \(t=T\) sont speciaux

Dans (2.42), si \(t=0\), alors
\[
F_t \exp\left(\frac{t}{T}x_K+\sigma_0\sqrt{\frac{(T-t)t}{T}}\,y\right)=S_0.
\]

Donc, a l'instant initial, seule la valeur locale en \(S=S_0\) compte.

Si \(t=T\), alors
\[
F_T \exp\left(\frac{T}{T}x_K+0\cdot y\right)=K.
\]

Donc, juste avant l'echeance, seule la valeur locale en \(S=K\) compte.

Cela est parfaitement naturel :

- au debut, toutes les trajectoires partent de \(S_0\) ;
- a la fin, le gamma est concentre autour du strike \(K\).

### 5.2 La trajectoire la plus probable

Le poids gaussien \(\phi(y)\) est maximal en \(y=0\). La trajectoire dominante est donc
\[
\ln S_t
=
\ln F_t+\frac{t}{T}x_K,
\]
c'est-a-dire la droite qui relie \(\ln S_0\) a \(\ln K\) dans l'espace des logs.

Si l'on ne gardait que \(y=0\), on obtiendrait l'approximation grossiere
\[
\hat\sigma_{KT}
\approx
\frac1T\int_0^T
\sigma_{\mathrm{loc}}\!\left(
t,
F_t e^{\frac{t}{T}\ln(K/F_T)}
\right)dt.
\tag{2.43}
\]

Mais Bergomi insiste :

> cette approximation "most likely path" seule n'est pas suffisante pour les smiles equity realistes.

### 5.3 Limite importante de l'approximation weak local vol

Le point essentiel du texte est :

- l'approximation d'ordre 1 capte souvent bien les **differences de volatilites** (donc le skew) ;
- elle ne capte pas assez bien les **niveaux absolus** du smile ;
- en pratique, la dependance de la densite elle-meme a la local vol est cruciale.

Conclusion pratique :

> pour faire du trading ou de la calibration precise sur smiles equity realistes, il faut en general resoudre numeriquement l'equation forward de Dupire.

---

## 6. Section 2.4.5 -- Le smile pres du forward

On suppose la local vol suffisamment reguliere et on developpe autour du forward :
\[
\sigma_{\mathrm{loc}}(t,S)
=
\sigma(t)+\alpha(t)x+\frac{\beta(t)}{2}x^2,
\qquad
x=\ln(S/F_t).
\tag{2.44}
\]

Ici :

- \(\sigma(t)\) est le niveau local ATM ;
- \(\alpha(t)\) est le **skew local** ;
- \(\beta(t)\) est la **courbure locale**.

### 6.1 Insertion dans la formule de Bergomi

Dans (2.42), l'argument de moneyness est
\[
X(t,y)
=
\frac{t}{T}x_K
+
\sigma_0\sqrt{\frac{(T-t)t}{T}}\,y.
\tag{2.46}
\]

En remplacant \(\sigma_{\mathrm loc}(t,S)\) par son developpement,
\[
\hat\sigma_{KT}
=
\frac1T\int_0^T \sigma(t)\,dt
+
\frac1T\int_0^T dt\int_{\mathbb R}\phi(y)
\left[
\alpha(t)X(t,y)+\frac{\beta(t)}{2}X(t,y)^2
\right]dy.
\]

Or
\[
\int_{\mathbb R}\phi(y)\,y\,dy=0,
\qquad
\int_{\mathbb R}\phi(y)\,y^2\,dy=1.
\]

Donc
\[
\int \phi(y)X(t,y)\,dy=\frac{t}{T}x_K,
\]
\[
\int \phi(y)X(t,y)^2\,dy
=
\left(\frac{t}{T}\right)^2 x_K^2
+
\sigma_0^2\frac{(T-t)t}{T}.
\]

On en deduit
\[
\hat\sigma_{KT}
=
\frac1T\int_0^T \sigma(t)\,dt
+
\frac{\sigma_0^2}{2T}\int_0^T \frac{(T-t)t}{T}\beta(t)\,dt
+
\left(
\frac1T\int_0^T \frac{t}{T}\alpha(t)\,dt
\right)x_K
+
\frac12
\left(
\frac1T\int_0^T \left(\frac{t}{T}\right)^2\beta(t)\,dt
\right)x_K^2.
\tag{2.47}
\]

### 6.2 Skew ATMF et courbure ATMF

En derivant par rapport a \(\ln K\), c'est-a-dire par rapport a \(x_K\), puis en evaluant en \(K=F_T\) (donc \(x_K=0\)), on obtient :
\[
\mathcal S_T
:=
\left.\frac{\partial \hat\sigma_{KT}}{\partial \ln K}\right|_{K=F_T}
=
\frac1T\int_0^T \frac{t}{T}\alpha(t)\,dt.
\tag{2.48}
\]

De meme,
\[
\left.\frac{\partial^2 \hat\sigma_{KT}}{\partial (\ln K)^2}\right|_{K=F_T}
=
\frac1T\int_0^T \left(\frac{t}{T}\right)^2 \beta(t)\,dt.
\tag{2.49}
\]

### 6.3 Interpretation du poids \(t/T\)

Le noyau
\[
\frac{t}{T}
\]
donne plus de poids aux dates proches de l'echeance.

Intuition :

- au debut de la vie de l'option, le strike final compte moins ;
- a l'approche de la maturite, la zone proche de \(K\) devient de plus en plus pertinente ;
- donc le skew implicite regarde surtout les valeurs de \(\alpha(t)\) pour les temps assez proches de \(T\).

### 6.4 Cas \(\alpha,\beta\) constants

Si \(\alpha(t)\equiv \alpha\) et \(\beta(t)\equiv \beta\), alors
\[
\mathcal S_T=\frac{\alpha}{2},
\qquad
\left.\frac{\partial^2 \hat\sigma_{KT}}{\partial (\ln K)^2}\right|_{K=F_T}
=\frac{\beta}{3}.
\tag{2.50}
\]

Interpretation :

- l'implicite **lisse** le skew local : facteur \(1/2\) ;
- l'implicite **lisse davantage** la courbure locale : facteur \(1/3\).

Ce lissage est exactement ce qu'on doit attendre d'une moyenne trajectorielle.

### 6.5 Cas d'un skew local en loi de puissance

Supposons
\[
\alpha(t)=
\begin{cases}
\alpha_0, & t\le \tau_0,\\[0.2cm]
\alpha_0\left(\frac{\tau_0}{t}\right)^\gamma, & t>\tau_0,
\end{cases}
\qquad \gamma<2.
\tag{2.51}
\]

Alors, par (2.48),

- si \(T\le \tau_0\),
  \[
  \mathcal S_T=\frac{\alpha_0}{2};
  \]

- si \(T\ge \tau_0\),
  \[
  \mathcal S_T
  =
  \frac{1}{2-\gamma}\alpha_0\left(\frac{\tau_0}{T}\right)^\gamma
  -\frac{\gamma}{2(2-\gamma)}\alpha_0\left(\frac{\tau_0}{T}\right)^2.
  \tag{2.52}
  \]

Pour les longues maturites, le dernier terme est negligeable, donc
\[
\mathcal S_T
\sim
\frac{1}{2-\gamma}\alpha_0\left(\frac{\tau_0}{T}\right)^\gamma.
\tag{2.53}
\]

Interpretation :

1. Le **meme exposant** \(\gamma\) reapparait au niveau implicite.
2. Le passage local vol \(\to\) implicite modifie surtout l'echelle, pas le regime asymptotique.
3. Si l'equity skew local decroit comme \(t^{-1/2}\), alors le skew implicite long terme decroit lui aussi comme \(T^{-1/2}\).

---

## 7. Extension utile pour l'oral : dynamique ATMF et ratio \(R_T\)

Cette partie depasse legerement le strict contenu de 2.4.6, mais elle decoule directement du mecanisme de moyenne de Bergomi et elle est indispensable pour commenter les slides.

### 7.1 Variation du smile a strike fixe quand le spot bouge

On garde la fonction de volatilite locale **en coordonnees absolues** \((t,S)\) et on deplace le spot initial
\[
S_0 \mapsto S_0 e^\varepsilon.
\]

Le nouveau forward est
\[
F_t^\varepsilon = F_t e^\varepsilon,
\]
et le nouveau log-moneyness du strike fixe \(K\) est
\[
x_K^\varepsilon = \ln\left(\frac{K}{F_T^\varepsilon}\right)=x_K-\varepsilon.
\]

Dans la formule de Bergomi (2.42), le point d'espace visite devient
\[
S^{(\varepsilon)}(t,y)
=
F_t^\varepsilon
\exp\left(
\frac{t}{T}x_K^\varepsilon
+
\sigma_0\sqrt{\frac{(T-t)t}{T}}\,y
\right).
\]

Si l'on mesure la local vol par rapport au **forward initial** \(F_t\), alors
\[
\ln\left(\frac{S^{(\varepsilon)}(t,y)}{F_t}\right)
=
\varepsilon+\frac{t}{T}(x_K-\varepsilon)
+
\sigma_0\sqrt{\frac{(T-t)t}{T}}\,y
=
\frac{t}{T}x_K
+
\left(1-\frac{t}{T}\right)\varepsilon
+
\sigma_0\sqrt{\frac{(T-t)t}{T}}\,y.
\]

En utilisant le developpement local
\[
\sigma_{\mathrm loc}(t,S)=\bar\sigma(t)+\alpha(t)x+\frac{\beta(t)}2 x^2,
\]
on deduit, a l'ordre 1,
\[
\left.
\frac{\partial \hat\sigma_{KT}}{\partial \ln S_0}
\right|_{K=F_T}
=
\frac1T\int_0^T
\left(1-\frac{t}{T}\right)\alpha(t)\,dt.
\]

### 7.2 Variation de la volatilite ATMF

Maintenant, on ne garde plus le strike fixe : on suit le strike ATMF, donc \(K=F_T^\varepsilon\).
Dans ce cas le log-moneyness terminal redevient nul :
\[
x_K^\varepsilon=0.
\]

Le point visite verifie alors
\[
\ln\left(\frac{S^{(\varepsilon)}(t,y)}{F_t}\right)
=
\varepsilon
+
\sigma_0\sqrt{\frac{(T-t)t}{T}}\,y.
\]

Le coefficient de \(\varepsilon\) vaut maintenant 1, ce qui donne
\[
\frac{d\hat\sigma_{F_T,T}}{d\ln S_0}
=
\frac1T\int_0^T \alpha(t)\,dt.
\]

Interpretation :

> la vol ATMF reagit au spot en agregant tout le skew local entre 0 et \(T\), sans le facteur de lissage \(t/T\).

### 7.3 Lien entre dynamique ATMF et skew ATMF

Comme
\[
\mathcal S_T
=
\frac1T\int_0^T \frac{t}{T}\alpha(t)\,dt,
\]
on obtient
\[
\frac{d\hat\sigma_{F_T,T}}{d\ln S_0}
=
\mathcal S_T + \frac1T\int_0^T \mathcal S_t\,dt.
\]

**Preuve.** On a
\[
\mathcal S_t = \frac{1}{t^2}\int_0^t u\,\alpha(u)\,du.
\]

Donc
\[
\frac1T\int_0^T \mathcal S_t\,dt
=
\frac1T\int_0^T \frac{1}{t^2}\int_0^t u\,\alpha(u)\,du\,dt.
\]

Par Fubini,
\[
\frac1T\int_0^T \mathcal S_t\,dt
=
\frac1T\int_0^T u\alpha(u)\left(\int_u^T \frac{dt}{t^2}\right)du
=
\frac1T\int_0^T u\alpha(u)\left(\frac1u-\frac1T\right)du.
\]

Donc
\[
\frac1T\int_0^T \mathcal S_t\,dt
=
\frac1T\int_0^T \alpha(u)\,du
-\frac{1}{T^2}\int_0^T u\alpha(u)\,du
=
\frac1T\int_0^T \alpha(u)\,du - \mathcal S_T.
\]

D'ou l'identite annoncee.

### 7.4 Definition du ratio \(R_T\)

On definit
\[
R_T
:=
\frac{\dfrac{d\hat\sigma_{F_T,T}}{d\ln S_0}}{\mathcal S_T}.
\]

En utilisant la formule precedente,
\[
R_T
=
1+\frac{1}{T\mathcal S_T}\int_0^T \mathcal S_t\,dt.
\]

### 7.5 Interpretation de \(R_T\)

\(R_T\) mesure :

> de combien bouge la volatilite ATMF quand le spot bouge, en unite de skew ATMF.

Regimes archetypaux :

- **sticky-strike** : \(R_T=1\) ;
- **sticky-delta** : \(R_T=0\) ;
- **local vol simple avec skew local constant** : \(R_T=2\).

En effet, si \(\alpha(t)\equiv \alpha\), alors
\[
\mathcal S_T=\frac{\alpha}{2},
\qquad
\frac{d\hat\sigma_{F_T,T}}{d\ln S_0}=\alpha,
\]
donc
\[
R_T=\frac{\alpha}{\alpha/2}=2.
\]

La "regle du \(R=2\)" signifie donc :

> **dans un modele local vol simple, la volatilite ATMF se deplace deux fois plus vite que le skew ATMF.**

### 7.6 Cas ou le skew implicite suit une loi de puissance

Si empiriquement
\[
\mathcal S_T = c T^{-\gamma},
\qquad 0\le \gamma <1,
\]
alors
\[
\frac1T\int_0^T \mathcal S_t\,dt
=
\frac{c}{T}\int_0^T t^{-\gamma}\,dt
=
\frac{c}{1-\gamma}T^{-\gamma}
=
\frac{1}{1-\gamma}\mathcal S_T.
\]

Donc
\[
R_T
=
1+\frac{1}{1-\gamma}
=
\frac{2-\gamma}{1-\gamma}.
\]

Exemple : si \(\gamma=1/2\),
\[
R_T=3.
\]

Cela explique pourquoi les smiles locaux sont souvent juges "trop rigides" du point de vue des dynamiques de marche.

---

## 8. Section 2.4.6 -- Resultat exact en courte maturite

C'est le plus beau resultat du passage.

### 8.1 Point de depart : Dupire en variables \((T,y)\)

On repart de
\[
\sigma_{\mathrm loc}^2(T,y)
=
\frac{\hat\sigma^2+2T\hat\sigma \hat\sigma_T}
{
\left(1-y\frac{\hat\sigma_y}{\hat\sigma}\right)^2
+T\left(\hat\sigma_y^2+\hat\sigma \hat\sigma_{yy}\right)
-\left(\frac14+\frac{1}{T\hat\sigma^2}\right)T^2\hat\sigma^2\hat\sigma_y^2
}.
\]

On suppose \(\hat\sigma(T,y)\) suffisamment reguliere quand \(T\to 0\).

### 8.2 Passage a la limite \(T\to 0\)

Quand \(T\to 0\),

- au numerateur :
  \[
  \hat\sigma^2+2T\hat\sigma \hat\sigma_T
  =
  \hat\sigma(0,y)^2 + o(1);
  \]

- au denominateur, tous les termes multiplies par \(T\) ou \(T^2\) disparaissent, donc
  \[
  \left(1-y\frac{\hat\sigma_y}{\hat\sigma}\right)^2 + o(1).
  \]

On obtient donc
\[
\sigma_{\mathrm loc}(0,S_0 e^y)^2
=
\frac{\hat\sigma(0,y)^2}
{\left(1-y\frac{\hat\sigma_y(0,y)}{\hat\sigma(0,y)}\right)^2}.
\]

Comme les volatilites sont positives,
\[
\frac{1}{\sigma_{\mathrm loc}(0,S_0 e^y)}
=
\frac{1-y\frac{\hat\sigma_y(0,y)}{\hat\sigma(0,y)}}{\hat\sigma(0,y)}.
\]

Or
\[
\frac{d}{dy}\left(\frac{y}{\hat\sigma(0,y)}\right)
=
\frac{1}{\hat\sigma(0,y)}
-y\frac{\hat\sigma_y(0,y)}{\hat\sigma(0,y)^2}
=
\frac{1-y\hat\sigma_y/\hat\sigma}{\hat\sigma}.
\]

Donc
\[
\frac{d}{dy}\left(\frac{y}{\hat\sigma(0,y)}\right)
=
\frac{1}{\sigma_{\mathrm loc}(0,S_0 e^y)}.
\]

En integrant de \(0\) a \(y\),
\[
\frac{y}{\hat\sigma(0,y)}
=
\int_0^y \frac{du}{\sigma_{\mathrm loc}(0,S_0 e^u)}.
\]

Finalement,
\[
\boxed{
\frac{1}{\hat\sigma(0,y)}
=
\frac1y\int_0^y \frac{du}{\sigma_{\mathrm loc}(0,S_0 e^u)}
}.
\tag{2.54}
\]

En variable strike,
\[
\boxed{
\frac{1}{\hat\sigma(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm loc}(0,S)}
}.
\]

### 8.3 Interpretation : moyenne harmonique, pas moyenne arithmetique

Ce resultat est exact et il est tres instructif.

Pour \(T\to 0\),

- il n'y a presque plus de moyenne temporelle ;
- la quantite naturelle n'est plus une moyenne de \(\sigma^2\) ;
- c'est l'inverse de l'implicite qui est la moyenne de l'inverse de la local vol.

Autrement dit :

> **a tres courte maturite, l'implicite est une moyenne harmonique spatiale de la volatilite locale entre le spot et le strike.**

### 8.4 Pourquoi c'est naturel

Si \(\sigma_{\mathrm loc}(0,S)\) est tres faible ou nulle sur un intervalle separant \(S_0\) de \(K\), alors, sur un tres petit temps :

- le spot a du mal, voire est incapable, de traverser cette zone ;
- la probabilite d'atteindre le strike s'effondre ;
- l'implicite correspondante doit donc devenir tres faible.

Une moyenne harmonique rend exactement ce phenomene visible.

### 8.5 Lecture par changement de variable de type Lamperti

Le commentaire de Bergomi renvoie a l'idee suivante :
\[
z(S)=\int_{S_0}^{S}\frac{d\xi}{\xi \sigma_{\mathrm loc}(0,\xi)}.
\]

A tres courte maturite, la variable transformee \(z_t\) est approximativement gaussienne. La distance pertinente entre \(S_0\) et \(K\) est donc la distance
\[
z(K)-z(S_0)=\int_{S_0}^K \frac{dS}{S\sigma_{\mathrm loc}(0,S)},
\]
ce qui explique immediatement l'apparition de la moyenne harmonique.

---

## 9. Lien avec les modeles de volatilite stochastique

### 9.1 Projection de Gyongy

Un modele local vol peut etre vu comme la projection markovienne d'un modele a volatilite stochastique :
\[
\sigma_{\mathrm loc}(t,S)^2
=
\mathbb E\big[v_t \mid S_t=S\big].
\]

Cette identite reproduit les marginales de \(S_t\), donc les prix de vanilles europeennes a la date 0.

Mais elle ne dit pas que les **dynamiques futures du smile** seront les memes.

### 9.2 Difference essentielle avec la vol stochastique

Dans un modele de vol stochastique :

- le smile futur depend d'un facteur de vol aleatoire ;
- la corrrelation spot/vol (leverage) joue un role essentiel ;
- les smiles forward sont eux-memes aleatoires.

Dans un modele local vol :

- tout est encode de facon deterministe dans \(\sigma_{\mathrm loc}(t,S)\) ;
- le smile futur est deduit mecaniquement du seul niveau futur du spot ;
- cela conduit souvent a des dynamiques trop rigides du type \(R\approx 2\) ou davantage.

Financierement :

> local vol reproduit bien la coupe de smile d'aujourd'hui, mais souvent mal la facon dont cette coupe se deplace demain.

---

## 10. Avertissements, limites et points de vigilance

### 10.1 Limites de l'approximation weak local vol

Les formules (2.40) et (2.42) sont des developpements d'ordre 1. Elles deviennent fragiles si :

- le smile local est fort ;
- on s'eloigne trop de l'ATM ;
- la maturite est longue ;
- la structure par terme est marquee ;
- on cherche des niveaux absolus de vol plutot que des differences de vol.

### 10.2 Ce qui reste fiable

Ce que Bergomi souligne explicitement :

- le **skew** est mieux approche que le niveau absolu ;
- la **courbure pres du forward** est interpretable ;
- les **lois d'echelle** en maturite sont souvent bien capturees.

### 10.3 Limites structurelles du modele local vol

Le modele local vol :

1. calibre exactement la surface de vanilles a la date 0 ;
2. mais peut produire des dynamiques de smile peu realistes ;
3. peut surestimer la rigidite du skew ;
4. n'introduit pas de facteur de vol propre, donc pas de forward smile aleatoire.

Pour des problemes de dynamique de smile, les modeles de vol stochastique ou locale-stochastique sont souvent plus adaptes.

---

## 11. Resume ultra-condense des formules a connaitre

### 11.1 Identite fondamentale

\[
\hat\sigma_{KT}^2
=
\frac{
\mathbb E\left[\int_0^T e^{-rt}S_t^2\Gamma_t\,\sigma_{\mathrm loc}(t,S_t)^2\,dt\right]
}
{
\mathbb E\left[\int_0^T e^{-rt}S_t^2\Gamma_t\,dt\right]
}.
\]

### 11.2 Approximation weak local vol

\[
\hat\sigma_{KT}
\approx
\frac1T\int_0^T dt\int \phi(y)\,
\sigma_{\mathrm loc}\!\left(
t,
F_t e^{\frac{t}{T}x_K+\sigma_0\sqrt{\frac{(T-t)t}{T}}y}
\right)dy.
\]

### 11.3 Smile pres du forward

\[
\sigma_{\mathrm loc}(t,S)=\sigma(t)+\alpha(t)x+\frac{\beta(t)}2 x^2,
\qquad x=\ln(S/F_t).
\]

\[
\mathcal S_T
=
\left.\partial_{\ln K}\hat\sigma_{KT}\right|_{K=F_T}
=
\frac1T\int_0^T \frac{t}{T}\alpha(t)\,dt.
\]

\[
\left.\partial_{\ln K}^2\hat\sigma_{KT}\right|_{K=F_T}
=
\frac1T\int_0^T \left(\frac{t}{T}\right)^2\beta(t)\,dt.
\]

### 11.4 Dynamique ATMF

\[
\frac{d\hat\sigma_{F_T,T}}{d\ln S_0}
=
\frac1T\int_0^T \alpha(t)\,dt.
\]

\[
R_T
=
\frac{\dfrac{d\hat\sigma_{F_T,T}}{d\ln S_0}}{\mathcal S_T}
=
1+\frac{1}{T\mathcal S_T}\int_0^T \mathcal S_t\,dt.
\]

### 11.5 Courte maturite exacte

\[
\frac{1}{\hat\sigma(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm loc}(0,S)}.
\]

---

## 12. Phrase finale d'examen

Si je devais resumer tout Bergomi 2.4 en une phrase :

> **Dupire donne la local vol a partir de l'implicite ; Bergomi montre reciproquement que l'implicite issue d'une local vol est une moyenne ponderee de la local vol, que ce mecanisme lisse le skew et la courbure, et qu'en courte maturite l'objet exact n'est plus une moyenne de \(\sigma^2\) mais une moyenne harmonique spatiale de \(\sigma\).**
