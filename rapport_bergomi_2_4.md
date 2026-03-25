# Bergomi, Chapitre 2.4 -- Des volatilites locales aux volatilites implicites

## Objet de ce rapport

Ce document suit **strictement** le contenu que tu as demande :

- expliquer **tous les resultats de 2.4 a 2.4.6** ;
- donner les **derivations detaillees** ;
- poser clairement les **hypotheses** ;
- expliquer les **Taylor expansions** dans le cadre weak local vol ;
- interpreter chaque formule ;
- mettre en evidence :
  - l'identite cle "implied volatility = weighted average of local vol" ;
  - la dependance en \(\alpha(t)\) ;
  - la dynamique ATMF ;
  - la definition et l'interpretation de \(R_T\) ;
- faire le lien avec :
  - **Dupire** ;
  - **les modeles de volatilite stochastique** ;
  - **l'intuition financiere de marche** ;
- donner des **warnings** sur :
  - les approximations ;
  - les limites du modele de volatilite locale.

Mais je vais en plus respecter une exigence pedagogique simple :

> ne jamais introduire une formule sans avoir dit avant ce qu'on cherche, ce que signifie chaque objet, et pourquoi l'idee est naturelle.

---

## 1. Question de fond du chapitre

Le chapitre 2.4 de Bergomi pose la question inverse de Dupire.

### 1.1. Problematique de Dupire

La formule de Dupire dit, en substance :

> si l'on connait toute la surface de prix vanilles, ou equivalemment la surface de volatilite implicite, alors on peut reconstruire une fonction de volatilite locale \(\sigma_{\mathrm{loc}}(t,S)\).

Donc Dupire fait le chemin :

\[
\widehat{\sigma}(K,T)\quad \longrightarrow \quad \sigma_{\mathrm{loc}}(t,S).
\]

### 1.2. Problematique de Bergomi en 2.4

Bergomi veut faire le chemin inverse :

\[
\sigma_{\mathrm{loc}}(t,S)\quad \longrightarrow \quad \widehat{\sigma}(K,T).
\]

Mais il ne cherche pas seulement a retrouver le **niveau** de l'implicite.

Il veut aussi comprendre :

1. comment le smile implicite se construit a partir de la local vol ;
2. comment le skew implicite pres du forward depend de la structure de la local vol ;
3. quelle dynamique du smile est imposee par un modele local vol.

Autrement dit, le chapitre 2.4 ne parle pas seulement de **calibration statique** ; il parle de **dynamique du smile**.

---

## 2. Rappels de cours indispensables

Comme tu m'as dit que certaines definitions n'etaient pas claires, je repars ici des notions de base.

### 2.1. Mesure risque-neutre

Sous la mesure risque-neutre, dans un modele local vol, on ecrit :

\[
dS_t=(r-q)S_t\,dt+\sigma_{\mathrm{loc}}(t,S_t)S_t\,dW_t.
\]

Ici :

- \(S_t\) est le spot ;
- \(r\) est le taux sans risque ;
- \(q\) est le taux de dividende / carry ;
- \(W_t\) est un brownien sous la mesure de pricing ;
- \(\sigma_{\mathrm{loc}}(t,S)\) est une fonction deterministe du temps et du spot.

### 2.2. Prix d'une option et EDP de pricing

Si \(P(t,S)\) est le prix d'une option europeenne, alors sous des hypotheses standard :

\[
\partial_t P+(r-q)S\partial_S P+\frac12 \sigma^2(t,S)S^2\partial_{SS}P-rP=0.
\]

Cette EDP vient du cours :

- par application de la formule d'Itô ;
- par replication / absence d'arbitrage ;
- ou equivalemment par Feynman-Kac.

### 2.3. Delta, gamma, theta

Pour une option de prix \(P(t,S)\), on note :

\[
\Delta=\partial_S P,\qquad
\Gamma=\partial_{SS}P,\qquad
\Theta=\partial_t P.
\]

Interpretation :

- \(\Delta\) mesure la sensibilite lineaire au spot ;
- \(\Gamma\) mesure la courbure, donc la variation du delta lui-meme ;
- \(\Theta\) mesure l'effet du passage du temps.

### 2.4. Qu'est-ce qu'un P&L ?

Le **P&L** ("Profit and Loss") est le gain ou la perte sur un petit intervalle de temps.

Si on detient une option et qu'on la couvre en delta, le P&L residuel est ce qui reste apres avoir neutralise le terme lineaire en \(dS_t\).

Sur un petit temps \(dt\), par Itô :

\[
dP=\partial_t P\,dt+\partial_S P\,dS+\frac12 \partial_{SS}P\,d\langle S\rangle.
\]

Si l'on prend en face une position de \(-\partial_S P\) actions, le terme en \(dS\) est neutralise.

Le P&L residuel ressemble alors a :

