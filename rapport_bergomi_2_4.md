# Bergomi, Chapitre 2.4 -- Des volatilites locales aux volatilites implicites

## Ce rapport est re-ecrit pour quelqu'un qui part de zero

L'objectif de cette version est different du precedent rapport :

- je ne suppose **pas** que tu comprennes deja ce qu'est un P\&L de couverture ;
- je ne suppose **pas** que les mots "gamma/theta", "modele de base", "modele reel", "moyenne ponderee" soient clairs ;
- je vais donc expliquer **d'abord l'idee**, puis seulement **ensuite** les calculs.

Le but est que tu puisses repondre a des questions du type :

- "Mais qu'est-ce qu'on est en train de calculer, au juste ?"
- "Pourquoi le gamma apparait ?"
- "C'est quoi le P\&L ici ?"
- "Qu'est-ce que veut dire couvrir delta ?"
- "Pourquoi on compare deux modeles ?"
- "Pourquoi ca donne une moyenne ponderee ?"

---

## 0. La question de fond du chapitre

Le chapitre 2.4 de Bergomi part d'une question tres simple en apparence :

> Si je connais une fonction de volatilite locale \(\sigma_{\mathrm{loc}}(t,S)\), quelle volatilite implicite \(\widehat{\sigma}(K,T)\) cette fonction engendre-t-elle ?

Autrement dit :

- **probleme de Dupire** : partir du smile implicite et retrouver la local vol ;
- **probleme de Bergomi ici** : partir de la local vol et comprendre le smile implicite qu'elle produit.

Mais Bergomi veut plus qu'une simple correspondance statique.

Il veut comprendre :

1. quel niveau d'implicite est engendre ;
2. comment le smile pres du forward se construit ;
3. pourquoi, dans un modele local vol, la dynamique du smile est tres contrainte.

---

## 1. Rappels de cours indispensables avant Bergomi

### 1.1. Mesure risque-neutre

Sous la mesure risque-neutre, dans un modele de volatilite locale, on ecrit :

\[
dS_t=(r-q)S_t\,dt+\sigma_{\mathrm{loc}}(t,S_t)S_t\,dW_t.
\]

Ici :

- \(S_t\) est le prix du sous-jacent ;
- \(r\) est le taux sans risque ;
- \(q\) est le taux de dividende / carry ;
- \(W_t\) est un brownien sous la mesure de pricing ;
- \(\sigma_{\mathrm{loc}}(t,S)\) est une fonction **deterministe** de \(t\) et \(S\).

### 1.2. Prix d'une option

Si \(P(t,S)\) designe le prix d'une option europeenne, alors sous des hypotheses standard de regularite, \(P\) satisfait l'EDP :

\[
\partial_t P+(r-q)S\partial_S P+\frac12 \sigma^2(t,S)S^2\partial_{SS}P-rP=0.
\]

Cette equation vient du cours via :

- Itô ;
- absence d'arbitrage ;
- Feynman-Kac.

### 1.3. Delta, gamma, theta : que signifient ces mots ?

Pour une option de prix \(P(t,S)\) :

- **delta** :
  \[
  \Delta=\partial_S P ;
  \]
- **gamma** :
  \[
  \Gamma=\partial_{SS}P ;
  \]
- **theta** :
  \[
  \Theta=\partial_t P .
  \]

Interpretation :

- le **delta** mesure la sensibilite lineaire au spot ;
- le **gamma** mesure la courbure, donc la sensibilite du delta lui-meme ;
- le **theta** mesure l'effet du passage du temps.

### 1.4. Qu'est-ce qu'un P\&L ?

Le **P\&L** = "Profit and Loss" = gain/perte d'une position sur un petit intervalle de temps.

Si je suis short une option et que je me couvre en delta, alors sur un petit temps \(dt\), le P\&L residuel est ce qui reste **apres** avoir neutralise le terme lineaire en \(dS_t\).

Le point important du cours est le suivant :

- si la couverture delta est parfaite dans **le bon modele**, le P\&L residuel est celui impose par l'EDP ;
- si je couvre avec **un mauvais modele de volatilite**, il reste un ecart de P\&L ;
- cet ecart fait apparaitre naturellement le **gamma**.

### 1.5. Que veut dire "couvrir delta" ?

Supposons que je detienne une position de prix \(P(t,S_t)\).

Sur un petit intervalle de temps, par Itô :

\[
dP \approx \partial_t P\,dt+\partial_S P\,dS+\frac12 \partial_{SS}P\,d\langle S\rangle.
\]

Si je prends en face une position de \(-\partial_S P\) actions, le terme en \(dS\) disparait.

Il reste donc essentiellement :

\[
dP-\partial_S P\,dS
\approx
\partial_t P\,dt+\frac12 \partial_{SS}P\,d\langle S\rangle.
\]

Voila pourquoi, dans une couverture delta, ce qui reste est un terme de type :

- theta ;
- plus gamma fois variation quadratique.

C'est exactement le coeur de l'idee de Bergomi.

---

## 2. Ideee intuitive de Bergomi avant les formules

Bergomi compare **deux modeles**.

### 2.1. Pourquoi deux modeles ?

Parce qu'il veut mesurer le prix de l'erreur qu'on fait si :

- on price/couvre l'option avec un **modele de base** ;
- mais le sous-jacent evolue en realite selon un **autre modele**.

Donc :

- **modele I** = modele de reference, celui avec lequel on calcule le prix et les greeks ;
- **modele II** = dynamique "reelle" suivie par le sous-jacent.

Le grand message est :

> la difference entre les deux prix est egale au P\&L cumule de la couverture delta faite avec le modele I alors que le sous-jacent suit le modele II.

Cela peut sembler abstrait, mais c'est une idee de cours tres classique :

- on evalue une strategie de couverture ;
- on regarde combien elle gagne/perd si le monde reel ne suit pas le modele utilise.

### 2.2. Pourquoi cela interesse l'implicite ?

Parce qu'on va choisir le modele I de facon tres intelligente :

- on le prend egal a Black-Scholes,
- avec une volatilite constante choisie egale a **la volatilite implicite de l'option**.

Dans ce cas :

- le prix dans Black-Scholes est, par definition, egal au prix de l'option ;
- donc la difference de prix entre modele I et modele II vaut zero ;
- et cela force une relation entre :
  - la volatilite implicite,
  - et la volatilite instantanee du modele II.

Si le modele II est ensuite le modele local vol, on obtient la formule centrale du chapitre.

---

## 3. Cadre mathematique precise

On note :

- \(P_1(t,S)\) : prix de l'option dans le **modele I** ;
- \(P_2(t,S)\) : prix de l'option dans le **modele II** ;
- \(f(S_T)\) : payoff europeen final.

Dans le modele II, le sous-jacent suit :

\[
dS_t=(r-q)S_t\,dt+\sigma_{2,t}S_t\,dW_t.
\]

Pour le moment, \(\sigma_{2,t}\) est un processus quelconque.

On introduit la quantite actualisee :

\[
Q_t=e^{-rt}P_1(t,S_t).
\]

Pourquoi cette quantite ?

Parce que :

