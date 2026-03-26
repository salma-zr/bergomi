# Bergomi, Chapitre 2.4 -- Des volatilites locales aux volatilites implicites

## Objet du rapport

Ce rapport est volontairement cale sur **le meme contenu que les slides actuels**.

Autrement dit, il developpe en detail les points suivants, et **seulement** ceux-la :

1. le probleme "local vol \(\to\) implied vol" ;
2. l'identite exacte : variance implicite comme moyenne ponderee de variance locale ;
3. l'approximation weak local vol ;
4. le developpement de la local vol pres du forward ;
5. les formules de skew et de courbure implicites ;
6. le cas d'une structure par terme du skew en loi de puissance ;
7. le resultat exact a courte maturite ;
8. la conclusion conceptuelle : lien avec Dupire, lien avec la stoch vol, et limites.

Donc :

- je garde un niveau **tres detaille** ;
- mais je ne developpe pas ici des blocs supplementaires qui ne figurent plus dans les slides actuels, comme une section autonome sur \(R_T\) ou la dynamique ATMF.

---

## 1. La question posee par Bergomi

### 1.1. Le point de depart

Dans un modele de volatilite locale, on suppose que sous la mesure risque-neutre :

\[
dS_t=(r-q)S_t\,dt+\sigma_{\mathrm{loc}}(t,S_t)S_t\,dW_t.
\]

Ici :

- \(S_t\) est le prix du sous-jacent ;
- \(r\) est le taux sans risque ;
- \(q\) est le taux de dividende / carry ;
- \(\sigma_{\mathrm{loc}}(t,S)\) est une fonction **deterministe** du temps et du spot.

Le probleme usuel de Dupire est :

\[
\widehat{\sigma}(K,T)\quad \longrightarrow \quad \sigma_{\mathrm{loc}}(t,S).
\]

Le probleme de Bergomi dans la section 2.4 est le probleme inverse :

\[
\sigma_{\mathrm{loc}}(t,S)\quad \longrightarrow \quad \widehat{\sigma}(K,T).
\]

### 1.2. Pourquoi cette question est importante ?

Parce que calibrer un modele local vol revient a dire :

> "je choisis une fonction \(\sigma_{\mathrm{loc}}(t,S)\) qui reproduit aujourd'hui la surface vanille."

Mais alors une question naturelle est :

> "qu'est-ce que cette fonction implique pour les volatilites implicites, et quelle structure mathematique se cache derriere cette implication ?"

Le chapitre 2.4 donne une reponse en deux temps :

1. une **identite exacte** ;
2. une **approximation analytique exploitable**.

---

## 2. Rappels minimaux de cours pour comprendre la derivee

Comme tu m'as dit que certaines notions n'etaient pas claires, je rappelle ici les objets indispensables.

### 2.1. Prix d'une option

Si \(P(t,S)\) est le prix d'une option europeenne, alors dans un modele de diffusion de volatilite \(\sigma(t,S)\), on a formellement :

\[
\partial_t P+(r-q)S\partial_S P+\frac12 \sigma^2(t,S)S^2\partial_{SS}P-rP=0.
\]

Cette equation vient du cours par Itô, replication et absence d'arbitrage.

### 2.2. Delta, gamma, theta

On note :

\[
\Delta=\partial_S P,\qquad
\Gamma=\partial_{SS}P,\qquad
\Theta=\partial_t P.
\]

Interpretation :

- \(\Delta\) = sensibilite lineaire au spot ;
- \(\Gamma\) = courbure, donc sensibilite du delta ;
- \(\Theta\) = effet du temps.

### 2.3. Qu'est-ce qu'un P&L de couverture delta ?

Le **P&L** ("profit and loss") est le gain ou la perte realise sur un petit intervalle de temps.

Si l'on detient une option et que l'on prend en face \(-\Delta\) actions, le terme lineaire en \(dS_t\) disparait.

Par Itô :

