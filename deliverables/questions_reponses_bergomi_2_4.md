# Questions / reponses -- Bergomi 2.4

Ce document contient 30 questions typiques d'oral ou d'examen, avec des reponses precises, rigoureuses et pedagogiques.

---

## 1. Quel est l'objectif de la section 2.4 de Bergomi ?

**Reponse.**
L'objectif est de comprendre comment une fonction de volatilite locale \(\sigma_{\mathrm loc}(t,S)\) se traduit en surface de volatilite implicite \(\hat\sigma(K,T)\). La formule de Dupire donne deja l'application inverse, de l'implicite vers la local vol. Bergomi cherche ici une lecture conceptuelle et approximative du chemin inverse, utile pour comprendre les dynamiques de smile dans un modele local vol.

---

## 2. Quelle est l'identite fondamentale de 2.4.1 ?

**Reponse.**
Dans un modele local vol,
\[
\hat\sigma_{KT}^2
=
\frac{
\mathbb E\left[\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}(\hat\sigma_{KT})\,
\sigma_{\mathrm loc}(t,S_t)^2\,dt\right]
}
{
\mathbb E\left[\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}(\hat\sigma_{KT})\,dt\right]
}.
\]
Elle dit que la **variance implicite** est une moyenne ponderee de la **variance locale**.

---

## 3. Pourquoi est-ce la variance, et non la volatilite, qui est moyennee ?

**Reponse.**
Parce que l'erreur de couverture gamma/theta depend du terme
\[
\frac12 S_t^2 \Gamma_t (\sigma_{2,t}^2-\sigma_1^2).
\]
La quantite naturelle dans le PnL de couverture est donc la **variance instantanee**, pas la volatilite elle-meme.

---

## 4. Que representent les poids dans cette moyenne ?

**Reponse.**
Les poids sont donnes par le **dollar gamma actualise**
\[
e^{-rt}S_t^2\Gamma_t^{BS}.
\]
Ils sont ensuite moyenes sur les trajectoires du spot. Les zones de temps et d'espace ou l'option a beaucoup de gamma comptent donc davantage dans la moyenne.

---

## 5. Comment derive-t-on l'identite (2.30) ?

**Reponse.**
On pose \(Q_t=e^{-rt}P_1(t,S_t)\), ou \(P_1\) est le prix de l'option dans un modele de base I. On applique Itô sous la dynamique reelle II, puis on utilise la PDE satisfaite par \(P_1\). Le residu fait apparaitre
\[
\frac12 e^{-rt}S_t^2\partial_{SS}P_1(\sigma_{2,t}^2-\sigma_1^2)\,dt.
\]
On integre ensuite de \(0\) a \(T\) et on utilise \(Q_T=e^{-rT}f(S_T)\).

---

## 6. Quelle est l'intuition financiere derriere (2.30) ?

**Reponse.**
Si l'on delta-couvre l'option avec un modele de base I alors que le vrai monde suit un autre modele II, le PnL cumule d'erreur de couverture provient du terme gamma/theta. Le prix dans le modele II est donc le prix dans le modele I plus l'esperance du PnL de couverture manquant.

---

## 7. Pourquoi la formule (2.32) est-elle implicite ?

**Reponse.**
Parce que \(\hat\sigma_{KT}\) apparait deja dans le membre de droite :

1. dans le gamma Black-Scholes utilise pour ponderer ;
2. dans la loi du spot si l'on reformule avec un modele BS de reference ;
3. dans le fait que les trajectoires pertinentes dependent du strike et de la maturite.

On ne peut donc pas en deduire directement \(\hat\sigma_{KT}\) sans approximation supplementaire.

---

## 8. Qu'appelle-t-on "weak local volatility" ?

**Reponse.**
On suppose que la variance locale
\[
u(t,S)=\sigma_{\mathrm loc}(t,S)^2
\]
est une petite perturbation d'une variance purement temporelle
\[
u_0(t)=\sigma_0(t)^2.
\]
Autrement dit
\[
u(t,S)=u_0(t)+\delta u(t,S),
\]
avec \(\delta u\) petit.

---

## 9. Pourquoi la correction de densite disparait-elle au premier ordre ?

**Reponse.**
Parce qu'on developpe un quotient. Si
\[
R(\varepsilon)=\frac{\mathbb E_\varepsilon[(u_0+\varepsilon h)G]}{\mathbb E_\varepsilon[G]},
\]
alors le terme de correction de la loi \(\mathbb E_\varepsilon\) apparait a la fois au numerateur et au denominateur, avec le meme coefficient multiplicatif \(u_0\). Au premier ordre, il se simplifie exactement dans le quotient.

---

## 10. Quel est le role du fait que le dollar gamma actualise soit une martingale ?