\[
\partial_t P\,dt+\frac12 \partial_{SS}P\,d\langle S\rangle.
\]

Donc, dans une couverture delta, le terme residuel est de type :

- theta ;
- plus gamma fois variation quadratique.

**C'est exactement pour cela que le gamma apparait dans Bergomi.**

### 2.5. Idee de "mauvais modele"

Si je couvre avec le **bon** modele, le P&L residuel est celui impose par l'EDP du modele.

Si je couvre avec un **mauvais** modele de volatilite, il reste un ecart de P&L.

Bergomi exploite precisement cette idee.

---

## 3. Section 2.4.1 -- Identite exacte : la variance implicite comme moyenne ponderee

### 3.1. Idee intuitive avant le calcul

Bergomi compare deux modeles :

- **modele I** : modele de reference, avec prix \(P_1(t,S)\) et volatilite instantanee \(\sigma_1(t,S)\) ;
- **modele II** : modele "reel", dans lequel le sous-jacent suit
  \[
  dS_t=(r-q)S_t\,dt+\sigma_{2,t}S_t\,dW_t.
  \]

Le but est de mesurer :

> combien vaut la difference de prix entre les deux modeles si on couvre delta l'option avec le modele I alors que le monde suit le modele II ?

### 3.2. Quantite actualisee

On introduit

\[
Q_t=e^{-rt}P_1(t,S_t).
\]

Pourquoi ?

Parce qu'en finance, les quantites actualisees sont les bonnes quantites a etudier ; si le modele est "juste", elles se comportent comme des martingales.

### 3.3. Application de la formule d'Itô

Sous le modele II :

\[
dQ_t
=
e^{-rt}
\left[
\left(-rP_1+\partial_t P_1\right)dt
+
\partial_S P_1\,dS_t
+
\frac12 \partial_{SS}P_1\,d\langle S\rangle_t
\right].
\]

Comme

\[
d\langle S\rangle_t=\sigma_{2,t}^2 S_t^2\,dt,
\]

on obtient

\[
dQ_t
=
e^{-rt}
\left[
\left(-rP_1+\partial_t P_1\right)dt
+
\partial_S P_1\,dS_t
+
\frac12 \sigma_{2,t}^2 S_t^2\partial_{SS}P_1\,dt
\right].
\]

### 3.4. Esperance conditionnelle

En prenant l'esperance conditionnelle sous le modele II, le terme brownien disparait :

\[
\mathbb E_2[dQ_t\mid \mathcal F_t]
=
e^{-rt}
\left[
-rP_1+\partial_t P_1+(r-q)S_t\partial_S P_1+\frac12 \sigma_{2,t}^2 S_t^2\partial_{SS}P_1
\right]dt.
\]

### 3.5. Utilisation de l'EDP du modele I

Dans le modele I, \(P_1\) satisfait :

\[
\partial_t P_1+(r-q)S\partial_S P_1+\frac12 \sigma_1^2(t,S)S^2\partial_{SS}P_1-rP_1=0.
\]

Donc :

\[
-rP_1+\partial_t P_1+(r-q)S_t\partial_S P_1
=
-\frac12 \sigma_1^2(t,S_t)S_t^2\partial_{SS}P_1.
\]

En remplaçant :

\[
\mathbb E_2[dQ_t\mid \mathcal F_t]
=
e^{-rt}\frac12 S_t^2\partial_{SS}P_1(t,S_t)\big(\sigma_{2,t}^2-\sigma_1^2(t,S_t)\big)\,dt.
\]

### 3.6. Interpretation : apparition du gamma/theta P&L

Cette formule dit que le drift residuel de la couverture delta est proportionnel a :

\[
\frac12 e^{-rt}S_t^2\Gamma_t\big(\sigma_{2,t}^2-\sigma_1^2(t,S_t)\big).
\]

Interpretation :

- si le modele de couverture a la bonne variance instantanee, ce terme est nul ;
- sinon il reste un P&L residuel ;
- ce P&L est pondere par le gamma.

Donc :

> plus l'option est convexe, plus une erreur sur la variance instantanee coute cher.

### 3.7. Integration jusqu'a maturite

En integrant :

\[
\mathbb E_2[Q_T]
=
Q_0+
\mathbb E_2\left[
\int_0^T e^{-rt}\frac12 S_t^2\partial_{SS}P_1(t,S_t)\big(\sigma_{2,t}^2-\sigma_1^2(t,S_t)\big)\,dt
\right].
\]

Mais

\[
Q_T=e^{-rT}f(S_T),
\]

donc

\[
\mathbb E_2[Q_T]=P_2(0,S_0,\cdot),
\qquad
Q_0=P_1(0,S_0).
\]

Finalement :