\[
dP=\partial_t P\,dt+\partial_S P\,dS+\frac12 \partial_{SS}P\,d\langle S\rangle.
\]

Si l'on retranche \(\Delta\,dS\), il reste un terme de type :

\[
\partial_t P\,dt+\frac12 \Gamma\,d\langle S\rangle.
\]

Autrement dit, dans une couverture delta, le residu est de type :

- theta ;
- plus gamma fois variation quadratique.

**C'est exactement pourquoi le gamma apparait dans Bergomi.**

---

## 3. Section 2.4.1 -- Identite exacte : moyenne ponderee de variance locale

### 3.1. Idee generale

Bergomi compare deux modeles :

- **modele I** : modele de reference, avec prix d'option \(P_1(t,S)\) et volatilite instantanee \(\sigma_1(t,S)\) ;
- **modele II** : modele "reel", dans lequel le sous-jacent suit
  \[
  dS_t=(r-q)S_t\,dt+\sigma_{2,t}S_t\,dW_t.
  \]

Le but est de comprendre :

> quel est le prix de l'erreur faite lorsque l'on couvre l'option avec le modele I alors que le sous-jacent evolue en realite selon le modele II ?

### 3.2. Quantite actualisee

On introduit :

\[
Q_t=e^{-rt}P_1(t,S_t).
\]

Cette quantite est naturelle car les quantites actualisees sont les bonnes quantites en pricing sous mesure risque-neutre.

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

Or

\[
d\langle S\rangle_t=\sigma_{2,t}^2S_t^2\,dt.
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
\frac12 \sigma_{2,t}^2S_t^2\partial_{SS}P_1\,dt
\right].
\]

En prenant l'esperance conditionnelle, le terme brownien disparait :

\[
\mathbb E_2[dQ_t\mid\mathcal F_t]
=
e^{-rt}
\left[
-rP_1+\partial_t P_1+(r-q)S_t\partial_S P_1+\frac12 \sigma_{2,t}^2S_t^2\partial_{SS}P_1
\right]dt.
\]

### 3.4. Utilisation de l'EDP du modele I

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
\mathbb E_2[dQ_t\mid\mathcal F_t]
=
e^{-rt}\frac12 S_t^2\partial_{SS}P_1(t,S_t)\big(\sigma_{2,t}^2-\sigma_1^2(t,S_t)\big)\,dt.
\]

### 3.5. Interpretation en P&L

Cette formule dit exactement :

> le drift residuel de la couverture delta est proportionnel au gamma, et a l'ecart entre la variance instantanee du monde reel et celle du modele de couverture.

Donc :

- si \(\sigma_{2,t}^2=\sigma_1^2(t,S_t)\), il n'y a pas d'erreur residuelle ;
- sinon, un ecart de P&L apparait ;
- plus l'option est convexe, plus cet ecart est important.

### 3.6. Integration jusqu'a maturite

En integrant entre \(0\) et \(T\) :

\[
\mathbb E_2[Q_T]
=
Q_0+
\mathbb E_2\left[
\int_0^T e^{-rt}\frac12 S_t^2\partial_{SS}P_1(t,S_t)\big(\sigma_{2,t}^2-\sigma_1^2(t,S_t)\big)\,dt
\right].
\]

Comme

\[
Q_T=e^{-rT}f(S_T),
\]

on a

\[
\mathbb E_2[Q_T]=P_2(0,S_0,\cdot),
\qquad
Q_0=P_1(0,S_0).
\]

Donc :

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

### 3.7. Choix Black-Scholes et identite exacte

On choisit maintenant comme modele I le modele Black-Scholes de volatilite constante egale a la volatilite implicite de l'option :

\[
\sigma_1(t,S)\equiv \widehat{\sigma}_{K,T}.
\]

Par definition meme de \(\widehat{\sigma}_{K,T}\), le prix Black-Scholes et le prix de l'option coincident a \(t=0\).

Donc le membre de gauche moins le premier terme du membre de droite vaut zero.

Il reste :

