# Presentation orale — section "Passage de la volatilite locale a la volatilite implicite"

Ce document est maintenant **synchronise avec les slides actuels** de `Partie_jules.tex`.

Il suit **exactement** les frames presentes dans la presentation :

1. Du local vol vers l'implicite : la question
2. Identite exacte : moyenne ponderee de variance locale
3. Approximation faible local vol
4. Smile pres du forward : parametrisation locale
5. Skew et courbure implicites : moyennes ponderees
6. Structure par terme du skew : loi de puissance
7. Maturites courtes : resultat exact
8. A retenir : Dupire, stoch vol, limites

Le but est de te donner, pour **chaque slide** :

- ce que tu dis ;
- ce que tu ecris au tableau ;
- les phrases d'intuition ;
- un temps cible.

---

## Avant la soutenance : liens de cours a maitriser

Les questions d'oral peuvent aussi porter sur le cours. Pour cette section, les ponts a connaitre sont :

1. **Mesure risque-neutre**
   - savoir expliquer pourquoi on ecrit
   \[
   dS_t=(r-q)S_t\,dt+\sigma_t S_t\,dW_t
   \]
   sous la mesure de pricing.

2. **Itô / EDP / Feynman-Kac**
   - savoir dire d'ou vient l'EDP de pricing ;
   - comprendre pourquoi un terme gamma apparait dans un P&L de couverture delta.

3. **Dupire**
   - Dupire : implicite \(\to\) local vol ;
   - ici : local vol \(\to\) implicite.

4. **Smile / skew / courbure**
   - savoir definir
   \[
   \partial_{\ln K}\hat{\sigma},\qquad \partial^2_{\ln K}\hat{\sigma}.
   \]

5. **Asymptotique courte maturite**
   - savoir expliquer pourquoi on obtient une moyenne harmonique.

---

## Slide 1 — Du local vol vers l'implicite : la question

### Temps cible
50 secondes

### Ce que je dis

"Dans cette partie, on part d'un modele de volatilite locale :
\[
dS_t=(r-q)S_t\,dt+\sigma_{\mathrm{loc}}(t,S_t)S_t\,dW_t.
\]

La formule de Dupire fait normalement le chemin de la surface implicite vers la volatilite locale. Ici, Bergomi pose le probleme inverse : si je fixe une fonction de volatilite locale, quelle volatilite implicite est produite ?

Le point important est que l'on ne cherche pas seulement un niveau d'implicite, mais une interpretation mathematique de ce qu'elle represente."

### Ce que j'ecris au tableau

\[
\sigma_{\mathrm{loc}}(t,S)\quad \Longrightarrow \quad \hat{\sigma}(K,T)
\]

### Phrases d'intuition

- "Le point cle ici est qu'on remonte de la dynamique locale vers le smile implicite."
- "Autrement dit, on cherche ce que 'voit' le marche des vanilles quand le monde sous-jacent suit une local vol."

---

## Slide 2 — Identite exacte : moyenne ponderee de variance locale

### Temps cible
1 min 15

### Ce que je dis

"Bergomi obtient d'abord une identite exacte. La variance implicite s'ecrit comme une moyenne ponderee de la variance locale :
\[
\hat{\sigma}_{K,T}^{\,2}
=
\frac{
\mathbb{E}^{\mathrm{loc}}\!\left[
\int_0^T e^{-rt} S_t^2 \Gamma_t^{BS}(\hat{\sigma}_{K,T})
\sigma_{\mathrm{loc}}^2(t,S_t)\,dt
\right]
}{
\mathbb{E}^{\mathrm{loc}}\!\left[
\int_0^T e^{-rt} S_t^2 \Gamma_t^{BS}(\hat{\sigma}_{K,T})\,dt
\right]
}.
\]

Donc l'objet exact n'est pas la vol implicite elle-meme, mais sa variance. Les poids sont des dollar gammas actualises."

### Ce que j'ecris au tableau

\[
\hat{\sigma}_{K,T}^{\,2}
=
\frac{\mathbb E\!\left[\int_0^T w_t\,\sigma_{\mathrm{loc}}^2(t,S_t)\,dt\right]}
{\mathbb E\!\left[\int_0^T w_t\,dt\right]},
\qquad
w_t=e^{-rt}S_t^2\Gamma_t
\]

### Phrases d'intuition

- "L'option ne moyenne pas uniformement la variance locale."
- "Elle donne plus de poids aux zones ou son gamma est fort."
- "Le mot important est : moyenne ponderee."

### Ce que la formule fait, en une phrase

Elle transforme une information locale instantanee, \(\sigma_{\mathrm{loc}}(t,S)\), en une quantite observable de marche, \(\hat{\sigma}_{K,T}\), en disant quelles zones de temps et de spot comptent vraiment pour l'option.