**Reponse.**
Cela permet de calculer explicitement le denominateur de (2.35). Si
\[
M_t=e^{-rt}S_t^2\Gamma_t^{BS},
\]
est une martingale, alors
\[
\mathbb E[M_t]=M_0.
\]
En integrant en temps, on obtient directement le denominateur :
\[
\mathbb E\left[\int_0^T M_t\,dt\right]=T M_0.
\]

---

## 11. Quelle est la formule d'approximation centrale obtenue par Bergomi ?

**Reponse.**
Au premier ordre autour d'une vol constante \(\sigma_0\),
\[
\hat\sigma_{KT}
\approx
\frac1T\int_0^T dt\int_{\mathbb R}\phi(y)\,
\sigma_{\mathrm loc}\!\left(
t,
F_t e^{\frac{t}{T}x_K+\sigma_0\sqrt{\frac{(T-t)t}{T}}\,y}
\right)dy.
\]

---

## 12. Comment interpreter la variable \(y\) dans cette formule ?

**Reponse.**
\(y\) est une variable gaussienne standard qui parametre l'ecart autour de la trajectoire centrale reliant \(\ln S_0\) a \(\ln K\). Le point \(y=0\) correspond a la trajectoire log-lineaire la plus probable.

---

## 13. Pourquoi dit-on que l'implicite est une image lissee de la local vol ?

**Reponse.**
Parce que l'implicite ne lit pas \(\sigma_{\mathrm loc}(t,S)\) en un point, mais fait une moyenne :

- en temps,
- sur plusieurs niveaux de spot,
- et sur plusieurs trajectoires.

Les irregularites locales de \(\sigma_{\mathrm loc}\) sont donc attenuees.

---

## 14. Pourquoi 2.42 est-elle utile meme si elle n'est pas assez precise pour le trading ?

**Reponse.**
Parce qu'elle donne des formules simples pour le skew, la courbure et les lois d'echelle en maturite. Meme si le niveau absolu des volatilites est mal approche, les **differences de volatilites** sont souvent plus robustes.

---

## 15. Quel est le developpement local de la volatilite locale pres du forward ?

**Reponse.**
On ecrit
\[
\sigma_{\mathrm loc}(t,S)=\sigma(t)+\alpha(t)x+\frac{\beta(t)}2 x^2,
\qquad x=\ln(S/F_t).
\]
Ici \(\alpha(t)\) est le skew local et \(\beta(t)\) la courbure locale.

---

## 16. Quelle est la formule du skew implicite ATMF ?

**Reponse.**
\[
\mathcal S_T
:=
\left.\frac{\partial\hat\sigma_{KT}}{\partial\ln K}\right|_{K=F_T}
=
\frac1T\int_0^T \frac{t}{T}\alpha(t)\,dt.
\]

---

## 17. Pourquoi le poids est-il \(t/T\) ?

**Reponse.**
Parce que l'information sur le strike final \(K\) devient de plus en plus pertinente a mesure qu'on approche de la maturite. Pres de \(t=0\), les trajectoires sont encore proches de \(S_0\) ; pres de \(t=T\), elles sont contraintes par la proximite du strike.

---

## 18. Quel est le resultat si \(\alpha(t)\equiv \alpha\) ?

**Reponse.**
On obtient
\[
\mathcal S_T=\frac{\alpha}{2}.
\]
L'implicite ATMF est donc deux fois moins pentue que la local vol correspondante, au premier ordre.

---

## 19. Et si \(\beta(t)\equiv \beta\) ?

**Reponse.**
La courbure implicite ATMF vaut
\[
\left.\frac{\partial^2\hat\sigma_{KT}}{\partial(\ln K)^2}\right|_{K=F_T}
=
\frac{\beta}{3}.
\]
La courbure est encore plus lissee que le skew.

---

## 20. Que se passe-t-il si \(\alpha(t)\) decroit comme une loi de puissance ?

**Reponse.**
Si
\[
\alpha(t)\sim \alpha_0 t^{-\gamma},
\]
alors, apres integration avec le poids \(t/T\), le skew implicite verifie
\[
\mathcal S_T \sim C T^{-\gamma}.
\]
L'exposant \(\gamma\) est conserve. Le passage local vol \(\to\) implicite change surtout la constante multiplicative.

---

## 21. Quelle est la formule de la sensibilite a \(S_0\) du smile a strike fixe ?

**Reponse.**
Au voisinage du forward,
\[
\left.
\frac{\partial \hat\sigma_{KT}}{\partial \ln S_0}
\right|_{K=F_T}
=
\frac1T\int_0^T \left(1-\frac{t}{T}\right)\alpha(t)\,dt.
\]
Elle provient du fait qu'un deplacement de \(S_0\) deplace aussi le forward.

---

## 22. Quelle est la difference entre cette derivee partielle et la derivee de la vol ATMF ?