- en finance de prix, les quantites actualisees sont les bonnes quantites a etudier ;
- elles permettent de faire apparaitre proprement le drift residuel ;
- si le modele etait exactement bon, cette quantite se comporterait comme une martingale.

---

## 4. Derivation complete de la formule de Bergomi

### 4.1. Application de la formule d'Itô a \(Q_t\)

On applique Itô a \(P_1(t,S_t)\), puis on actualise :

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

Or, dans le modele II :

\[
d\langle S\rangle_t=\sigma_{2,t}^2 S_t^2\,dt.
\]

Donc :

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

### 4.2. Esperance conditionnelle

En prenant l'esperance conditionnelle, le terme brownien disparait.

On obtient :

\[
\mathbb{E}_2[dQ_t\mid \mathcal F_t]
=
e^{-rt}
\left[
-rP_1+\partial_t P_1+(r-q)S_t\partial_S P_1+\frac12 \sigma_{2,t}^2 S_t^2\partial_{SS}P_1
\right]dt.
\]

### 4.3. Utilisation de l'EDP satisfaite par \(P_1\)

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

En remplacant :

\[
\mathbb{E}_2[dQ_t\mid \mathcal F_t]
=
e^{-rt}\frac12 S_t^2\partial_{SS}P_1(t,S_t)\big(\sigma_{2,t}^2-\sigma_1^2(t,S_t)\big)\,dt.
\]

### 4.4. Interpretation immediate : c'est le P\&L gamma/theta

Cette formule est fondamentale.

Elle dit que le drift residuel de la couverture delta est proportionnel a :

\[
\frac12 e^{-rt}S_t^2\Gamma_t\big(\sigma_{2,t}^2-\sigma_1^2(t,S_t)\big).
\]

Interpretation tres simple :

- si la variance instantanee du vrai monde est la meme que celle du modele de couverture, alors ce terme vaut zero ;
- sinon, il reste un ecart ;
- cet ecart est pondere par le gamma.

Autrement dit :

> plus l'option est convexe, plus une erreur sur la variance instantanee coute cher en P\&L.

### 4.5. Integration jusqu'a maturite

On integre entre \(0\) et \(T\) :

\[
\mathbb{E}_2[Q_T]
=
Q_0+
\mathbb{E}_2\left[
\int_0^T e^{-rt}\frac12 S_t^2\partial_{SS}P_1(t,S_t)\big(\sigma_{2,t}^2-\sigma_1^2(t,S_t)\big)\,dt
\right].
\]

Mais

\[
Q_T=e^{-rT}f(S_T),
\]

donc

\[
\mathbb{E}_2[Q_T]=P_2(0,S_0,\cdot).
\]

Et

\[
Q_0=P_1(0,S_0).
\]

Ainsi :

\[
\boxed{
P_2(0,S_0,\cdot)
=
P_1(0,S_0)+
\mathbb{E}_2\left[
\int_0^T e^{-rt}\frac12 S_t^2\partial_{SS}P_1(t,S_t)\big(\sigma_{2,t}^2-\sigma_1^2(t,S_t)\big)\,dt
\right].
}
\]

### 4.6. Phrase simple a retenir

Cette formule signifie :

> Le prix dans le modele II = le prix dans le modele I + le P\&L moyen de la couverture delta calculee avec le modele I alors que le monde suit le modele II.

Cette phrase est probablement la plus importante de toute la section 2.4.1.

---

## 5. Comment on obtient la "moyenne ponderee"

### 5.1. Choix malin du modele I

On prend maintenant le modele I comme etant Black-Scholes avec une volatilite constante egale a l'implicite de l'option :

\[
\sigma_1(t,S)\equiv \widehat{\sigma}_{K,T}.
\]

Par definition meme de l'implicite :

\[
P_1(0,S_0)=P_2(0,S_0,\cdot).
\]

Donc le membre de gauche moins le premier terme du membre de droite vaut zero.

Il reste :

\[
0=
\mathbb{E}_2\left[
\int_0^T e^{-rt}\frac12 S_t^2\Gamma_t^{BS}(\widehat{\sigma}_{K,T})
\big(\sigma_{2,t}^2-\widehat{\sigma}_{K,T}^2\big)\,dt
\right].
\]

En depliant :

\[
\widehat{\sigma}_{K,T}^2
\mathbb{E}_2\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}(\widehat{\sigma}_{K,T})\,dt
\right]
=
\mathbb{E}_2\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}(\widehat{\sigma}_{K,T})\,\sigma_{2,t}^2\,dt
\right].
\]

Donc :

\[
\boxed{
\widehat{\sigma}_{K,T}^{\,2}
=
\frac{
\mathbb{E}_2\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}(\widehat{\sigma}_{K,T})\,\sigma_{2,t}^2\,dt
\right]
}{
\mathbb{E}_2\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}(\widehat{\sigma}_{K,T})\,dt
\right]
}.
}
\]

### 5.2. Pourquoi c'est une moyenne ponderee ?

Cette formule a exactement la forme :

\[
\frac{\mathbb E[\int w_t X_t\,dt]}{\mathbb E[\int w_t\,dt]},
\]

avec :

- \(X_t=\sigma_{2,t}^2\),
- \(w_t=e^{-rt}S_t^2\Gamma_t^{BS}(\widehat{\sigma}_{K,T})\).

Donc \(\widehat{\sigma}_{K,T}^2\) est une **moyenne ponderee** de \(\sigma_{2,t}^2\).

### 5.3. Pourquoi le poids est-il un dollar gamma ?

Parce que :

- le facteur \(S_t^2\Gamma_t\) apparait naturellement dans le terme de second ordre d'Itô ;
- economiquement, c'est dans les zones ou l'option a beaucoup de convexite que l'erreur de volatilite coute le plus ;
- donc ces zones doivent peser davantage dans la moyenne.

### 5.4. Specialisation au modele local vol

Si le modele II est le modele local vol, alors :

\[
\sigma_{2,t}=\sigma_{\mathrm{loc}}(t,S_t),
\]

et on obtient :

\[
\boxed{
\widehat{\sigma}_{K,T}^{\,2}
=
\frac{
\mathbb{E}^{\mathrm{loc}}\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}(\widehat{\sigma}_{K,T})\,\sigma_{\mathrm{loc}}^2(t,S_t)\,dt
\right]
}{
\mathbb{E}^{\mathrm{loc}}\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}(\widehat{\sigma}_{K,T})\,dt
\right]
}.
}
\]

### 5.5. Point de comprehension crucial

**Exactement**, cette formule porte sur :

\[
\widehat{\sigma}_{K,T}^{\,2},
\]

pas sur \(\widehat{\sigma}_{K,T}\) lui-meme.

Donc :

- formule exacte = moyenne de **variance locale** ;
- formule approchee d'ordre 1 = moyenne de **volatilite locale**.

Si tu confonds ces deux niveaux, tu perds le coeur de la section.

---

## 6. Pourquoi l'identite exacte ne suffit pas encore

La formule exacte est tres belle, mais elle est **implicite**.

Pourquoi ?

Parce que \(\widehat{\sigma}_{K,T}\) apparait des deux cotes :

- a gauche directement ;
- a droite dans le gamma Black-Scholes ;
- et aussi dans la loi induite, puisque les poids dependent de l'option consideree.

