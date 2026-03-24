# Presentation orale — Bergomi 2.4 a 2.4.6

## Objectif

Ce document est concu pour accompagner **la partie de 10 a 12 minutes** sur :

- 2.4.1 : volatilite implicite comme moyenne ponderee de volatilite locale,
- 2.4.2-2.4.4 : approximation "weakly local vol",
- 2.4.5 : skew, dynamique ATMF, role de \(\alpha(t)\), ratio \(R_T\),
- 2.4.6 : resultat exact a courte maturite.

Il est organise slide par slide.

---

## Avant la soutenance : liens explicites avec le cours a maitriser

Comme les questions porteront aussi sur le contenu du cours, il faut etre capable de rattacher Bergomi 2.4 aux themes classiques de M2 Probabilites & Finance :

1. **Mesure risque-neutre et dynamique de l'actif**
   - savoir expliquer pourquoi on ecrit
   \[
   dS_t=(r-q)S_t\,dt+\sigma_t S_t\,dW_t
   \]
   sous la mesure de pricing ;
   - savoir distinguer volatilite locale deterministe en \((t,S)\) et volatilite stochastique.

2. **EDP de pricing / Feynman-Kac**
   - savoir retrouver l'EDP
   \[
   \partial_t P+(r-q)S\partial_S P+\frac12 \sigma^2 S^2\partial_{SS}P-rP=0 ;
   \]
   - comprendre que l'identite de Bergomi repose sur Itô applique a \(e^{-rt}P(t,S_t)\).

3. **Dupire**
   - savoir rappeler que Dupire reconstruit \(\sigma_{\mathrm{loc}}(T,K)\) a partir de la surface vanille ;
   - expliquer que Bergomi regarde le probleme inverse : que devient l'implicite si on part de \(\sigma_{\mathrm{loc}}\) ?

4. **Smile, skew, curvature**
   - savoir definir rigoureusement
   \[
   \partial_{\ln K}\hat{\sigma},\qquad \partial_{\ln K\ln K}\hat{\sigma},
   \]
   et expliquer leur interpretation financiere.

5. **Dynamique du smile**
   - comprendre la difference entre :
     - mouvement a strike fixe,
     - mouvement ATMF,
     - regimes sticky-strike / sticky-delta.

6. **Asymptotiques de courte maturite**
   - savoir expliquer pourquoi \(T\to 0\) change la nature de la moyenne ;
   - comprendre la difference entre moyenne temporelle de variances et moyenne spatiale harmonique.

7. **Calibration statique vs dynamique**
   - point tres classique de cours : un modele peut calibrer la surface a \(t=0\) mais mal decrire sa dynamique future.

Si on te pose une question "de cours", les ponts les plus probables sont donc :
**Itô / Feynman-Kac / Dupire / smile dynamics / asymptotique courte maturite / calibration vs dynamique.**

---

## Slide 1 — Du local vol vers l'implicite : la question

### Temps cible
50 secondes

### Ce que je dis

"Dans cette partie, on se place dans un modele de volatilite locale, donc
\[
dS_t=(r-q)S_tdt+\sigma_{\mathrm{loc}}(t,S_t)S_tdW_t.
\]
La formule de Dupire permet de construire la volatilite locale a partir de la surface implicite. Mais Bergomi pose ici la question inverse : si je fixe une fonction de volatilite locale, quel smile implicite est produit, et surtout quelle dynamique de smile cela implique quand le spot bouge ?

L'enjeu est tres important en pratique, parce qu'un modele peut bien calibrer la surface aujourd'hui, mais produire une dynamique de smile peu realiste demain."

### Ce que j'ecris au tableau

\[
\sigma_{\mathrm{loc}}(t,S)\ \Longrightarrow\ \hat{\sigma}(K,T)
\]

### Phrases d'intuition

- "Le point cle ici est qu'on ne cherche plus a calibrer, mais a comprendre la dynamique induite."
- "Financierement, la vraie question n'est pas seulement le fit du smile aujourd'hui, mais sa reaction demain apres un mouvement de spot."

---

## Slide 2 — Identite exacte : moyenne ponderee de variance locale

### Temps cible
1 min 10

### Ce que je dis

"Bergomi commence par une identite exacte. En comparant un modele de base et un modele reel, puis en regardant le P\&L d'une couverture delta, il obtient une formule qui exprime la volatilite implicite au carre comme une moyenne ponderee de la variance locale.

La formule est :
\[
\hat{\sigma}_{K,T}^{\,2}
=
\frac{
\mathbb{E}^{\mathrm{loc}}\!\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}\,\sigma_{\mathrm{loc}}^2(t,S_t)\,dt
\right]
}{
\mathbb{E}^{\mathrm{loc}}\!\left[
\int_0^T e^{-rt}S_t^2\Gamma_t^{BS}\,dt
\right]
}.
\]