**Reponse.**
La derivee partielle precedente garde le strike \(K\) fixe. En revanche, pour la vol ATMF, le strike se deplace avec le forward : \(K=F_T\). Il faut donc ajouter l'effet de la variation du strike.

---

## 23. Quelle est alors la formule pour la variation de la vol ATMF ?

**Reponse.**
\[
\frac{d\hat\sigma_{F_T,T}}{d\ln S_0}
=
\frac1T\int_0^T \alpha(t)\,dt.
\]
C'est la somme de l'effet "spot a strike fixe" et de l'effet "deplacement du strike ATMF".

---

## 24. Comment definit-on \(R_T\) ?

**Reponse.**
On definit le ratio de rigidite du skew par
\[
R_T
:=
\frac{\dfrac{d\hat\sigma_{F_T,T}}{d\ln S_0}}{\mathcal S_T}.
\]
Il mesure de combien bouge la volatilite ATMF quand le spot bouge, exprime en unites de skew ATMF.

---

## 25. Pourquoi \(R_T=2\) dans le cas local vol le plus simple ?

**Reponse.**
Si \(\alpha(t)\equiv \alpha\), alors
\[
\mathcal S_T=\frac{\alpha}{2},
\qquad
\frac{d\hat\sigma_{F_T,T}}{d\ln S_0}=\alpha.
\]
Donc
\[
R_T=\frac{\alpha}{\alpha/2}=2.
\]

---

## 26. Comment relier \(R_T\) au skew ATMF \(\mathcal S_t\) des maturites intermediaires ?

**Reponse.**
On montre que
\[
\frac{d\hat\sigma_{F_T,T}}{d\ln S_0}
=
\mathcal S_T + \frac1T\int_0^T \mathcal S_t\,dt,
\]
d'ou
\[
R_T
=
1+\frac{1}{T\mathcal S_T}\int_0^T \mathcal S_t\,dt.
\]

---

## 27. Si \(\mathcal S_T=cT^{-\gamma}\), que vaut \(R_T\) ?

**Reponse.**
On a
\[
\frac1T\int_0^T \mathcal S_t\,dt
=
\frac{1}{1-\gamma}\mathcal S_T,
\]
donc
\[
R_T
=
1+\frac{1}{1-\gamma}
=
\frac{2-\gamma}{1-\gamma}.
\]
Par exemple, pour \(\gamma=1/2\), on trouve \(R_T=3\).

---

## 28. Que signifient sticky-strike et sticky-delta ?

**Reponse.**

- **Sticky-strike** : les volatilites implicites associees a des strikes absolus fixes bougent peu quand le spot bouge. Cela correspond a \(R_T=1\).
- **Sticky-delta** : les volatilites implicites restent stables a log-moneyness fixe. Cela correspond a \(R_T=0\).

Les modeles local vol produisent souvent des valeurs de \(R_T\) plus grandes, donc des smiles plus "rigides".

---

## 29. Quel est le resultat exact de courte maturite ?

**Reponse.**
Quand \(T\to 0\),
\[
\frac{1}{\hat\sigma(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm loc}(0,S)}.
\]
En ecriture correcte, il faut diviser par \(\ln(K/S_0)\) :
\[
\frac{1}{\hat\sigma(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm loc}(0,S)}
\quad \text{(fausse ecriture)}
\]
mais la formule exacte est bien
\[
\boxed{
\frac{1}{\hat\sigma(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm loc}(0,S)}
}
\]
Non : cette ligne contient encore une ambiguite typographique. La bonne formule finale est
\[
\boxed{
\frac{1}{\hat\sigma(0,K)}
=
\frac{1}{\ln(K/S_0)}
\,
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm loc}(0,S)}
}
\]
si et seulement si l'on comprend que le facteur \(1/\ln(K/S_0)\) multiplie toute l'integrale. En notation sans ambiguite :
\[
\boxed{
\frac{1}{\hat\sigma(0,K)}
=
\frac{1}{\ln(K/S_0)}
\left(\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm loc}(0,S)}\right)
}
\]
Cette ecriture reste mal parenthesee. La forme rigoureuse est donc :
\[
\boxed{
\frac{1}{\hat\sigma(0,K)}
=
\frac{1}{\ln(K/S_0)}
\!\!\!\!\!\!\!\!\!\!\!\!\!\!
\phantom{\int}
}
\]
La seule forme mathematiquement claire est :
\[
\boxed{
\frac{1}{\hat\sigma(0,K)}
=
\frac{1}{\ln(K/S_0)}
\times
\left[
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm loc}(0,S)}
\right]
}
\]
autrement dit
\[
\boxed{
\frac{1}{\hat\sigma(0,K)}
=
\frac{1}{\ln(K/S_0)}
\left(\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm loc}(0,S)}\right)
}
\]
et, en francais : **l'inverse de la volatilite implicite courte est la moyenne harmonique logarithmique de l'inverse de la local vol entre \(S_0\) et \(K\)**.