Donc on ne peut pas "lire" directement \(\widehat{\sigma}_{K,T}\).

C'est pour cela que Bergomi passe a une approximation perturbative.

---

## 7. Approximation "weakly local vol" : idee avant calcul

### 7.1. Idee simple

On suppose que la variance locale est proche d'une variance de reference deterministe :

\[
u(t,S)=\sigma_{\mathrm{loc}}^2(t,S)=u_0(t)+\delta u(t,S),
\]

ou \(\delta u\) est petit.

Le sens de "weakly local" est donc :

> la dependance en spot n'est pas trop forte ; on peut traiter cette dependance comme une perturbation.

### 7.2. Pourquoi faire cela ?

Parce que dans ce cas :

- la loi de reference est connue explicitement ;
- la gamma de reference est connue explicitement ;
- on peut developper le rapport au premier ordre.

### 7.3. Pourquoi la perturbation de la densite ne contribue pas a l'ordre 1 ?

C'est souvent un point qui perturbe.

Ecrivons symboliquement :

\[
\widehat{\sigma}^2=\frac{\mathbb E[(u_0+\delta u)\bullet]}{\mathbb E[\bullet]}.
\]

En developpant un quotient :

\[
\frac{A_0+\delta A}{B_0+\delta B}
=
\frac{A_0}{B_0}+\frac{\delta A\,B_0-A_0\,\delta B}{B_0^2}+O(\delta^2).
\]

Comme au point de base \(A_0=u_0 B_0\), la partie provenant de la perturbation de la loi se simplifie.

Il reste seulement la contribution explicite de \(\delta u\).

Le resultat est :

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

---

## 8. Formule approchee exploitable

En effectuant les calculs explicites sous Black-Scholes de reference, Bergomi obtient :

\[
\boxed{
\widehat{\sigma}_{K,T}^{\,2}
=
\frac{1}{T}\int_0^T dt \int_{\mathbb R}\phi(y)\,
u\!\left(
t,\,
F_t\exp\!\left(
\frac{\omega_t}{\omega_T}x_K+\frac{\sqrt{(\omega_T-\omega_t)\omega_t}}{\sqrt{\omega_T}}\,y
\right)
\right),
}
\]

ou :

- \(u=\sigma_{\mathrm{loc}}^2\),
- \(x_K=\ln(K/F_T)\),
- \(\omega_t=\int_0^t \sigma_0^2(s)\,ds\),
- \(\phi(y)=\dfrac{e^{-y^2/2}}{\sqrt{2\pi}}\).

### Interpretation

Cette formule dit :

> la variance implicite approchee est une moyenne spatiale-temporelle de la variance locale, le long de trajectoires intermediaires reliees au point initial et au strike final.

---

## 9. Cas plus simple : developpement autour d'une vol constante

Si \(\sigma_0(t)\equiv \sigma_0\) est constante, alors :

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
\right).
}
\]

### Que signifie cette formule ?

Elle est tres importante intuitivement :

- on moyenne la local vol sur des points \((t,S)\) intermediaires ;
- ces points joignent \(S_0\) au strike final \(K\) ;
- \(y=0\) correspond au chemin central, ou "chemin le plus probable".

Ce n'est pas encore parfait numeriquement, mais c'est extremement parlant pour comprendre la construction du smile.

---

## 10. Smile pres du forward

Pres du forward, Bergomi pose :

\[
\sigma_{\mathrm{loc}}(t,S)=\overline{\sigma}(t)+\alpha(t)x+\frac{\beta(t)}{2}x^2,
\qquad
x=\ln(S/F_t).
\]

Interpretation :

- \(\overline{\sigma}(t)\) = niveau instantane de vol locale ;
- \(\alpha(t)\) = pente locale instantanee du smile local ;
- \(\beta(t)\) = courbure locale instantanee.

En reinjectant cette expansion dans la formule approchee, on obtient :

\[
\widehat{\sigma}_{K,T}
\approx
\frac{1}{T}\int_0^T \overline{\sigma}(t)\,dt
+
\left(\frac{1}{T}\int_0^T \frac{t}{T}\alpha(t)\,dt\right)x_K
+
\frac12\left(\frac{1}{T}\int_0^T \left(\frac{t}{T}\right)^2\beta(t)\,dt\right)x_K^2
\]

a un terme de niveau pres.

Donc :

\[
\boxed{
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
=
\frac{1}{T}\int_0^T \frac{t}{T}\alpha(t)\,dt
}
\]

et

\[
\boxed{
\left.\frac{\partial^2 \widehat{\sigma}_{K,T}}{\partial (\ln K)^2}\right|_{K=F_T}
=
\frac{1}{T}\int_0^T \left(\frac{t}{T}\right)^2\beta(t)\,dt.
}
\]

### Idee centrale

Le skew implicite pres du forward est une **moyenne ponderee** du skew local \(\alpha(t)\).

Pourquoi le poids \(t/T\) ?

Parce que plus on est proche de l'echeance, plus la contrainte terminale \(S_T\approx K\) est forte.

Cas particulier :

si \(\alpha(t)\equiv \alpha\), alors

\[
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
=\frac{\alpha}{2}.
\]

Donc le skew implicite ATMF vaut la moitie du skew local constant.

---

## 11. Loi de puissance pour le skew

Supposons

\[
\alpha(t)=
\begin{cases}
\alpha_0, & t\le \tau_0,\\
\alpha_0(\tau_0/t)^\gamma, & t>\tau_0.
\end{cases}
\]

Alors a grande maturite :

\[
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
\sim
\frac{1}{2-\gamma}\alpha_0\left(\frac{\tau_0}{T}\right)^\gamma.
\]

Donc :

- le skew implicite decroit avec le meme exposant \(\gamma\) que le skew local ;
- \(\alpha(t)\) controle directement la structure par terme du skew implicite.

---

## 12. Resultat exact de courte maturite

Ici, Bergomi ne fait plus une approximation perturbative.

Quand \(T\to 0\), il obtient un resultat exact :

\[
\boxed{
\frac{1}{\widehat{\sigma}(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm{loc}}(0,S)}.
}
\]

### Pourquoi ce resultat surprend ?

Parce qu'on pourrait s'attendre a voir apparaitre :

- une moyenne de \(\sigma\),
- ou une moyenne de \(\sigma^2\).

Mais non.

Ici, c'est une **moyenne harmonique** de \(1/\sigma_{\mathrm{loc}}\).

### Intuition

A tres courte maturite :

- il n'y a presque plus de moyenne temporelle ;
- ce qui compte, c'est la geometrie spatiale entre \(S_0\) et \(K\).

Si la vol locale s'annule sur une region a franchir, alors le spot ne peut pratiquement pas traverser cette region instantanement ; il est donc logique que l'implicite associee tende aussi vers zero.

La moyenne harmonique respecte exactement cette intuition.

---

## 13. Lien avec Dupire

### 13.1. Sens direct de Dupire

Dupire dit :

\[
\sigma_{\mathrm{loc}}^2(T,K)
=
\frac{\partial_T C(T,K)+(r-q)K\partial_K C(T,K)+qC(T,K)}
{\frac12 K^2\partial_{KK}C(T,K)}.
\]

Donc, connaissant la surface des calls, on reconstruit la local vol.