\[
0=
\mathbb E_2\left[
\int_0^T e^{-rt}\frac12 S_t^2\Gamma_t^{BS}(\widehat{\sigma}_{K,T})\big(\sigma_{2,t}^2-\widehat{\sigma}_{K,T}^2\big)\,dt
\right].
\]

En isolant \(\widehat{\sigma}_{K,T}^2\), on obtient :

\[
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
\]

Si le modele II est justement le modele local vol, c'est-a-dire \(\sigma_{2,t}=\sigma_{\mathrm{loc}}(t,S_t)\), alors :

\[
\boxed{
\widehat{\sigma}_{K,T}^{\,2}
=
\frac{
\mathbb E^{\mathrm{loc}}\!\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}(\widehat{\sigma}_{K,T})
\sigma_{\mathrm{loc}}^2(t,S_t)\,dt
\right]
}{
\mathbb E^{\mathrm{loc}}\!\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}(\widehat{\sigma}_{K,T})\,dt
\right]
}.
}
\]

### 3.8. Interpretation precise

Cette formule signifie :

- l'objet exact est la **variance implicite** ;
- elle est une **moyenne ponderee de variance locale** ;
- les poids sont des **dollar gammas actualises**.

Warning essentiel :

- **exactement** : moyenne de \(\sigma_{\mathrm{loc}}^2\) ;
- **pas** moyenne directe de \(\sigma_{\mathrm{loc}}\).

---

## 4. Section 2.4.2 et 2.4.3 -- Approximation weak local vol

### 4.1. Pourquoi une approximation est necessaire ?

L'identite precedente est exacte, mais implicite :

- \(\widehat{\sigma}_{K,T}\) apparait des deux cotes ;
- les poids dependent eux-memes de \(\widehat{\sigma}_{K,T}\).

Donc on cherche une approximation analytique.

### 4.2. Hypothese weak local vol

On ecrit la variance locale comme :

\[
u(t,S)=\sigma_{\mathrm{loc}}^2(t,S)=u_0(t)+\delta u(t,S),
\]

ou bien, dans le cas plus simple :

\[
\sigma_{\mathrm{loc}}(t,S)=\sigma_0+\delta\sigma(t,S),
\]

avec perturbation petite.

L'idee est :

- prendre un modele de reference simple ;
- figer la loi au premier ordre ;
- garder seulement la contribution explicite de la perturbation.

### 4.3. Formule approchee autour d'une vol constante

Autour d'une vol constante \(\sigma_0\), Bergomi obtient :