\[
\boxed{
P_2(0,S_0,\cdot)
=
P_1(0,S_0)
+
\mathbb E_2\left[
\int_0^T e^{-rt}\frac12 S_t^2\partial_{SS}P_1(t,S_t)\big(\sigma_{2,t}^2-\sigma_1^2(t,S_t)\big)\,dt
\right].
}
\]

### 3.8. Phrase simple a retenir

Cette formule signifie :

> prix dans le modele II = prix dans le modele I + P&L moyen de la couverture delta faite avec le modele I alors que le monde suit le modele II.

### 3.9. Choix malin du modele I : Black-Scholes avec volatilite implicite

On choisit maintenant comme modele I le modele Black-Scholes de volatilite constante egale a la volatilite implicite de l'option :

\[
\sigma_1(t,S)\equiv \widehat{\sigma}_{K,T}.
\]

Par definition de l'implicite :

\[
P_1(0,S_0)=P_2(0,S_0,\cdot).
\]

Donc :

\[
0=
\mathbb E_2\left[
\int_0^T e^{-rt}\frac12 S_t^2\Gamma_t^{BS}(\widehat{\sigma}_{K,T})
\big(\sigma_{2,t}^2-\widehat{\sigma}_{K,T}^2\big)\,dt
\right].
\]

On reordonne :

\[
\widehat{\sigma}_{K,T}^2
\mathbb E_2\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}(\widehat{\sigma}_{K,T})\,dt
\right]
=
\mathbb E_2\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}(\widehat{\sigma}_{K,T})\,\sigma_{2,t}^2\,dt
\right].
\]

Donc :

\[
\boxed{
\widehat{\sigma}_{K,T}^{\,2}
=
\frac{
\mathbb E_2\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}(\widehat{\sigma}_{K,T})\,\sigma_{2,t}^2\,dt
\right]
}{
\mathbb E_2\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}(\widehat{\sigma}_{K,T})\,dt
\right]
}.
}
\]

### 3.10. Cas du modele local vol

Si le modele II est le modele de volatilite locale :

\[
\sigma_{2,t}=\sigma_{\mathrm{loc}}(t,S_t),
\]

alors :

\[
\boxed{
\widehat{\sigma}_{K,T}^{\,2}
=
\frac{
\mathbb E^{\mathrm{loc}}\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}(\widehat{\sigma}_{K,T})
\sigma_{\mathrm{loc}}^2(t,S_t)\,dt
\right]
}{
\mathbb E^{\mathrm{loc}}\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}(\widehat{\sigma}_{K,T})\,dt
\right]
}.
}
\]

### 3.11. Interprétation exacte de l'identite cle

Cette formule dit :

- la **variance implicite** n'est pas arbitraire ;
- elle est une **moyenne ponderee** de la **variance locale** ;
- les poids sont les **dollar gammas actualises**.

**Warning tres important :**

- la formule exacte porte sur \(\widehat{\sigma}^2\), pas sur \(\widehat{\sigma}\) ;
- donc **exactement**, c'est une moyenne de variances locales ;
- ce n'est qu'**a l'ordre 1** qu'on obtient une moyenne de volatilites locales.

---

## 4. Section 2.4.2 -- Approximation pour une local vol faiblement locale

### 4.1. Pourquoi l'identite exacte ne suffit pas

La formule precedente est exacte, mais elle est **implicite** :

- \(\widehat{\sigma}_{K,T}\) apparait deja dans le membre de droite ;
- a travers le gamma Black-Scholes ;
- et a travers les densites utilisees dans l'esperance.

On cherche donc une approximation analytiquement exploitable.

### 4.2. Hypothese weak local vol

On pose la variance locale :

\[
u(t,S)=\sigma_{\mathrm{loc}}^2(t,S)=u_0(t)+\delta u(t,S),
\]

ou :

- \(u_0(t)\) est une variance deterministe en temps ;
- \(\delta u(t,S)\) est une petite perturbation.

Si \(\delta u=0\), on est dans un modele de Black-Scholes a vol deterministe en temps.

### 4.3. Idee du developpement de Taylor

Le membre de droite de la formule exacte est un quotient :

\[
\frac{\mathbb E[(u_0+\delta u)\bullet]}{\mathbb E[\bullet]}.
\]

On developpe au premier ordre en \(\delta u\).

L'idee cle de Bergomi est :

> a l'ordre 1, la contribution venant de la perturbation de la densite s'annule.

Pourquoi ?

Parce que si l'on ecrit

\[
\frac{A_0+\delta A}{B_0+\delta B}
=
\frac{A_0}{B_0}
+
\frac{\delta A\,B_0-A_0\,\delta B}{B_0^2}
+
O(\delta^2),
\]

et que \(A_0=u_0 B_0\), alors

\[
\frac{\delta A\,B_0-A_0\,\delta B}{B_0^2}
=
\frac{\delta A-u_0\delta B}{B_0}.
\]