---

## Slide 3 — Approximation faible local vol

### Temps cible
1 min 10

### Ce que je dis

"L'identite exacte est belle, mais elle est implicite et difficile a manipuler. Bergomi suppose alors que la local vol est une petite perturbation autour d'une vol de reference \(\sigma_0\). On obtient :
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
\right)dy.
\]

Cette formule donne une interpretation geometrique : l'implicite est une moyenne gaussienne de la local vol le long de trajectoires intermediaires reliant \(S_0\) au strike."

### Ce que j'ecris au tableau

\[
x_K=\ln\!\left(\frac{K}{F_T}\right),\qquad
\phi(y)=\frac{e^{-y^2/2}}{\sqrt{2\pi}}
\]

\[
\hat{\sigma}_{K,T}\approx \frac1T\int_0^T\!\!\int \phi(y)\,\sigma_{\mathrm{loc}}(t,S(t,y))\,dy\,dt
\]

### Phrases d'intuition

- "Le point cle ici est qu'on passe d'une formule exacte implicite a une formule approchee lisible."
- "Si on ne garde que le chemin central \(y=0\), on obtient une image intuitive du chemin le plus probable."

### Ce que la formule fait, en une phrase

Elle remplace une formule exacte mais inutilisable directement par une formule simple qui dit, en gros, que l'implicite est une moyenne de local vol le long de chemins raisonnables entre aujourd'hui et le strike.

---

## Slide 4 — Smile pres du forward : parametrisation locale

### Temps cible
55 secondes

### Ce que je dis

"Pour etudier le smile pres du forward, on developpe la local vol en moneyness :
\[
\sigma_{\mathrm{loc}}(t,S)=\bar{\sigma}(t)+\alpha(t)x+\frac{\beta(t)}{2}x^2,
\qquad
x=\ln\!\left(\frac{S}{F_t}\right).
\]

Ici \(\alpha(t)\) est le skew local instantane, et \(\beta(t)\) la courbure locale instantanee.

L'idee de Bergomi est simple : en remplaçant cette expansion dans la formule faible local vol, on va lire directement le skew et la courbure implicites."

### Ce que j'ecris au tableau

\[
\sigma_{\mathrm{loc}}(t,S)=\bar{\sigma}(t)+\alpha(t)x+\frac{\beta(t)}{2}x^2
\]

### Phrases d'intuition

- "On linearise localement la local vol autour du forward."
- "Le parametre vraiment central pour le skew sera \(\alpha(t)\)."

### Ce que la formule fait, en une phrase

Elle decompose la local vol en trois effets simples : le niveau, la pente du smile local, et sa courbure.

---

## Slide 5 — Skew et courbure implicites : moyennes ponderees

### Temps cible
1 min 20

### Ce que je dis