Donc l'implicite n'est pas la valeur de la local vol a un point ; c'est une moyenne de la variance locale le long des trajectoires, avec des poids de type dollar gamma."

### Ce que j'ecris au tableau

\[
\hat{\sigma}_{K,T}^{\,2}
=
\frac{\mathbb{E}\big[\int_0^T w_t\,\sigma_{\mathrm{loc}}^2(t,S_t)\,dt\big]}
{\mathbb{E}\big[\int_0^T w_t\,dt\big]},
\qquad
w_t=e^{-rt}S_t^2\Gamma_t
\]

### Phrases d'intuition

- "Intuitivement, l'option regarde surtout les regions ou son gamma est fort."
- "Ce que cela signifie financierement, c'est qu'une variance locale situee dans une zone peu visitee ou peu sensible compte peu."

---

## Slide 3 — Approximation faible local vol

### Temps cible
1 min 05

### Ce que je dis

"Cette identite exacte est tres elegante, mais elle est implicite et difficile a utiliser directement. Bergomi suppose donc que la local vol est une petite perturbation autour d'une volatilite de reference constante \(\sigma_0\).

On obtient alors une formule d'ordre 1 :
\[
\hat{\sigma}_{K,T}
\approx
\frac1T\int_0^Tdt\int_{\mathbb R}\phi(y)\,
\sigma_{\mathrm{loc}}\!\left(t,F_t\exp\!\left(\frac{t}{T}x_K+\sigma_0\sqrt{\frac{(T-t)t}{T}}y\right)\right)dy.
\]

Le point important, c'est que l'implicite devient une moyenne gaussienne de la local vol le long de trajectoires joignant \(S_0\) a \(K\)."

### Ce que j'ecris au tableau

\[
x_K=\ln(K/F_T),\qquad
S(t,y)=F_t\exp\!\left(\frac{t}{T}x_K+\sigma_0\sqrt{\frac{(T-t)t}{T}}y\right)
\]

\[
\hat{\sigma}_{K,T}\approx \frac1T\int_0^T\!\!\int \phi(y)\,\sigma_{\mathrm{loc}}(t,S(t,y))\,dy\,dt
\]

### Phrases d'intuition

- "Le point cle ici est la geometrie des trajectoires : on moyenne sur des ponts lognormaux."
- "Si je prends seulement \(y=0\), je regarde le chemin le plus probable entre le spot initial et le strike final."

---

## Slide 4 — Parametrisation locale autour du forward

### Temps cible
55 secondes

### Ce que je dis

"Pour etudier le smile pres du forward, on developpe la local vol en moneyness
\[
x=\ln(S/F_t),
\qquad
\sigma_{\mathrm{loc}}(t,S)=\bar{\sigma}(t)+\alpha(t)x+\frac{\beta(t)}{2}x^2.
\]

Ici, \(\alpha(t)\) mesure le skew local instantane et \(\beta(t)\) la courbure locale instantanee. En reinjectant ce developpement dans la formule precedente, on peut lire directement le skew et la convexite implicites."

### Ce que j'ecris au tableau

\[
\sigma_{\mathrm{loc}}(t,S)=\bar{\sigma}(t)+\alpha(t)x+\frac{\beta(t)}{2}x^2
\]

### Phrases d'intuition

- "Intuitivement, \(\alpha(t)\) est la pente locale du smile instantane."
- "Le point cle ici est que toute la dynamique du smile va etre gouvernee principalement par \(\alpha(t)\)."

---

## Slide 5 — Le skew implicite est une moyenne ponderee de \(\alpha(t)\)

### Temps cible
1 min 10

### Ce que je dis