La partie de \(\delta A\) qui provient du changement de loi vaut justement \(u_0\delta B\), donc elle se compense.

Il reste seulement la contribution **explicite** de \(\delta u\).

### 4.4. Formule a l'ordre 1

On obtient :

\[
\delta\!\left(\widehat{\sigma}_{K,T}^{\,2}\right)
=
\frac{
\mathbb E_{\sigma_0}\left[
\int_0^T e^{-rt}\delta u(t,S_t)S_t^2\Gamma_t^{(0)}\,dt
\right]
}{
\mathbb E_{\sigma_0}\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{(0)}\,dt
\right]
}.
\]

Le point essentiel est que :

- la loi du spot utilisee ici est celle du modele de reference ;
- le gamma est egalement calcule dans le modele de reference.

Donc la formule devient calculable explicitement.

### 4.5. Forme finale de Bergomi

Apres calcul explicite de la densite lognormale et du gamma Black-Scholes, Bergomi obtient :

\[
\boxed{
\delta\!\left(\widehat{\sigma}_{K,T}^{\,2}\right)
=
\frac{1}{T}\int_0^T dt\int_{\mathbb R}\phi(y)\,
\delta u\!\left(
t,\,
F_t\exp\!\left(
\frac{\omega_t}{\omega_T}x_K+\frac{\sqrt{(\omega_T-\omega_t)\omega_t}}{\sqrt{\omega_T}}y
\right)
\right)
dy
}
\]

avec :

\[
\omega_t=\int_0^t \sigma_0^2(s)\,ds,
\qquad
x_K=\ln\left(\frac{K}{F_T}\right).
\]

Donc :

\[
\boxed{
\widehat{\sigma}_{K,T}^{\,2}
=
\frac{1}{T}\int_0^T dt\int_{\mathbb R}\phi(y)\,
u\!\left(
t,\,
F_t\exp\!\left(
\frac{\omega_t}{\omega_T}x_K+\frac{\sqrt{(\omega_T-\omega_t)\omega_t}}{\sqrt{\omega_T}}y
\right)
\right)
dy.
}
\]

### 4.6. Interprétation

Cette formule dit que, a l'ordre 1 :

- la **variance implicite** est une moyenne de la **variance instantanee locale** ;
- les points \((t,S)\) explores ne sont pas arbitraires ;
- ils sont pondérés par une variable gaussienne \(y\) qui decrit les trajectoires intermediaires.

**Attention :**

- la formule est exacte si \(u\) depend seulement de \(t\) ;
- sinon ce n'est qu'une approximation d'ordre 1.

---

## 5. Section 2.4.3 et 2.4.4 -- Expansion autour d'une vol constante et interpretation geometrique

### 5.1. Cas d'une vol de reference constante

On suppose maintenant :

\[
\sigma_{\mathrm{loc}}(t,S)=\sigma_0+\delta \sigma(t,S),
\]

avec \(\sigma_0\) constante et \(\delta\sigma\) petit.

Alors :

\[
u(t,S)=\sigma_0^2+2\sigma_0\delta\sigma(t,S)+O(\delta\sigma^2).
\]

Comme

\[
\widehat{\sigma}_{K,T}^{\,2}
=
\sigma_0^2+2\sigma_0\,\delta\widehat{\sigma}_{K,T}+O(\delta\sigma^2),
\]

on obtient la formule plus simple :

\[
\boxed{
\widehat{\sigma}_{K,T}
\approx
\frac{1}{T}\int_0^T dt\int_{\mathbb R}\phi(y)\,
\sigma_{\mathrm{loc}}\!\left(
t,\,
F_t\exp\!\left(
\frac{t}{T}x_K+\sigma_0\sqrt{\frac{(T-t)t}{T}}\,y
\right)
\right)dy.
}
\]

### 5.2. Pourquoi on dit parfois "implied volatility = weighted average of local vol" ?

Attention a la precision mathematique :

- **exactement** : \(\widehat{\sigma}^2\) est moyenne de \(\sigma_{\mathrm{loc}}^2\) ;
- **a l'ordre 1 autour d'une vol constante** : \(\widehat{\sigma}\) devient une moyenne de \(\sigma_{\mathrm{loc}}\).

C'est pour cela qu'on resume souvent l'idee en disant :

> implied volatility = weighted average of local vol.

Mais ce slogan n'est **rigoureux** qu'a l'ordre 1.

### 5.3. Chemin le plus probable

Dans la formule precedente, le terme le plus important correspond a \(y=0\).

On obtient alors le chemin central :

\[
S_{\ast}(t)=F_t\exp\left(\frac{t}{T}x_K\right).
\]

En log-espace, c'est une droite allant de \(\ln S_0\) a \(\ln K\).

Cela suggere l'approximation plus grossiere :