"Le calcul donne alors :
\[
\left.\frac{\partial \hat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
=
\frac{1}{T}\int_0^T \frac{t}{T}\alpha(t)\,dt,
\]
et
\[
\left.\frac{\partial^2 \hat{\sigma}_{K,T}}{\partial (\ln K)^2}\right|_{K=F_T}
=
\frac{1}{T}\int_0^T \Big(\frac{t}{T}\Big)^2\beta(t)\,dt.
\]

Le message principal est que le skew implicite est une moyenne ponderee du skew local \(\alpha(t)\), avec un poids \(t/T\).

Si \(\alpha(t)\) est constant, alors on retrouve le resultat classique :
\[
\text{skew implicite ATMF}=\frac{\alpha}{2}.
\]

### Ce que j'ecris au tableau

\[
\left.\frac{\partial \hat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
=
\frac{1}{T}\int_0^T \frac{t}{T}\alpha(t)\,dt
\]

\[
\alpha(t)\equiv\alpha
\quad\Longrightarrow\quad
\text{skew implicite}=\frac{\alpha}{2}
\]

### Phrases d'intuition

- "Le smile implicite ne recopie pas directement le skew local."
- "Il en fait une moyenne temporelle ponderee."
- "Le poids \(t/T\) favorise les temps proches de la maturite."

### Ce que la formule fait, en une phrase

Elle relie directement la pente et la courbure du smile implicite aux coefficients \(\alpha(t)\) et \(\beta(t)\) de la local vol.

---

## Slide 6 — Structure par terme du skew : loi de puissance

### Temps cible
55 secondes

### Ce que je dis

"Bergomi regarde ensuite le cas ou le skew local suit une loi de puissance :
\[
\alpha(t)=\alpha_0\left(\frac{\tau_0}{t}\right)^\gamma
\quad (t>\tau_0).
\]

Dans ce cas, le skew implicite ATMF verifie a longue maturite :
\[
\left.\frac{\partial \hat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
\sim
\frac{1}{2-\gamma}\,
\alpha_0\left(\frac{\tau_0}{T}\right)^\gamma.
\]

Donc l'exposant de decroissance du skew implicite est le meme que celui du skew local."

### Ce que j'ecris au tableau

\[
\alpha(t)\sim t^{-\gamma}
\quad\Longrightarrow\quad
\text{skew implicite}\sim T^{-\gamma}
\]

### Phrases d'intuition

- "Le point cle ici est que la structure par terme de l'implicite herite de celle de \(\alpha(t)\)."
- "Le modele transporte donc directement l'information temporelle du skew local vers le skew implicite."

### Ce que la formule fait, en une phrase

Elle montre que si le skew local decroit comme une loi de puissance, alors le skew implicite decroit avec le meme exposant.

---

## Slide 7 — Maturites courtes : resultat exact

### Temps cible
1 min 15

### Ce que je dis

"Enfin, quand \(T\to 0\), Bergomi donne un resultat exact :
\[
\frac{1}{\hat{\sigma}(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm{loc}}(0,S)}.
\]

Ce n'est donc pas une moyenne arithmetique de \(\sigma_{\mathrm{loc}}\), mais une moyenne harmonique de son inverse.

C'est important parce qu'a tres courte maturite, il n'y a presque plus de moyenne temporelle : on lit une structure spatiale entre le spot initial et le strike."

### Ce que j'ecris au tableau

\[
\frac{1}{\hat{\sigma}(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm{loc}}(0,S)}
\]

### Phrases d'intuition

- "La bonne moyenne ici est harmonique, pas arithmetique."
- "Si la local vol devient tres faible sur une zone a traverser, l'implicite courte maturite doit aussi refleter cette difficulte de passage."

### Ce que la formule fait, en une phrase

Elle donne une relation exacte en maturite tres courte entre local vol et implicite, sans passer par l'approximation weak local vol.

---

## Slide 8 — A retenir : Dupire, stoch vol, limites

### Temps cible
1 min

### Ce que je dis

"Je termine par trois idees.

Premiere idee : Dupire fait le chemin de l'implicite vers la local vol ; ici on fait le chemin inverse.

Deuxieme idee : les formules d'ordre 1 sont tres utiles pour comprendre le skew, la courbure et le role de \(\alpha(t)\).

Troisieme idee : cette approximation reste une approximation. Sur des smiles actions realistes, elle n'est pas suffisante pour obtenir des niveaux absolus tres precis.

Donc le chapitre est extremement utile pour l'intuition et pour l'analyse theorique, mais il ne faut pas confondre cela avec une recette numerique parfaite."

### Ce que j'ecris au tableau

\[
\text{Dupire : } \hat{\sigma}\to \sigma_{\mathrm{loc}}
\qquad\text{et ici}\qquad
\sigma_{\mathrm{loc}}\to \hat{\sigma}
\]

### Phrases d'intuition

- "Le message central est : la local vol impose une structure au smile implicite."
- "Mais les formules weak local vol restent des formules d'ordre 1."

### Ce que cette slide fait, en une phrase

Elle replace tout le chapitre dans son vrai cadre : un outil d'analyse tres fort pour comprendre le smile, mais pas une recette numerique parfaite de marche.

---

## Timing total indicatif

- Slide 1 : 0:50
- Slide 2 : 1:15
- Slide 3 : 1:10
- Slide 4 : 0:55
- Slide 5 : 1:20
- Slide 6 : 0:55
- Slide 7 : 1:15
- Slide 8 : 1:00

Total brut : environ **8 minutes 40**.

Avec :
- transitions,
- reprise orale de certaines formules,
- une ou deux secondes de respiration entre slides,

tu arrives naturellement autour de **10 minutes**, ce qui est coherent avec la repartition d'un expose a deux.

---

## Tableau : minimum syndical a ecrire si on t'interrompt

Si tu dois absolument reduire l'ecriture au tableau, garde seulement :

1. \[
\hat{\sigma}_{K,T}^{\,2}
=
\frac{\mathbb E[\int_0^T w_t\,\sigma_{\mathrm{loc}}^2\,dt]}
{\mathbb E[\int_0^T w_t\,dt]}
\]

2. \[
\left.\frac{\partial \hat{\sigma}_{K,T}}{\partial \ln K}\right|_{K=F_T}
=
\frac{1}{T}\int_0^T \frac{t}{T}\alpha(t)\,dt
\]

3. \[
\frac{1}{\hat{\sigma}(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm{loc}}(0,S)}
\]

Ce sont les trois formules les plus defendables si on te demande le coeur mathematique du passage.