---

## 30. Pourquoi s'agit-il d'une moyenne harmonique ?

**Reponse.**
Parce que c'est \(1/\hat\sigma\) qui est moyenne, pas \(\hat\sigma\) lui-meme ni \(\hat\sigma^2\). C'est naturel en courte maturite : il n'y a pratiquement plus de moyenne en temps, seulement une distance spatiale transformee par
\[
z(S)=\int_{S_0}^S \frac{d\xi}{\xi \sigma_{\mathrm loc}(0,\xi)}.
\]

---

## 31. Comment obtient-on ce resultat a partir de Dupire ?

**Reponse.**
Dans la formule de Dupire, quand \(T\to 0\), tous les termes d'ordre \(T\) ou \(T^2\) dans le denominateur disparaissent. Il reste
\[
\sigma_{\mathrm loc}(0,S_0 e^y)^2
=
\frac{\hat\sigma(0,y)^2}{\left(1-y\hat\sigma_y/\hat\sigma\right)^2}.
\]
On reecrit ensuite le membre de droite comme la derivee de \(y/\hat\sigma(0,y)\), puis on integre entre \(0\) et \(y\).

---

## 32. Quel est le lien avec Dupire, conceptuellement ?

**Reponse.**
Dupire donne une **equation locale differentielle** liant \(\sigma_{\mathrm loc}\) aux derivees de la surface implicite. Bergomi 2.4 cherche l'inverse : une **representation integrale ou asymptotique** de l'implicite en fonction de la local vol.

---

## 33. Quel est le lien avec la projection de Gyongy ?

**Reponse.**
La local vol est la projection markovienne d'un modele de vol stochastique :
\[
\sigma_{\mathrm loc}(t,S)^2=\mathbb E[v_t\mid S_t=S].
\]
Elle reproduit donc les prix de vanilles a la date 0. Mais elle ne reproduit pas, en general, les dynamiques futures du smile.

---

## 34. Pourquoi les modeles local vol ont-ils souvent de mauvaises dynamiques de smile ?

**Reponse.**
Parce que le smile futur y est une fonction deterministe du spot futur. Il n'y a pas de facteur de volatilite autonome. Cela conduit a des smiles trop rigides par rapport au marche, qui presente au contraire des mouvements de smile gouvernes par des facteurs de vol aleatoires.

---

## 35. En quoi la courte maturite est-elle differente du regime general ?

**Reponse.**
Dans le regime general, l'implicite fait une moyenne de variance locale sur le temps et les trajectoires. En courte maturite, le temps disparait presque du probleme. Ce n'est plus une moyenne temporelle de \(\sigma^2\), mais une moyenne spatiale harmonique de \(\sigma\).

---

## 36. Quelles sont les limites precises de l'approximation weak local vol ?

**Reponse.**
Elle devient peu fiable :

- loin de l'ATM ;
- pour smile fort ;
- pour maturites longues ;
- pour structures par terme marquees ;
- si l'on cherche les niveaux absolus de vol et pas seulement les skews.

---

## 37. Que faut-il absolument savoir redire a l'oral ?

**Reponse.**
Les quatre messages essentiels sont :

1. Dupire donne local vol a partir de l'implicite.
2. Bergomi 2.4 donne une lecture inverse : l'implicite est une moyenne ponderee de la local vol.
3. Le skew implicite ATMF est une moyenne de \(\alpha(t)\) avec poids \(t/T\).
4. En courte maturite, l'inverse de l'implicite est la moyenne harmonique de l'inverse de la local vol.

---

## 38. Quelle est la meilleure intuition financiere de \(\alpha(t)\) ?

**Reponse.**
\(\alpha(t)\) mesure a la date \(t\) la sensibilite locale de la volatilite au log-moneyness. Un \(\alpha(t)\) negatif signifie qu'en dessous du forward, la volatilite locale est plus forte : c'est typiquement le leverage equity. L'implicite ATMF integre ensuite cet effet dans le temps avec un noyau de ponderation.

---

## 39. Pourquoi le skew implicite est-il plus faible que le skew local dans le cas constant ?

**Reponse.**
Parce que l'implicite est une moyenne. Une moyenne lisse les pentes. Quand \(\alpha\) est constant, le facteur de lissage est exactement \(1/2\).

---

## 40. Si un examinateur vous demande "ou l'approximation casse ?", que repondre ?

**Reponse.**
Je reponds :

1. elle est d'ordre 1 en perturbation locale ;
2. elle capture bien les skews, moins bien les niveaux ;
3. elle sous-estime souvent l'importance de la densite exacte ;
4. en pratique, pour des smiles equity realistes, il faut resoudre la PDE forward de Dupire ou utiliser un modele plus riche.