### 13.2. Sens inverse dans Bergomi

Le chapitre 2.4 etudie l'autre sens :

- la local vol est supposee donnee ;
- on cherche la surface implicite qu'elle engendre.

### 13.3. Message conceptuel

Dupire repond a une question de **calibration statique**.

Bergomi pose ensuite la vraie question de **dynamique** :

> une fois ce modele local vol calibre aujourd'hui, comment la surface implicite bougera-t-elle demain ?

---

## 14. Limites du modele local vol et de l'approximation

### 14.1. Limite de l'ordre 1

Les formules approchees sont excellentes pour :

- l'intuition ;
- le skew ;
- la courbure ;
- les asymptotiques.

Mais elles ne sont pas assez precises pour des niveaux absolus de smile equity realistes.

### 14.2. Limite du modele local vol lui-meme

Un modele local vol peut calibrer exactement les vanilles a \(t=0\), mais produire une mauvaise dynamique du smile.

En pratique :

- le smile local vol est souvent trop rigide ;
- les modeles de volatilite stochastique reproduisent mieux la dynamique observee.

---

## 15. Ce qu'il faut retenir absolument

1. Le point de depart de Bergomi est un **argument de couverture delta**.
2. Le **P\&L residuel** est gouverne par un terme de type gamma/theta.
3. Cela donne une formule exacte ou la **variance implicite** est une **moyenne ponderee de variances locales**.
4. En regime **weakly local vol**, on obtient une formule exploitable pour l'implicite elle-meme.
5. Pres du forward, le smile implicite est controle par \(\alpha(t)\) et \(\beta(t)\).
6. A courte maturite, on obtient un resultat exact en **moyenne harmonique**.
7. Le sens profond du chapitre est : **calibrer une surface n'est pas comprendre sa dynamique**.

---

## 16. En une phrase simple pour toi

Si tu veux une version ultra simple :

> Bergomi montre que, dans un modele local vol, la volatilite implicite n'est pas une quantite mysterieuse : elle s'obtient en moyennant la volatilite locale de facon tres precise, avec des poids qui viennent du gamma de l'option ; ensuite, pres du forward, tout le smile est pilote par la pente locale \(\alpha(t)\).

### 3.1. Decomposition de la variance locale

On pose :

\[
u(t,S)=\sigma_{\mathrm{loc}}^2(t,S)=u_0(t)+\delta u(t,S),
\]

ou :

- \(u_0(t)=\sigma_0^2(t)\) est une variance **deterministe en temps** ;
- \(\delta u(t,S)\) est une petite perturbation.

Si \(\delta u=0\), alors l'implicite correspondante est simplement la volatilite moyenne quadratique :

\[
\widehat{\sigma}_{0;t_1,t_2}^{\,2}
=
\frac{1}{t_2-t_1}\int_{t_1}^{t_2}\sigma_0^2(t)\,dt.
\]

En particulier,

\[
\widehat{\sigma}_{0;0,T}^{\,2}
=
\frac{1}{T}\int_0^T \sigma_0^2(t)\,dt.
\]

### 3.2. Pourquoi la perturbation de la densite disparait a l'ordre 1

Dans la formule exacte, \(\delta u\) intervient :

1. explicitement dans le numerateur ;
2. implicitement, car la loi de \(S_t\) depend elle aussi de \(u\).

Notons schematiquement :

\[
\frac{\mathbb{E}_{u_0+\delta u}[(u_0+\delta u)\bullet]}
{\mathbb{E}_{u_0+\delta u}[\bullet]}.
\]

Au premier ordre :

\[
\frac{A_0+\delta A}{B_0+\delta B}
=
\frac{A_0}{B_0}
+
\frac{\delta A\,B_0-A_0\,\delta B}{B_0^2}
+
O(\delta^2).
\]

Mais au point de base, on a \(A_0=u_0 B_0\). Donc :

\[
\frac{\delta A\,B_0-A_0\,\delta B}{B_0^2}
=
\frac{\delta A-u_0\delta B}{B_0}.
\]

Or la partie de \(\delta A\) due a la perturbation de la loi est exactement \(u_0\delta B\), donc elle s'annule. Il reste seulement la contribution **explicite** de \(\delta u\).

Conclusion :

\[
\delta\!\left(\widehat{\sigma}_{K,T}^{\,2}\right)
=
\frac{
\mathbb{E}_{\sigma_0}\left[
\int_0^T e^{-rt}\delta u(t,S_t)S_t^2\Gamma_t^{(0)}\,dt
\right]
}{
\mathbb{E}_{\sigma_0}\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{(0)}\,dt
\right]
},
\]

ou \(\Gamma_t^{(0)}\) est la gamma dans le modele Black-Scholes de variance deterministe \(u_0(t)\).

### 3.3. Calcul explicite du denominateur

Sous Black-Scholes a volatilite deterministe, on sait que

\[
e^{-rt}S_t^2\Gamma_t^{(0)}
\]

est une martingale. Donc

\[
\mathbb{E}_{\sigma_0}\left[e^{-rt}S_t^2\Gamma_t^{(0)}\right]
=
S_0^2\Gamma_0^{(0)}.
\]

Par integration en temps :

\[
\mathbb{E}_{\sigma_0}\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{(0)}\,dt
\right]
=
T S_0^2 \Gamma_0^{(0)}.
\]

### 3.4. Calcul du numerateur

Posons

\[
\omega_t=\int_0^t \sigma_0^2(\tau)\,d\tau.
\]

Sous le modele de base, \(X_t=\ln(S_t/F_t)\) suit une loi normale :

\[
X_t\sim \mathcal{N}\!\left(-\frac{\omega_t}{2},\,\omega_t\right).
\]

En ecrivant le numerateur comme une integrale sur \(S\), puis en passant a la variable

\[
x=\ln(S/F_t),
\]

le produit :

- de la densite lognormale de \(S_t\),
- et de la gamma Black-Scholes de l'option \((K,T)\),

donne un produit de deux gaussiennes en \(x\). En regroupant les termes quadratiques, on obtient une nouvelle gaussienne de :

- moyenne

\[
\mu_t=\frac{\omega_t}{\omega_T}x_K,
\]

- variance

\[
\nu_t=\frac{\omega_t(\omega_T-\omega_t)}{\omega_T}.
\]

Apres standardisation :

\[
x=\frac{\omega_t}{\omega_T}x_K+\sqrt{\frac{\omega_t(\omega_T-\omega_t)}{\omega_T}}\,y,
\qquad y\sim \mathcal{N}(0,1),
\]

on obtient la formule de Bergomi :

\[
\boxed{
\delta\!\left(\widehat{\sigma}_{K,T}^{\,2}\right)
=
\frac{1}{T}\int_0^T dt\int_{\mathbb{R}}\phi(y)\,
\delta u\!\left(
t,\,
F_t\exp\!\left(
\frac{\omega_t}{\omega_T}x_K+
\sqrt{\frac{\omega_t(\omega_T-\omega_t)}{\omega_T}}\,y
\right)
\right)dy
}
\]

avec

\[
\phi(y)=\frac{e^{-y^2/2}}{\sqrt{2\pi}}.
\]

Comme