"Le calcul donne
\[
\left.\frac{\partial\hat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
=
\frac1T\int_0^T \frac{t}{T}\alpha(t)\,dt.
\]

C'est une formule tres importante : le skew implicite ATMF est une moyenne ponderee du skew local instantane \(\alpha(t)\). Le poids n'est pas uniforme : c'est \(t/T\), donc les temps proches de la maturite comptent davantage.

Si \(\alpha(t)\) est constant, on obtient simplement
\[
\text{skew implicite ATMF}=\frac{\alpha}{2}.
\]
Donc l'implicite retient seulement la moitie du skew local constant."

### Ce que j'ecris au tableau

\[
S_T^{\mathrm{ATMF}}
:=
\left.\frac{\partial\hat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
=
\frac1T\int_0^T \frac{t}{T}\alpha(t)\,dt
\]

\[
\alpha(t)\equiv \alpha
\quad\Longrightarrow\quad
S_T^{\mathrm{ATMF}}=\frac{\alpha}{2}
\]

### Phrases d'intuition

- "Pourquoi ce poids \(t/T\) ? Parce qu'au debut de la vie de l'option, le strike terminal ne contraint presque pas encore la trajectoire."
- "A l'inverse, pres de l'echeance, atteindre le strike final devient essentiel."

---

## Slide 6 — Mouvement du smile pour un strike fixe quand le spot bouge

### Temps cible
1 min

### Ce que je dis

"Bergomi calcule ensuite la reaction de l'implicite a un mouvement du spot, a strike fixe :
\[
\left.\frac{\partial\hat{\sigma}_{K,T}}{\partial \ln S_0}\right|_{K=F_T}
=
\frac1T\int_0^T\left(1-\frac{t}{T}\right)\alpha(t)\,dt.
\]

Cette fois, les temps courts portent le plus de poids. C'est logique : quand le spot bouge aujourd'hui, l'impact se fait d'abord sentir au debut de la trajectoire."

### Ce que j'ecris au tableau

\[
\left.\frac{\partial\hat{\sigma}_{K,T}}{\partial \ln S_0}\right|_{K=F_T}
=
\frac1T\int_0^T\left(1-\frac{t}{T}\right)\alpha(t)\,dt
\]

### Phrases d'intuition

- "Le point cle ici est l'opposition entre deux poids complements : \(t/T\) pour le skew en strike, \(1-t/T\) pour la reaction au spot a strike fixe."
- "Ce que cela signifie financierement, c'est que le strike fixe et le spot ne sondent pas la meme partie de la structure temporelle du skew local."

---

## Slide 7 — Dynamique ATMF

### Temps cible
1 min 05

### Ce que je dis

"Le cas le plus important pour le marche est l'ATMF. Comme le strike ATMF verifie \(K=F_T(S_0)\), quand le spot bouge il faut tenir compte a la fois de l'effet direct sur le smile et de la translation du strike ATMF.

On obtient :
\[
\frac{d\hat{\sigma}_{F_T,T}}{d\ln S_0}
=
\left.\frac{\partial\hat{\sigma}_{K,T}}{\partial \ln S_0}\right|_{K=F_T}
+
\left.\frac{\partial\hat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
=
\frac1T\int_0^T \alpha(t)\,dt.
\]

Donc la dynamique de la volatilite ATMF depend uniquement de la structure par terme de \(\alpha(t)\), avec cette fois un poids uniforme."

### Ce que j'ecris au tableau

\[
\frac{d\hat{\sigma}_{F_T,T}}{d\ln S_0}
=
\frac1T\int_0^T \alpha(t)\,dt
\]

### Phrases d'intuition

- "Le point cle ici est que toutes les dates comptent pareil."
- "Financierement, la vol ATMF est entierement pilotee par la courbe de skew local instantane."

---

## Slide 8 — Le ratio \(R_T\)

### Temps cible
1 min 10

### Ce que je dis

"Bergomi introduit alors un ratio tres parlant :
\[
R_T
=
\frac{\dfrac{d\hat{\sigma}_{F_T,T}}{d\ln S_0}}
{\left.\dfrac{\partial\hat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}}.
\]

Il mesure combien la vol ATMF bouge en unite du skew ATMF. En remplaçant les formules precedentes, on trouve
\[
R_T=
\frac{\int_0^T \alpha(t)\,dt}
{\int_0^T \frac{t}{T}\alpha(t)\,dt}.
\]

Si \(\alpha(t)\) est constant, on obtient \(R_T=2\). C'est la fameuse regle du \(R=2\) en local vol."

### Ce que j'ecris au tableau

\[
R_T=
\frac{\int_0^T \alpha(t)\,dt}
{\int_0^T \frac{t}{T}\alpha(t)\,dt}
\]

\[
\alpha(t)\equiv \alpha \quad\Longrightarrow\quad R_T=2
\]

### Phrases d'intuition

- "Sous local vol, le smile est rigide : quand le spot baisse, la vol ATMF monte souvent trop fortement."
- "C'est justement l'une des critiques classiques des modeles de volatilite locale sur les sous-jacents equity."

---

## Slide 9 — Structure par terme du skew : loi de puissance

### Temps cible
55 secondes

### Ce que je dis

"Bergomi regarde ensuite un cas important en pratique : une structure de skew locale de type loi de puissance,
\[
\alpha(t)\sim \alpha_0\left(\frac{\tau_0}{t}\right)^\gamma.
\]
Alors le skew implicite ATMF decroit, a longue maturite, avec le meme exposant \(\gamma\) :
\[
\left.\frac{\partial\hat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
\sim
\frac{1}{2-\gamma}\alpha_0\left(\frac{\tau_0}{T}\right)^\gamma.
\]

Donc la structure par terme du smile implicite herite directement de celle de \(\alpha(t)\)."

### Ce que j'ecris au tableau

\[
\alpha(t)\sim t^{-\gamma}
\quad\Longrightarrow\quad
\text{skew implicite}\sim T^{-\gamma}
\]

### Phrases d'intuition

- "Intuitivement, l'exposant de decroissance est transmis du local vers l'implicite."
- "Le point cle ici est que \(\alpha(t)\) ne controle pas seulement le niveau du skew, mais aussi sa structure par terme."

---

## Slide 10 — Maturite courte : resultat exact

### Temps cible
1 min 15

### Ce que je dis

"Enfin, Bergomi donne un resultat exact quand \(T\to 0\). Cette fois, ce n'est plus une approximation d'ordre 1. On a
\[
\frac{1}{\hat{\sigma}(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm{loc}}(0,S)}.
\]

Donc, a tres courte maturite, l'inverse de la volatilite implicite est la moyenne harmonique de l'inverse de la local vol entre \(S_0\) et \(K\).

C'est surprenant au debut, parce qu'on est habitues a voir des moyennes de variances. Mais ici il n'y a quasiment plus de moyenne temporelle : c'est la geometrie spatiale locale qui domine."

### Ce que j'ecris au tableau

\[
\frac{1}{\hat{\sigma}(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm{loc}}(0,S)}
\]

### Phrases d'intuition

- "Le point cle ici est que la bonne moyenne a courte maturite est harmonique, pas arithmetique."
- "Si la local vol s'annule sur une zone a franchir, l'implicite doit aussi tendre vers zero : la formule le respecte naturellement."

---

## Slide 11 — Conclusion, Dupire, stochastic volatility, limites

### Temps cible
1 min

### Ce que je dis

"Je termine par trois messages.

Premier message : Dupire donne l'aller, c'est-a-dire surface implicite vers local vol ; Bergomi etudie le retour local vol vers implicite et surtout la dynamique de smile induite.

Deuxieme message : le parametre central est \(\alpha(t)\). Il controle le skew implicite, le mouvement a strike fixe, la dynamique ATMF et donc le ratio \(R_T\).

Troisieme message : meme si ces formules sont tres utiles pour l'intuition et les asymptotiques, le modele local vol reste souvent trop rigide pour les smiles equity, ce qui motive l'interet des modeles de volatilite stochastique."

### Ce que j'ecris au tableau

\[
\text{local vol} \Rightarrow \text{smile trop rigide}
\]

\[
\alpha(t)\ \text{controle}\ \text{skew, ATMF, }R_T
\]

### Phrases d'intuition

- "Ce que cela signifie financierement, c'est qu'un bon fit instantane ne garantit pas une bonne dynamique de smile."
- "La stochastic volatility est justement introduite pour assouplir cette dynamique."

---

## Timing global

- Slide 1 : 0:50
- Slide 2 : 1:10
- Slide 3 : 1:05
- Slide 4 : 0:55
- Slide 5 : 1:10
- Slide 6 : 1:00
- Slide 7 : 1:05
- Slide 8 : 1:10
- Slide 9 : 0:55
- Slide 10 : 1:15
- Slide 11 : 1:00

**Total cible : environ 10 min 45 s**

---

## Conseils de tableau

Ne pas surcharger le tableau. Ecrire seulement :

1. le SDE local vol au debut ;
2. la formule de moyenne ponderee sur \(\hat{\sigma}^2\) ;
3. la formule du skew implicite
   \[
   \left.\partial_{\ln K}\hat{\sigma}\right|_{ATMF}
   =
   \frac1T\int_0^T \frac{t}{T}\alpha(t)\,dt;
   \]
4. la formule de dynamique ATMF
   \[
   \frac{d\hat{\sigma}_{ATMF}}{d\ln S_0}
   =
   \frac1T\int_0^T \alpha(t)\,dt;
   \]
5. le ratio
   \[
   R_T=
   \frac{\int_0^T \alpha(t)\,dt}
   {\int_0^T \frac{t}{T}\alpha(t)\,dt};
   \]
6. le resultat exact a courte maturite.

---

## Si on me coupe et qu'il faut raccourcir

Supprimer ou raccourcir :

- slide 9 (loi de puissance),
- une partie du detail sur la derivation weak-local-vol,
- une partie de la conclusion.

Garder absolument :

- moyenne ponderee,
- role de \(\alpha(t)\),
- dynamique ATMF,
- \(R_T\),
- resultat court terme.