\[
\widehat{\sigma}_{K,T}\approx \frac1T\int_0^T
\sigma_{\mathrm{loc}}\!\left(t,F_t e^{(t/T)x_K}\right)\,dt.
\]

### 5.4. Warning de Bergomi

Bergomi insiste sur le fait que cette approximation n'est **pas numeriquement suffisante** pour les smiles equity realistes.

Pourquoi ?

Parce que :

- les smiles de marche sont forts ;
- les bid-offers sont etroits ;
- les effets de la densite importent vraiment ;
- un developpement d'ordre 1 ne suffit pas pour les **niveaux absolus** de volatilite.

En revanche, cette approximation est tres utile pour comprendre :

- le skew ;
- la courbure ;
- la dynamique pres du forward.

---

## 6. Section 2.4.5 -- Smile pres du forward

### 6.1. Parametrisation locale

On developpe la local vol autour du forward \(F_t\) :

\[
\sigma_{\mathrm{loc}}(t,S)
=
\overline{\sigma}(t)+\alpha(t)x+\frac{\beta(t)}{2}x^2,
\qquad
x=\ln\left(\frac{S}{F_t}\right).
\]

Interpretation :

- \(\overline{\sigma}(t)\) = niveau instantane de vol locale ;
- \(\alpha(t)\) = skew local instantane ;
- \(\beta(t)\) = courbure locale instantanee.

### 6.2. Remplacement dans la formule weak local vol

Dans la formule approchee, l'argument en moneyness devient :

\[
X(t,y)=\frac{t}{T}x_K+\sigma_0\sqrt{\frac{(T-t)t}{T}}\,y.
\]

Donc :

\[
\sigma_{\mathrm{loc}}(t,S(t,y))
=
\overline{\sigma}(t)+\alpha(t)X(t,y)+\frac{\beta(t)}{2}X(t,y)^2.
\]

En integrant en \(y\), Bergomi obtient :

\[
\widehat{\sigma}_{K,T}
\approx
\frac1T\int_0^T \overline{\sigma}(t)\,dt
+
\left(\frac1T\int_0^T \frac{t}{T}\alpha(t)\,dt\right)x_K
+
\frac12\left(\frac1T\int_0^T \left(\frac{t}{T}\right)^2\beta(t)\,dt\right)x_K^2
\]

a une correction additive en niveau liee a \(\beta\) pres.

### 6.3. Skew implicite ATMF

Au forward \(K=F_T\), donc \(x_K=0\), on derive :

\[
\boxed{
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
=
\frac1T\int_0^T \frac{t}{T}\alpha(t)\,dt.
}
\]

**Interpretation essentielle :**

> le skew implicite ATMF est une moyenne ponderee du skew local \(\alpha(t)\).

Le poids est \(t/T\), donc les temps proches de la maturite comptent plus.

### 6.4. Courbure implicite ATMF

De meme :

\[
\boxed{
\left.\frac{\partial^2 \widehat{\sigma}_{K,T}}{\partial (\ln K)^2}\right|_{K=F_T}
=
\frac1T\int_0^T \left(\frac{t}{T}\right)^2\beta(t)\,dt.
}
\]

Le poids \((t/T)^2\) renforce encore davantage les temps proches de \(T\).

### 6.5. Cas ou \(\alpha,\beta\) sont constants

Si \(\alpha(t)\equiv \alpha\) et \(\beta(t)\equiv \beta\), alors :

\[
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
=
\frac{\alpha}{2},
\qquad
\left.\frac{\partial^2 \widehat{\sigma}_{K,T}}{\partial (\ln K)^2}\right|_{K=F_T}
=
\frac{\beta}{3}.
\]

Interpretation :

- le skew implicite vaut **la moitie** du skew local ;
- la courbure implicite vaut **un tiers** de la courbure locale.

### 6.6. Cas d'une loi de puissance pour \(\alpha(t)\)

On suppose :

\[
\alpha(t)=
\begin{cases}
\alpha_0, & t\le \tau_0,\\
\alpha_0(\tau_0/t)^\gamma, & t>\tau_0.
\end{cases}
\]

Alors, pour \(T\) grand devant \(\tau_0\) :

\[
\boxed{
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
\sim
\frac{1}{2-\gamma}\alpha_0\left(\frac{\tau_0}{T}\right)^\gamma.
}
\]

Interpretation :

- le skew implicite a longue maturite decroit avec **le meme exposant** \(\gamma\) que le skew local ;
- \(\alpha(t)\) controle donc directement la structure par terme du skew implicite.

---

## 7. Dependence en \(\alpha(t)\), dynamique ATMF et \(R_T\)

Tu avais exige ce point explicitement, donc je le traite de maniere detaillee.

### 7.1. Derivee par rapport au strike

On vient de voir :

\[
\mathcal S_T
:=
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
=
\frac1T\int_0^T \frac{t}{T}\alpha(t)\,dt.
\]