\[
\widehat{\sigma}_{K,T}^{\,2}
=
\frac{1}{T}\int_0^T u_0(t)\,dt
+
\delta\!\left(\widehat{\sigma}_{K,T}^{\,2}\right),
\]

on peut reecrire :

\[
\boxed{
\widehat{\sigma}_{K,T}^{\,2}
=
\frac{1}{T}\int_0^T dt\int_{\mathbb{R}}\phi(y)\,
u\!\left(
t,\,
F_t\exp\!\left(
\frac{\omega_t}{\omega_T}x_K+
\sqrt{\frac{\omega_t(\omega_T-\omega_t)}{\omega_T}}\,y
\right)
\right)dy.
}
\]

### 3.5. Interpretation geometrique

Cette formule dit que la variance implicite s'obtient en moyennant \(u(t,S)\) sur des points \(S\) de la forme

\[
S(t,y)=F_t\exp\!\left(
\frac{\omega_t}{\omega_T}x_K+
\sqrt{\frac{\omega_t(\omega_T-\omega_t)}{\omega_T}}\,y
\right).
\]

Le terme

\[
\frac{\omega_t}{\omega_T}x_K
\]

interpole entre :

- \(0\) a la date \(0\) ;
- \(x_K\) a la date \(T\).

Le terme aleatoire

\[
\sqrt{\frac{\omega_t(\omega_T-\omega_t)}{\omega_T}}\,y
\]

est maximal a mi-parcours et nul aux extremites. Cela traduit le fait que les chemins sont "epingles" en :

- \(S_0\) au temps \(0\),
- \(K\) au temps \(T\).

---

## 4. Section 2.4.3 -- Expansion autour d'une volatilite constante

Supposons maintenant :

\[
\sigma_{\mathrm{loc}}(t,S)=\sigma_0+\delta\sigma(t,S),
\]

avec \(\sigma_0\) constante et \(\delta\sigma\) petit.

Alors :

\[
u(t,S)=\sigma_0^2+2\sigma_0\delta\sigma(t,S)+O(\delta\sigma^2),
\]

et

\[
\widehat{\sigma}_{K,T}^{\,2}
=
\sigma_0^2+2\sigma_0\,\delta\widehat{\sigma}_{K,T}+O(\delta\sigma^2).
\]

En remplaçant dans la formule precedente et en divisant par \(2\sigma_0\), on obtient :

\[
\delta\widehat{\sigma}_{K,T}
=
\frac{1}{T}\int_0^T dt
\int_{\mathbb{R}}\phi(y)\,
\delta\sigma\!\left(
t,\,
F_t\exp\!\left(
\frac{t}{T}x_K+\sigma_0\sqrt{\frac{(T-t)t}{T}}\,y
\right)
\right)dy.
\]

En ajoutant \(\sigma_0\) des deux cotes :

\[
\boxed{
\widehat{\sigma}_{K,T}
\approx
\frac{1}{T}\int_0^T dt
\int_{\mathbb{R}}\phi(y)\,
\sigma_{\mathrm{loc}}\!\left(
t,\,
F_t\exp\!\left(
\frac{t}{T}x_K+\sigma_0\sqrt{\frac{(T-t)t}{T}}\,y
\right)
\right)dy.
}
\]

### 4.1. Pourquoi on parle de "moyenne ponderee de local vol"

Cette formule justifie l'heuristique suivante :

\[
\widehat{\sigma}_{K,T}
\approx
\text{moyenne gaussienne de } \sigma_{\mathrm{loc}}(t,S)
\text{ le long de ponts lognormaux.}
\]

Encore une fois :

- **exact** : moyenne de \(\sigma_{\mathrm{loc}}^2\) ;
- **a l'ordre 1** : moyenne de \(\sigma_{\mathrm{loc}}\).

### 4.2. Chemin le plus probable

Le poids gaussien maximal est obtenu pour \(y=0\). Le point dominant est donc :

\[
S^*(t)=F_t\exp\!\left(\frac{t}{T}x_K\right).
\]

En ne gardant que cette trajectoire, on obtient l'approximation dite du "most likely path" :

\[
\widehat{\sigma}_{K,T}
\approx
\frac{1}{T}\int_0^T \sigma_{\mathrm{loc}}\!\left(t,S^*(t)\right)\,dt.
\]

Geometriquement :

- \(\ln S^*(0)=\ln S_0\),
- \(\ln S^*(T)=\ln K\),

donc \(\ln S^*(t)\) est la droite reliant \(\ln S_0\) a \(\ln K\).

### 4.3. Warning de Bergomi

Cette approximation est **utile pour comprendre** le smile, mais **pas assez precise pour le trading** sur smiles equity realistes. La raison profonde est que l'effet de \(\sigma_{\mathrm{loc}}\) sur la **densite** de \(S_t\) est crucial et ne peut pas etre neglige pour obtenir des niveaux absolus precis.

Autrement dit :

- pour l'intuition, l'ordre 1 est tres bon ;
- pour les prix de marche de precision, il faut resoudre l'equation forward de Dupire.

---

## 5. Section 2.4.5 -- Smile pres du forward

### 5.1. Parametrisation locale

On suppose la volatilite locale suffisamment reguliere et on developpe autour du forward \(F_t\) :

\[
\sigma_{\mathrm{loc}}(t,S)
=
\overline{\sigma}(t)+\alpha(t)x+\frac{\beta(t)}{2}x^2,
\qquad
x=\ln(S/F_t).
\]

Interpretation :

- \(\overline{\sigma}(t)\) : niveau instantane moyen de vol locale ;
- \(\alpha(t)\) : **skew local instantane** ;
- \(\beta(t)\) : **courbure locale instantanee**.

### 5.2. Remplacement dans la formule weak-local-vol

Dans la formule precedente, l'argument de la local vol est

\[
X(t,y)=\frac{t}{T}x_K+\sigma_0\sqrt{\frac{(T-t)t}{T}}\,y.
\]

Donc :

\[
\sigma_{\mathrm{loc}}(t,S(t,y))
=
\overline{\sigma}(t)+\alpha(t)X(t,y)+\frac{\beta(t)}{2}X^2(t,y).
\]

En integrant en \(y\) :

\[
\mathbb{E}[X(t,y)]
=
\frac{t}{T}x_K,
\]

et

\[
\mathbb{E}[X^2(t,y)]
=
\left(\frac{t}{T}\right)^2 x_K^2
+
\sigma_0^2\frac{(T-t)t}{T}.
\]

Par suite :

\[
\widehat{\sigma}_{K,T}
\approx
\frac{1}{T}\int_0^T \overline{\sigma}(t)\,dt
+
\left(
\frac{1}{T}\int_0^T \frac{t}{T}\alpha(t)\,dt
\right)x_K
+
\frac12
\left(
\frac{1}{T}\int_0^T \left(\frac{t}{T}\right)^2\beta(t)\,dt
\right)x_K^2
\]

\[
\qquad
+
\frac{\sigma_0^2 T}{2}
\left(
\frac{1}{T}\int_0^T \frac{(T-t)t}{T^2}\beta(t)\,dt
\right).
\]

Le dernier terme est une correction de niveau, independante de \(K\). Les termes importants pour la forme du smile sont ceux en \(x_K\) et \(x_K^2\).