\[
\widehat{\sigma}_{K,T}
\approx
\frac{1}{T}
\int_0^T dt
\int_{\mathbb R}\phi(y)\,
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

### 4.4. Interpretation geometrique

Cette formule dit :

> l'implicite est approximativement une moyenne gaussienne de la local vol le long de chemins intermediaires reliant \(S_0\) au strike \(K\).

Le terme

\[
S(t,y)=F_t\exp\!\Big(
\frac{t}{T}x_K+\sigma_0\sqrt{\frac{(T-t)t}{T}}\,y
\Big)
\]

decrit le spot intermediaire visite a la date \(t\).

Si l'on garde seulement \(y=0\), on retient le chemin central, souvent interprete comme le "chemin le plus probable".

### 4.5. Warning

Cette formule est tres utile pour comprendre les derivations du skew.

Mais Bergomi insiste :

- elle est d'ordre 1 ;
- elle est bonne pour l'intuition ;
- elle n'est pas suffisante pour une precision de trading sur des smiles equity realistes.

---

## 5. Section 2.4.5 -- Smile pres du forward

### 5.1. Parametrisation locale

On developpe la local vol autour du forward \(F_t\) :

\[
\sigma_{\mathrm{loc}}(t,S)=\bar{\sigma}(t)+\alpha(t)x+\frac{\beta(t)}{2}x^2,
\qquad
x=\ln\!\left(\frac{S}{F_t}\right).
\]

Interpretation :

- \(\bar{\sigma}(t)\) = niveau local ;
- \(\alpha(t)\) = skew local instantane ;
- \(\beta(t)\) = courbure locale instantanee.

### 5.2. Variable intermediaire

Dans la formule weak local vol, on pose

\[
X(t,y)=\frac{t}{T}x_K+\sigma_0\sqrt{\frac{(T-t)t}{T}}\,y.
\]

En remplaçant \(x\) par \(X(t,y)\), on obtient :

\[
\sigma_{\mathrm{loc}}(t,S(t,y))
=
\bar{\sigma}(t)+\alpha(t)X(t,y)+\frac{\beta(t)}{2}X(t,y)^2.
\]

### 5.3. Calcul des moyennes gaussiennes

Comme \(y\) est gaussien centre reduit :

\[
\int_{\mathbb R}\phi(y)\,y\,dy=0,
\qquad
\int_{\mathbb R}\phi(y)\,y^2\,dy=1.
\]

Donc, a l'ordre 1 en \(\alpha,\beta\), on trouve :

\[
\widehat{\sigma}_{K,T}
\approx
\frac{1}{T}\int_0^T \bar{\sigma}(t)\,dt
\;+\;
\left(
\frac{1}{T}\int_0^T \frac{t}{T}\alpha(t)\,dt
\right)x_K
\;+\;
\frac12
\left(
\frac{1}{T}\int_0^T\Big(\frac{t}{T}\Big)^2\beta(t)\,dt
\right)x_K^2.
\]

### 5.4. Skew implicite pres du forward

En derivant par rapport a \(\ln K\) puis en evaluant en \(K=F_T\) (donc \(x_K=0\)), on obtient :

\[
\boxed{
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial\ln K}\right|_{K=F_T}
=
\frac{1}{T}\int_0^T \frac{t}{T}\alpha(t)\,dt.
}
\]

Interpretation :

> le skew implicite est une moyenne ponderee du skew local \(\alpha(t)\).

Le poids \(t/T\) donne plus d'importance aux temps proches de la maturite.

### 5.5. Courbure implicite pres du forward

De meme :

\[
\boxed{
\left.\frac{\partial^2 \widehat{\sigma}_{K,T}}{\partial(\ln K)^2}\right|_{K=F_T}
=
\frac{1}{T}\int_0^T \Big(\frac{t}{T}\Big)^2\beta(t)\,dt.
}
\]

### 5.6. Cas constant

Si \(\alpha(t)\equiv\alpha\), alors :

\[
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial\ln K}\right|_{K=F_T}
=
\frac{\alpha}{2}.
\]

Donc :

> quand le skew local est constant dans le temps, le skew implicite ATMF vaut la moitie du skew local.

---

## 6. Structure par terme du skew : loi de puissance

On suppose :

\[
\alpha(t)=
\begin{cases}
\alpha_0, & t\le \tau_0,\\[0.3em]
\alpha_0\left(\dfrac{\tau_0}{t}\right)^\gamma, & t>\tau_0.
\end{cases}
\]

Alors, pour \(T\) grand devant \(\tau_0\),

\[
\left.\frac{\partial \widehat{\sigma}_{K,T}}{\partial\ln K}\right|_{K=F_T}
\sim
\frac{1}{2-\gamma}\,
\alpha_0\left(\frac{\tau_0}{T}\right)^\gamma.
\]

### Interpretation

Ce resultat signifie :

- le skew implicite ATMF decroit avec le **meme exposant** \(\gamma\) que le skew local ;
- la structure par terme du smile implicite herite directement de la structure par terme de \(\alpha(t)\).

Autrement dit :

> \(\alpha(t)\) controle non seulement le niveau du skew, mais aussi sa decroissance avec la maturite.

---

## 7. Section 2.4.6 -- Resultat exact a courte maturite

### 7.1. Point de depart

Quand \(T\to 0\), Bergomi part de la formule de Dupire dans les coordonnees \((T,y)\), avec