### 7.2. Derivee par rapport au spot initial a strike fixe

Bergomi montre aussi qu'au voisinage ATMF :

\[
\boxed{
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln S_0}\right|_{K=F_T}
=
\frac1T\int_0^T \left(1-\frac{t}{T}\right)\alpha(t)\,dt.
}
\]

Interpretation :

- ici le poids est \(1-t/T\) ;
- donc les **temps courts** dominent ;
- cela correspond au fait qu'un choc de spot agit d'abord au debut de la trajectoire.

### 7.3. Dynamique de la volatilite ATMF

Comme le strike ATMF depend lui-meme du spot :

\[
K=F_T(S_0),
\]

on applique la regle de chaine :

\[
\frac{d\widehat{\sigma}_{F_T,T}}{d\ln S_0}
=
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln S_0}\right|_{K=F_T}
+
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}.
\]

En remplaçant les deux formules precedentes :

\[
\boxed{
\frac{d\widehat{\sigma}_{F_T,T}}{d\ln S_0}
=
\frac1T\int_0^T \alpha(t)\,dt.
}
\]

**Interpretation fondamentale :**

> le mouvement de la vol ATMF est entierement determine par la structure par terme de \(\alpha(t)\).

Cette fois, toutes les dates comptent avec le meme poids.

### 7.4. Definition de \(R_T\)

Le ratio de rigidite du skew est defini par :

\[
\boxed{
R_T
:=
\frac{
\dfrac{d\widehat{\sigma}_{F_T,T}}{d\ln S_0}
}{
\left.\dfrac{\partial \widehat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
}
=
\frac{\int_0^T \alpha(t)\,dt}
{\int_0^T \frac{t}{T}\alpha(t)\,dt}.
}
\]

### 7.5. Interpretation de \(R_T\)

\(R_T\) mesure :

> de combien la vol ATMF bouge quand le spot bouge, exprimee en unites de skew ATMF.

Donc :

- \(R_T\) grand = smile rigide, forte reaction de la vol ATMF ;
- \(R_T\) faible = smile plus "transporté" avec le spot.

### 7.6. Regimes de reference

#### Sticky-delta

Si la surface reste fixe en moneyness, alors la vol ATMF ne bouge pas :

\[
\frac{d\widehat{\sigma}_{F_T,T}}{d\ln S_0}=0,
\qquad
R_T=0.
\]

#### Sticky-strike

Si les vols a strike fixe ne bougent pas, alors :

\[
\frac{d\widehat{\sigma}_{F_T,T}}{d\ln S_0}
=
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T},
\qquad
R_T=1.
\]

#### Local vol avec skew local constant

Si \(\alpha(t)\equiv \alpha\), alors :

\[
\frac{d\widehat{\sigma}_{F_T,T}}{d\ln S_0}=\alpha,
\qquad
\mathcal S_T=\frac{\alpha}{2},
\qquad
R_T=2.
\]

C'est la **regle du \(R=2\)**.

### 7.7. Ecriture de \(R_T\) en fonction du skew ATMF \(\delta_T\)

On note

\[
\delta_T:=\mathcal S_T
=
\frac{1}{T^2}\int_0^T t\,\alpha(t)\,dt.
\]

Alors :

\[
T^2\delta_T=\int_0^T t\,\alpha(t)\,dt.
\]

En derivant :

\[
2T\delta_T+T^2\delta_T'=T\alpha(T),
\]

donc

\[
\alpha(T)=2\delta_T+T\delta_T'.
\]

En integrant :

\[
\int_0^T \alpha(t)\,dt
=
\int_0^T \big(2\delta_t+t\delta_t'\big)\,dt
=
\int_0^T \delta_t\,dt+T\delta_T.
\]

Ainsi :

\[
\frac{d\widehat{\sigma}_{F_T,T}}{d\ln S_0}
=
\delta_T+\frac1T\int_0^T \delta_t\,dt.
\]

Et donc :

\[
\boxed{
R_T=
1+\frac1T\int_0^T \frac{\delta_t}{\delta_T}\,dt.
}
\]

Conséquences :

- si \(\delta_t\equiv \delta\), alors \(R_T=2\) ;
- si \(\delta_t\) est continue pres de 0, alors \(R_T\to 2\) quand \(T\to 0\).

---

## 8. Version generale : \(\overline{\sigma}(t)\) non constante

Bergomi donne aussi une version plus generale, lorsque le niveau moyen de la local vol depend du temps.

On obtient alors des poids modifies :

\[
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
=
\frac1T\int_0^T
\frac{\widehat{\sigma}_t^{\,2}t}{\widehat{\sigma}_T^{\,2}T}
\frac{\overline{\sigma}(t)}{\widehat{\sigma}_T}
\alpha(t)\,dt,
\]

\[
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln S_0}\right|_{K=F_T}
=
\frac1T\int_0^T
\left(
1-\frac{\widehat{\sigma}_t^{\,2}t}{\widehat{\sigma}_T^{\,2}T}
\right)
\frac{\overline{\sigma}(t)}{\widehat{\sigma}_T}
\alpha(t)\,dt,
\]