### 5.3. Skew implicite ATMF

En derivant par rapport a \(\ln K\), puis en evaluant en \(K=F_T\) (soit \(x_K=0\)) :

\[
\boxed{
\mathcal{S}_T
:=
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
=
\frac{1}{T}\int_0^T \frac{t}{T}\alpha(t)\,dt.
}
\]

Cette formule est fondamentale.

### 5.4. Courbure implicite ATMF

De meme :

\[
\boxed{
\left.\frac{\partial^2 \widehat{\sigma}_{K,T}}{\partial (\ln K)^2}\right|_{K=F_T}
=
\frac{1}{T}\int_0^T \left(\frac{t}{T}\right)^2\beta(t)\,dt.
}
\]

### 5.5. Interpretation du poids \(t/T\)

Le poids \(t/T\) n'est pas un hasard.

Pour un strike \(K\) donne, le point pertinent a la date \(t\) est centre sur :

\[
\frac{t}{T}x_K.
\]

Donc :

- au debut (\(t\approx 0\)), la contrainte finale \(K\) ne compte presque pas ;
- a la fin (\(t\approx T\)), elle compte presque integralement.

Autrement dit, le skew implicite proche du forward est une **moyenne temporelle non uniforme** du skew local : les temps proches de la maturite pesent davantage.

### 5.6. Cas ou \(\alpha,\beta\) sont constants

Si \(\alpha(t)\equiv \alpha\) et \(\beta(t)\equiv \beta\), alors :

\[
\mathcal{S}_T=\frac{\alpha}{2},
\qquad
\left.\frac{\partial^2 \widehat{\sigma}_{K,T}}{\partial (\ln K)^2}\right|_{K=F_T}
=
\frac{\beta}{3}.
\]

Interpretation :

- le skew implicite vaut **la moitie** du skew local ;
- la courbure implicite vaut **un tiers** de la courbure locale.

### 5.7. Cas d'une decroissance en loi de puissance

Supposons

\[
\alpha(t)=
\begin{cases}
\alpha_0, & t\le \tau_0,\\
\alpha_0(\tau_0/t)^\gamma, & t>\tau_0.
\end{cases}
\]

Alors, pour \(T\) grand devant \(\tau_0\),

\[
\mathcal{S}_T
\sim
\frac{1}{2-\gamma}
\alpha_0\left(\frac{\tau_0}{T}\right)^\gamma.
\]

Donc :

- le skew implicite decroit avec **le meme exposant** \(\gamma\) que le skew local ;
- seule change la constante multiplicative \(1/(2-\gamma)\).

---

## 6. Dependance en \(\alpha(t)\), mouvement avec le spot et dynamique ATMF

Cette partie est cruciale pour un oral, car elle explique comment un modele local vol fait bouger le smile quand le spot varie.

### 6.1. Derivee par rapport au strike

Dans la formule faible-local-vol,

\[
\ln S(t,y)
=
\ln F_t+\frac{t}{T}x_K+\sigma_0\sqrt{\frac{(T-t)t}{T}}\,y.
\]

A spot initial fixe, si l'on derive par rapport a \(\ln K\), on a :

\[
\frac{\partial \ln S(t,y)}{\partial \ln K}=\frac{t}{T}.
\]

Donc

\[
\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln K}
=
\frac{1}{T}\int_0^T dt\int_{\mathbb{R}}\phi(y)\,
\frac{t}{T}\,
\frac{\partial \sigma_{\mathrm{loc}}}{\partial \ln S}(t,S(t,y)).
\]

Au voisinage ATMF, \(\partial \sigma_{\mathrm{loc}}/\partial \ln S=\alpha(t)+O(x)\), d'ou

\[
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
=
\frac{1}{T}\int_0^T \frac{t}{T}\alpha(t)\,dt.
\]

### 6.2. Derivee par rapport au spot initial a strike fixe

Maintenant \(K\) est fixe et on derive par rapport a \(\ln S_0\). Comme

\[
x_K=\ln K-\ln S_0-(r-q)T,
\]

on a \(\partial x_K/\partial \ln S_0=-1\), tandis que \(\partial \ln F_t/\partial \ln S_0=1\). Par suite :

\[
\frac{\partial \ln S(t,y)}{\partial \ln S_0}
=
1-\frac{t}{T}.
\]

Donc

\[
\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln S_0}
=
\frac{1}{T}\int_0^T dt\int_{\mathbb{R}}\phi(y)\,
\left(1-\frac{t}{T}\right)
\frac{\partial \sigma_{\mathrm{loc}}}{\partial \ln S}(t,S(t,y)).
\]

Au voisinage ATMF :

\[
\boxed{
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln S_0}\right|_{K=F_T}
=
\frac{1}{T}\int_0^T \left(1-\frac{t}{T}\right)\alpha(t)\,dt.
}
\]

### 6.3. Interpretation du poids \(1-t/T\)

Cette fois, les temps courts pesent davantage.

Intuition :

- quand on bouge le spot aujourd'hui, l'effet se transmet surtout au debut de la trajectoire ;
- a l'approche de l'echeance, la contrainte terminale \(K\) prend le dessus et "ecrase" cet effet.

### 6.4. Dynamique de la volatilite ATMF

Pour l'ATMF, le strike n'est pas fixe : il suit le forward,

\[
K=F_T(S_0).
\]

Donc la derivee totale vaut

\[
\frac{d\widehat{\sigma}_{F_T,T}}{d\ln S_0}
=
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln S_0}\right|_{K=F_T}
+
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
\frac{d\ln F_T}{d\ln S_0}.
\]

Or \(d\ln F_T/d\ln S_0=1\). Ainsi :

\[
\frac{d\widehat{\sigma}_{F_T,T}}{d\ln S_0}
=
\frac{1}{T}\int_0^T \left(1-\frac{t}{T}\right)\alpha(t)\,dt
+
\frac{1}{T}\int_0^T \frac{t}{T}\alpha(t)\,dt.
\]

Les poids se somment en \(1\), donc :

\[
\boxed{
\frac{d\widehat{\sigma}_{F_T,T}}{d\ln S_0}
=
\frac{1}{T}\int_0^T \alpha(t)\,dt.
}
\]

### 6.5. Interpretation fondamentale

La dynamique de la vol ATMF n'est pas libre :

\[
\text{ATMF move}
=
\text{moyenne temporelle uniforme du skew local } \alpha(t).
\]

Autrement dit, toute la dynamique de la vol ATMF dans le modele local vol est **encodee dans la structure par terme de \(\alpha(t)\)**.

---

## 7. Definition et interpretation de \(R_T\)

On appelle **Skew Stickiness Ratio** ou **ratio de rigidite du skew** :

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

### 7.1. Signification

Le denominateur est le **skew ATMF** :

\[
\mathcal{S}_T
=
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}.
\]

Le numerateur est le **mouvement de la vol ATMF** quand le spot bouge.

Donc \(R_T\) mesure :

> de combien la vol ATMF bouge, exprimee en unites de skew ATMF.

### 7.2. Regimes de marche de reference

#### Sticky-delta

Si la surface implicite reste fixe en moneyness, alors la vol ATMF ne bouge pas quand le spot bouge :