\[
y=\ln(K/S_0).
\]

En gardant les termes dominants en petite maturite, on obtient finalement une relation exacte entre local vol et implicite.

### 7.2. Resultat exact

\[
\boxed{
\frac{1}{\widehat{\sigma}(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm{loc}}(0,S)}.
}
\]

### 7.3. Interpretation

Ce resultat dit que, a maturite tres courte :

- on n'obtient pas une moyenne arithmetique de \(\sigma_{\mathrm{loc}}\) ;
- ni une moyenne quadratique ;
- mais une **moyenne harmonique** de \(1/\sigma_{\mathrm{loc}}\).

Autrement dit :

> a tres courte maturite, il n'y a presque plus de moyenne temporelle ; ce qui domine est une moyenne spatiale entre \(S_0\) et \(K\).

### 7.4. Pourquoi est-ce naturel ?

Si \(\sigma_{\mathrm{loc}}(0,S)\) devient tres faible sur une region qu'il faut franchir pour aller de \(S_0\) a \(K\), alors le sous-jacent a beaucoup de mal a traverser cette region sur une maturite tres courte.

Il est donc naturel que l'implicite reflète cette difficulte de passage.

C'est exactement ce que capture la moyenne harmonique.

---

## 8. A retenir : Dupire, stoch vol, limites

### 8.1. Lien avec Dupire

Dupire fait le chemin :

\[
\widehat{\sigma}(K,T)\quad \longrightarrow \quad \sigma_{\mathrm{loc}}(t,S).
\]

Le chapitre 2.4 etudie l'autre sens :

\[
\sigma_{\mathrm{loc}}(t,S)\quad \longrightarrow \quad \widehat{\sigma}(K,T).
\]

### 8.2. Lien avec les modeles de volatilite stochastique

Le modele local vol est tres utile pour comprendre analytiquement le smile.

Mais les modeles de volatilite stochastique sont souvent preferes en pratique pour la dynamique, car ils introduisent des facteurs aleatoires supplementaires pour la variance future.

### 8.3. Limite majeure de l'approximation weak local vol

L'approximation d'ordre 1 :

- explique bien le skew et la courbure ;
- donne une bonne intuition ;
- mais n'est pas assez precise pour les **niveaux absolus** sur des smiles equity realistes.

### 8.4. Message central de la section

Le message essentiel de Bergomi, dans le perimetre des slides actuels, est :

> l'implicite n'est pas un objet arbitraire ; elle est fortement contrainte par la structure spatio-temporelle de la volatilite locale.

Et plus precisement :

- l'identite exacte porte sur une moyenne ponderee de **variance locale** ;
- l'ordre 1 weak local vol donne une moyenne exploitable de **volatilite locale** ;
- le skew implicite pres du forward est gouverne par \(\alpha(t)\) ;
- la structure par terme du skew implicite herite de celle de \(\alpha(t)\) ;
- a tres courte maturite, on obtient une moyenne harmonique exacte.

---

## 9. Resume ultra-court pour l'examen

Si tu dois retenir seulement six phrases, retiens celles-ci :

1. Bergomi inverse la logique de Dupire : il part de \(\sigma_{\mathrm{loc}}(t,S)\) et cherche \(\widehat{\sigma}(K,T)\).
2. La formule exacte dit que \(\widehat{\sigma}_{K,T}^2\) est une moyenne ponderee de \(\sigma_{\mathrm{loc}}^2\), avec des poids de type dollar gamma.
3. L'approximation weak local vol donne une formule analytique d'ordre 1 pour \(\widehat{\sigma}_{K,T}\).
4. Pres du forward, le skew implicite est une moyenne ponderee de \(\alpha(t)\), avec poids \(t/T\).
5. Si \(\alpha(t)\) suit une loi de puissance, le skew implicite decroit avec le meme exposant.
6. Quand \(T\to 0\), l'inverse de l'implicite est une moyenne harmonique de l'inverse de la local vol.