\[
\frac{d\widehat{\sigma}_{F_T,T}}{d\ln S_0}
=
\frac1T\int_0^T
\frac{\overline{\sigma}(t)}{\widehat{\sigma}_T}\alpha(t)\,dt.
\]

Interpretation :

- les poids ne sont plus simplement \(t/T\) ou \(1-t/T\) ;
- ils sont deformés par la structure par terme du niveau de vol.

Mais l'idee fondamentale reste la meme :

> c'est toujours \(\alpha(t)\) qui pilote la dynamique du smile.

---

## 9. Section 2.4.6 -- Resultat exact a courte maturite

### 9.1. Point de depart : la formule de Dupire

En coordonnees \((T,y)\), avec

\[
y=\ln\left(\frac{K}{F_T}\right),
\]

Dupire relie la local vol a la surface implicite.

Quand \(T\to 0\), en ne gardant que les termes dominants, Bergomi obtient :

\[
\sigma^2(0,S_0 e^y)
=
\frac{\widehat{\sigma}^2(0,y)}
{\left(y\,\widehat{\sigma}(0,y)\,\partial_y\widehat{\sigma}(0,y)-1\right)^2}.
\]

En prenant la racine :

\[
\frac{1}{\sigma(0,S_0 e^y)}
=
\pm\left(\frac{y}{\widehat{\sigma}(0,y)}\right)'.
\]

On choisit le signe compatible avec des volatilites positives et on integre entre \(0\) et \(y\) :

\[
\int_0^y \frac{du}{\sigma(0,S_0 e^u)}
=
\frac{y}{\widehat{\sigma}(0,y)}.
\]

Donc :

\[
\boxed{
\frac{1}{\widehat{\sigma}(0,y)}
=
\frac{1}{y}\int_0^y \frac{du}{\sigma(0,S_0 e^u)}.
}
\]

En revenant a \(K=S_0e^y\) :

\[
\boxed{
\frac{1}{\widehat{\sigma}(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm{loc}}(0,S)}.
}
\]

### 9.2. Pourquoi c'est une moyenne harmonique

La formule porte sur :

\[
\frac{1}{\widehat{\sigma}},
\]

qui est la moyenne de

\[
\frac{1}{\sigma_{\mathrm{loc}}}.
\]

Donc la bonne moyenne est **harmonique**, et non arithmetique.

### 9.3. Pourquoi ce resultat est surprenant

On aurait pu s'attendre a voir apparaitre une moyenne de \(\sigma\) ou de \(\sigma^2\).

Mais ici :

- on est a tres courte maturite ;
- il n'y a presque plus de moyenne temporelle ;
- la bonne structure devient **spatiale**, pas temporelle.

### 9.4. Intuition financiere

Si la local vol s'annule sur une region entre \(S_0\) et \(K\), alors a tres courte maturite le spot ne peut pratiquement pas la traverser.

Donc l'implicite doit aussi tendre vers zero.

La moyenne harmonique respecte exactement cette propriete.

### 9.5. Cas ATM

Quand \(K\to S_0\), la formule redonne :

\[
\widehat{\sigma}(0,S_0)=\sigma_{\mathrm{loc}}(0,S_0).
\]

Ce qui est coherent : a maturite instantanee et a la monnaie, l'implicite lit directement la vol locale au point initial.

---

## 10. Lien avec la formule de Dupire

### 10.1. Dupire direct

La formule de Dupire s'ecrit :

\[
\sigma_{\mathrm{loc}}^2(T,K)
=
\frac{\partial_T C(T,K)+(r-q)K\partial_K C(T,K)+qC(T,K)}
{\frac12 K^2 \partial_{KK}C(T,K)}.
\]

Elle donne :

\[
\widehat{\sigma}(K,T)\longrightarrow \sigma_{\mathrm{loc}}(t,S).
\]

### 10.2. Bergomi 2.4 fait le chemin inverse

Le chapitre 2.4 dit :

\[
\sigma_{\mathrm{loc}}(t,S)\longrightarrow \widehat{\sigma}(K,T),
\]

mais :

- de facon exacte et implicite pour la variance ;
- de facon approchee et explicite a l'ordre 1 ;
- de facon asymptotique exacte a courte maturite.

### 10.3. Point conceptuel important

Dupire montre qu'on peut **calibrer parfaitement** les vanilles a \(t=0\).

Bergomi montre ensuite que :

> cela ne suffit pas du tout a garantir une bonne dynamique future du smile.

Autrement dit :