\[
\frac{d\widehat{\sigma}_{F_T,T}}{d\ln S_0}=0,
\qquad
R_T=0.
\]

#### Sticky-strike

Si les volatilites a strike fixe ne bougent pas, alors le mouvement de la vol ATMF vient uniquement du fait que l'ATMF se deplace le long de la courbe en strike. On obtient :

\[
\frac{d\widehat{\sigma}_{F_T,T}}{d\ln S_0}
=
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T},
\qquad
R_T=1.
\]

#### Local vol avec skew local constant

Si \(\alpha(t)\equiv \alpha\), alors

\[
\frac{d\widehat{\sigma}_{F_T,T}}{d\ln S_0}
=\alpha,
\qquad
\mathcal{S}_T=\frac{\alpha}{2},
\qquad
R_T=2.
\]

Conclusion :

- sticky-delta : \(R_T=0\),
- sticky-strike : \(R_T=1\),
- local vol classique : souvent \(R_T\) proche de \(2\) a courte maturite.

### 7.3. Formule en fonction du skew ATMF lui-meme

Posons

\[
\delta_T:=\mathcal{S}_T
=
\frac{1}{T^2}\int_0^T t\,\alpha(t)\,dt.
\]

Alors

\[
T^2\delta_T=\int_0^T t\,\alpha(t)\,dt.
\]

En derivant :

\[
2T\delta_T+T^2\delta_T'=T\alpha(T),
\]

soit

\[
\alpha(T)=2\delta_T+T\delta_T'.
\]

En integrant de \(0\) a \(T\) :

\[
\int_0^T \alpha(t)\,dt
=
\int_0^T \left(2\delta_t+t\delta_t'\right)dt
=
\int_0^T \delta_t\,dt + T\delta_T.
\]

Donc

\[
\frac{d\widehat{\sigma}_{F_T,T}}{d\ln S_0}
=
\delta_T+\frac{1}{T}\int_0^T \delta_t\,dt.
\]

Finalement :

\[
\boxed{
R_T
=
1+\frac{1}{T}\int_0^T \frac{\delta_t}{\delta_T}\,dt.
}
\]

### 7.4. Consequences importantes

#### Si le skew ATMF est plat en maturite

Si \(\delta_t\equiv \delta\), alors :

\[
R_T=1+\frac{1}{T}\int_0^T 1\,dt=2.
\]

C'est la celebre **regle du \(R=2\)**.

#### Quand \(T\to 0\)

Si \(\delta_t\) est continue au voisinage de \(0\), alors

\[
\frac{1}{T}\int_0^T \frac{\delta_t}{\delta_T}\,dt \longrightarrow 1,
\]

donc

\[
\boxed{R_T\longrightarrow 2 \quad \text{quand } T\to 0.}
\]

Cette limite est importante en pratique : le modele local vol impose un smile de court terme particulierement rigide.

---

## 8. Version generale : \(\overline{\sigma}(t)\) non constante

Si l'on ne developpe pas autour d'une constante \(\sigma_0\), mais autour d'une structure par terme deterministe \(\overline{\sigma}(t)\), alors

\[
\omega_t=\int_0^t \overline{\sigma}^2(u)\,du,
\qquad
\widehat{\sigma}_t^2=\frac{\omega_t}{t},
\qquad
\widehat{\sigma}_T^2=\frac{\omega_T}{T}.
\]

Dans la formule generale, le coefficient \(t/T\) est remplace par :

\[
\frac{\omega_t}{\omega_T}
=
\frac{\widehat{\sigma}_t^2 t}{\widehat{\sigma}_T^2 T}.
\]

Comme on passe ensuite de la variance a la volatilite, un facteur

\[
\frac{\overline{\sigma}(t)}{\widehat{\sigma}_T}
\]

apparait.

On obtient donc :

\[
\boxed{
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
=
\frac{1}{T}\int_0^T
\frac{\widehat{\sigma}_t^2 t}{\widehat{\sigma}_T^2 T}
\frac{\overline{\sigma}(t)}{\widehat{\sigma}_T}
\alpha(t)\,dt.
}
\]

\[
\boxed{
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln S_0}\right|_{K=F_T}
=
\frac{1}{T}\int_0^T
\left(
1-\frac{\widehat{\sigma}_t^2 t}{\widehat{\sigma}_T^2 T}
\right)
\frac{\overline{\sigma}(t)}{\widehat{\sigma}_T}
\alpha(t)\,dt.
}
\]

et

\[
\boxed{
\frac{d\widehat{\sigma}_{F_T,T}}{d\ln S_0}
=
\frac{1}{T}\int_0^T
\frac{\overline{\sigma}(t)}{\widehat{\sigma}_T}\alpha(t)\,dt.
}
\]

Ces formules generiques montrent que la logique precedente survit :

- un poids associe a l'effet-strike ;
- un poids associe a l'effet-spot ;
- et leur somme donne la dynamique ATMF.

---

## 9. Section 2.4.6 -- Resultat exact a courte maturite

Cette section est tres importante, car elle fournit un resultat **exact**, et non plus perturbatif.

### 9.1. Point de depart : la formule de Dupire

Dans les coordonnees \((T,y)\), avec

\[
y=\ln(K/F_T),
\]

Dupire permet d'exprimer la volatilite locale en fonction de la surface implicite \(\widehat{\sigma}(T,y)\).

Quand \(T\to 0\), les termes dominants de cette formule donnent :

\[
\sigma^2(0,S_0 e^y)
=
\frac{\widehat{\sigma}^2(0,y)}
\left(y\,\widehat{\sigma}(0,y)\,\partial_y\widehat{\sigma}(0,y)-1\right)^2}.
\]

En prenant la racine et en reordonnant :

\[
\frac{1}{\sigma(0,S_0 e^y)}
=
\pm\left(\frac{y}{\widehat{\sigma}(0,y)}\right)'.
\]

On choisit le signe compatible avec des volatilites positives, puis on integre entre \(0\) et \(y\) :

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

En revenant a la variable \(K=S_0 e^y\) :

\[
\boxed{
\frac{1}{\widehat{\sigma}(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm{loc}}(0,S)}.
}
\]

### 9.2. Pourquoi c'est une moyenne harmonique

Ecrite autrement :