> calibrer n'est pas dynamiser correctement.

---

## 11. Lien avec les modeles de volatilite stochastique

### 11.1. Difference generale

Dans un modele local vol :

- la volatilite future est une fonction deterministe de \((t,S_t)\) ;
- donc, une fois le spot connu, la dynamique future du smile est tres contrainte.

Dans un modele de volatilite stochastique :

- il y a des facteurs aleatoires supplementaires ;
- donc, a spot donne, la variance future peut encore bouger.

### 11.2. Conséquence sur la dynamique du smile

Le modele local vol produit souvent :

- un smile trop rigide ;
- une reaction de la vol ATMF trop forte ;
- un comportement souvent proche de \(R_T=2\) a courte maturite.

En pratique equity, les modeles stochastiques donnent souvent des dynamics plus proches de :

- sticky-delta ;
- ou d'un regime intermediaire entre sticky-delta et sticky-strike.

### 11.3. Interpretation financiere

Le marche actions observe souvent :

- que le smile se deforme ;
- mais pas uniquement a cause du spot ;
- et pas aussi rigidement que dans local vol.

Donc local vol peut bien calibrer les vanilles,
mais mal decrire :

- la dynamique future ;
- la couverture ;
- les exotiques.

---

## 12. Warnings, approximations et limites

### 12.1. La formule weak local vol est une approximation d'ordre 1

Elle est utile pour :

- comprendre les dependances qualitatives ;
- extraire le role de \(\alpha(t)\), \(\beta(t)\), \(R_T\) ;
- faire des asymptotiques pres du forward.

Mais elle n'est pas suffisante pour des niveaux absolus de smile de precision marchée.

### 12.2. Le developpement pres du forward est local

Les formules en \(\alpha(t)\) et \(\beta(t)\) decrivent le smile **pres du forward**.

Elles ne pretendent pas capturer tout le smile lointain.

### 12.3. Ne pas confondre exact et approche

Il faut distinguer :

- **exact** :
  - formule de moyenne ponderee pour \(\widehat{\sigma}^2\),
  - resultat harmonique a courte maturite ;
- **approche** :
  - formules weak local vol ;
  - developpements du skew et de la courbure.

### 12.4. Limites structurelles du modele local vol

Le modele local vol :

- calibre exactement les vanilles ;
- mais impose une dynamique tres contrainte du smile ;
- donc peut etre peu realiste pour la couverture dynamique.

En particulier :

- \(R_T\) trop eleve ;
- smile trop rigide ;
- mauvaise reproduction de certains mouvements de surface.

---

## 13. Ce qu'il faut retenir absolument

### 13.1. Message mathematique

1. La variance implicite est une moyenne ponderee de la variance locale.
2. A l'ordre 1, l'implicite peut etre vue comme une moyenne de la local vol.
3. Le skew implicite pres du forward est une moyenne ponderee de \(\alpha(t)\).
4. La dynamique de la vol ATMF depend de la moyenne uniforme de \(\alpha(t)\).
5. Le ratio \(R_T\) mesure la rigidite du smile.
6. A courte maturite, l'inverse de l'implicite est une moyenne harmonique de l'inverse de la local vol.

### 13.2. Message financier

Le modele local vol ne se contente pas de calibrer le smile :

> il impose une dynamique particuliere du smile.

Et cette dynamique est souvent trop rigide par rapport au marche.

### 13.3. Formules a savoir absolument

\[
\widehat{\sigma}_{K,T}^{\,2}
=
\frac{
\mathbb E^{\mathrm{loc}}\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}\sigma_{\mathrm{loc}}^2(t,S_t)\,dt
\right]
}{
\mathbb E^{\mathrm{loc}}\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}\,dt
\right]
}
\]

\[
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
=
\frac1T\int_0^T \frac{t}{T}\alpha(t)\,dt
\]

\[
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln S_0}\right|_{K=F_T}
=
\frac1T\int_0^T \left(1-\frac{t}{T}\right)\alpha(t)\,dt
\]

\[
\frac{d\widehat{\sigma}_{F_T,T}}{d\ln S_0}
=
\frac1T\int_0^T \alpha(t)\,dt
\]

\[
R_T=
\frac{d\widehat{\sigma}_{F_T,T}/d\ln S_0}
{\left.\partial_{\ln K}\widehat{\sigma}_{K,T}\right|_{K=F_T}}
\]

\[
\frac{1}{\widehat{\sigma}(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm{loc}}(0,S)}.
\]

---

## 14. Phrase de conclusion pour l'oral

Le point cle du chapitre 2.4 est le suivant :

> sous un modele de volatilite locale, le smile implicite futur n'est pas libre ; il est entierement contraint par la structure spatiale et temporelle de \(\sigma_{\mathrm{loc}}(t,S)\), en particulier via le skew local instantane \(\alpha(t)\).