\[
\frac{1}{\widehat{\sigma}(0,K)}
=
\text{moyenne de } \frac{1}{\sigma_{\mathrm{loc}}(0,S)}
\text{ sur l'intervalle logarithmique entre } S_0 \text{ et } K.
\]

Ce n'est donc pas :

- ni une moyenne arithmetique de \(\sigma_{\mathrm{loc}}\),
- ni une moyenne quadratique.

C'est une **moyenne harmonique**.

### 9.3. Interpretation intuitive

Quand \(T\to 0\) :

- il n'y a pratiquement plus de moyenne temporelle ;
- le mouvement se joue sur une tres courte echelle ;
- ce qui compte est la "difficulte locale" a traverser les niveaux entre \(S_0\) et \(K\).

Si \(\sigma_{\mathrm{loc}}(0,S)\) devient tres faible sur une zone intermediaire, l'actif ne peut presque pas traverser cette zone a temps tres court. Il est donc naturel que l'implicite correspondante baisse tres fortement ; la moyenne harmonique capture exactement cela.

### 9.4. Cas ATM

Quand \(K\to S_0\), on retrouve :

\[
\widehat{\sigma}(0,S_0)=\sigma_{\mathrm{loc}}(0,S_0).
\]

Ce point est coherent avec l'intuition : a echeance instantanee, l'implicite ATM lit directement la vol locale au point initial.

---

## 10. Lien avec la formule de Dupire

Le lien avec Dupire est double.

### 10.1. Dupire direct

La formule de Dupire exprime :

\[
\sigma_{\mathrm{loc}}^2(T,K)
=
\frac{\partial_T C(T,K)+(r-q)K\partial_K C(T,K)+qC(T,K)}
\frac12 K^2 \partial_{KK}C(T,K)
.
\]

Autrement dit :

- si la surface de prix (ou d'implicites) est connue,
- on peut reconstruire la fonction locale.

### 10.2. Le chapitre 2.4 fait le chemin inverse

Le probleme inverse :

\[
\sigma_{\mathrm{loc}}(t,S) \mapsto \widehat{\sigma}(K,T)
\]

n'admet pas de formule simple exacte en general. Bergomi fournit :

1. une identite exacte mais implicite ;
2. une approximation utile a l'ordre 1 ;
3. un resultat exact en petite maturite.

### 10.3. Point conceptuel

Dupire permet une calibration parfaite aux vanilles a la date initiale. Mais cela ne veut pas dire que la dynamique induite du smile est realiste. C'est justement le sujet profond du chapitre 2.4 : **calibrer n'est pas dynamiser correctement**.

---

## 11. Lien avec les modeles de volatilite stochastique

### 11.1. Comparaison generale

Dans un modele de volatilite stochastique :

\[
\begin{cases}
dS_t=(r-q)S_t\,dt+\sqrt{v_t}S_t\,dW_t^S,\\
dv_t=\cdots
\end{cases}
\]

la surface implicite future depend d'un facteur latent aleatoire \(v_t\). Le smile peut donc bouger "de lui-meme", pas seulement parce que le spot a bouge.

Dans un modele local vol, au contraire :

- la volatilite instantanee est une fonction deterministe de \((t,S_t)\) ;
- toute la dynamique implicite est entierement forcee par la trajectoire du spot.

### 11.2. Consequence sur le SSR

Les modeles stochastiques equity produisent souvent des dynamics plus proches de :

- sticky-delta a court terme,
- ou d'un regime intermediaire entre sticky-delta et sticky-strike.

Le modele local vol, lui, tend souvent a produire un comportement **trop rigide**, avec :

\[
R_T \approx 2 \quad \text{a courte maturite}.
\]

### 11.3. Interpretation financiere

Sur le marche equity, quand le spot baisse :

- la skew monte souvent ;
- mais la vol ATMF ne reagit pas exactement comme le predirait un modele local vol pur.

Le smile observe est en pratique moins "contraint" que dans local vol. C'est une raison majeure pour laquelle les modeles de volatilite stochastique sont preferes pour la dynamique et la couverture.

### 11.4. Lien conceptuel avec la projection markovienne

Un modele local vol peut etre vu comme une projection markovienne qui reproduit les lois marginales des vanilles a chaque maturite. Mais :

- reproduire les marges n'implique pas reproduire les dynamiques conditionnelles ;
- donc un modele local vol peut calibrer parfaitement la surface initiale tout en ayant une dynamique de smile peu realiste.

---

## 12. Warnings, limites et zones de validite

### 12.1. La formule weak-local-vol est une approximation d'ordre 1

Elle ne doit pas etre surinterpretee.

Elle est tres utile pour :

- comprendre les dependances qualitatives ;
- extraire le role de \(\alpha(t)\), \(\beta(t)\), \(R_T\) ;
- faire des asymptotiques pres du forward.

Elle est insuffisante pour :

- reproduire avec precision les niveaux absolus de smile sur des equities a fort skew ;
- faire du pricing/trading fin sans resolution numerique.

### 12.2. Le developpement pres du forward est local

Les formules en \(\alpha(t)\) et \(\beta(t)\) decrivent :

- le skew ATMF ;
- la courbure ATMF ;

mais pas necessairement les ailes du smile.

### 12.3. Risque de confusion entre exact et approximatif

Il faut distinguer :

- l'identite exacte sur \(\widehat{\sigma}^2\),
- l'approximation d'ordre 1 sur \(\widehat{\sigma}\),
- et la formule exacte a courte maturite sur \(1/\widehat{\sigma}\).

Ce sont trois regimes mathematiques differents.

### 12.4. Limites structurelles du modele local vol

Le modele local vol :

- calibre parfaitement les vanilles a \(t=0\) ;
- hedge bien les vanilles "statistiquement" si la surface ne bouge pas de maniere trop exogene ;
- mais impose une dynamique de smile qui peut etre tres irrealisable.

En particulier :

- \(R_T\) trop eleve ;
- smile trop rigide ;
- mauvaise gestion des mouvements de surface non expliques par le spot.

---

## 13. Resume ultra-court a retenir pour l'examen

1. **Identite exacte** :

\[
\widehat{\sigma}_{K,T}^{\,2}
=
\frac{
\mathbb{E}\!\left[\int_0^T \text{dollar gamma}\times \sigma_{\mathrm{loc}}^2\,dt\right]
}{
\mathbb{E}\!\left[\int_0^T \text{dollar gamma}\,dt\right]
}.
\]

2. **Approximation weak-local-vol** :

\[
\widehat{\sigma}_{K,T}
\approx
\frac{1}{T}\int_0^T dt\int \phi(y)\,
\sigma_{\mathrm{loc}}(t,S(t,y))\,dy.
\]

3. **Skew implicite ATMF** :

\[
\mathcal{S}_T
=
\frac{1}{T}\int_0^T \frac{t}{T}\alpha(t)\,dt.
\]

4. **Mouvement a strike fixe** :

\[
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial \ln S_0}\right|_{K=F_T}
=
\frac{1}{T}\int_0^T \left(1-\frac{t}{T}\right)\alpha(t)\,dt.
\]

5. **Mouvement de la vol ATMF** :

\[
\frac{d\widehat{\sigma}_{F_T,T}}{d\ln S_0}
=
\frac{1}{T}\int_0^T \alpha(t)\,dt.
\]

6. **Ratio de rigidite** :

\[
R_T=
\frac{d\widehat{\sigma}_{F_T,T}/d\ln S_0}
{\mathcal{S}_T},
\qquad
R_T\to 2 \text{ quand } T\to 0.
\]

7. **Petite maturite exacte** :

\[
\frac{1}{\widehat{\sigma}(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm{loc}}(0,S)}.
\]

---

## 14. Phrase de conclusion pour un oral

Le point cle du chapitre 2.4 est que le modele local vol ne se contente pas de calibrer une surface : il impose une dynamique tres specifique du smile. Cette dynamique est en grande partie codee par le skew local instantane \(\alpha(t)\), et c'est precisement ce caractere trop rigide -- visible par exemple via \(R_T\approx 2\) a courte maturite -- qui explique les limites financieres du modele local vol face aux modeles de volatilite stochastique.
