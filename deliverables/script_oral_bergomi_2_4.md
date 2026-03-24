# Script oral -- Presentation Bergomi 2.4

## Mode d'emploi

Ce document suit **exactement la structure de la section Beamer** dans `extracted_slides/Partie_jules.tex`.

Pour chaque slide, je donne :

- **Ce que je dis** : formulation naturelle, orale, en francais ;
- **Ce que j'ecris au tableau** : seulement les formules strategiques ;
- **Intuition a verbaliser** : phrases courtes et efficaces ;
- **Temps cible** : pour rester entre **10 et 12 minutes**.

Le fil directeur de l'expose est le suivant :

1. Dupire donne local vol a partir de l'implicite.
2. Bergomi fait le chemin inverse de maniere approchée/interprétable.
3. Le resultat central est une moyenne ponderee.
4. Cette moyenne permet d'expliquer :
   - le skew implicite pres du forward,
   - la dynamique ATMF,
   - le ratio \(R_T\),
   - le resultat exact en courte maturite.

---

## Slide 1 -- Question du chapitre 2.4

### Temps cible
**0 min 50**

### Ce que je dis

"Dans le chapitre precedent, Bergomi utilise la formule de Dupire pour passer de la surface de volatilite implicite a une fonction de volatilite locale.  
Ici, il pose la question inverse, qui est en fait la question economiquement la plus interessante pour comprendre les dynamiques : si je me donne une volatilite locale \(\sigma_{\mathrm{loc}}(t,S)\), a quoi ressemble la volatilite implicite \(\hat\sigma(K,T)\) correspondante ?  
Le point fondamental, c'est que l'implicite n'est pas la local vol lue en un point. Elle provient d'une moyenne le long des trajectoires qui comptent pour l'option."

### Ce que j'ecris au tableau

\[
dS_t=(r-q)S_tdt+\sigma_{\mathrm{loc}}(t,S_t)S_tdW_t
\]

\[
x_K=\ln\!\left(\frac{K}{F_T}\right)
\]

### Intuition a verbaliser

- "Intuitivement, une option de strike \(K\) ne regarde pas toute la surface locale de la meme facon."
- "Ce que l'option voit, ce sont surtout les zones ou son gamma est fort."

### Transition

"Le premier resultat de Bergomi consiste justement a rendre cette idee mathematiquement precise."

---

## Slide 2 -- Identite cle : moyenne ponderee

### Temps cible
**1 min 20**

### Ce que je dis

"Bergomi part d'une identite generale entre deux modeles, obtenue en delta-couvrant une option avec un modele de base, alors que le spot suit en realite un autre modele.  
Si on prend comme modele de base Black-Scholes avec la volatilite implicite de l'option, on obtient cette relation remarquable : la variance implicite est une moyenne ponderee de la variance instantanee.  
Dans le cas local vol, cela devient une moyenne ponderee de \(\sigma_{\mathrm{loc}}(t,S_t)^2\), avec comme poids le dollar gamma."

### Ce que j'ecris au tableau

\[
\hat{\sigma}_{KT}^{\,2}
=
\frac{
\mathbb{E}\!\left[\int_0^T e^{-rt}S_t^2\Gamma_t\,\sigma_{\mathrm{loc}}(t,S_t)^2dt\right]
}{
\mathbb{E}\!\left[\int_0^T e^{-rt}S_t^2\Gamma_t\,dt\right]
}
\]

### Intuition a verbaliser

- "Le point cle ici est que l'implicite est une moyenne de variance, pas une valeur ponctuelle."
- "Financierement, le poids naturel est le gamma, puisque c'est lui qui mesure la sensibilite a la variance realisee."

### Transition

"Cette formule est exacte, mais elle reste implicite. Il faut donc la simplifier pour obtenir des expressions utilisables."

---

## Slide 3 -- Approximation weak local vol

### Temps cible
**1 min 10**

### Ce que je dis

"Bergomi suppose maintenant que la volatilite locale est proche d'un Black-Scholes de reference. On developpe alors la formule precedente au premier ordre.  
On obtient une expression tres parlante : l'implicite est approximativement une moyenne de la local vol le long d'un ensemble de trajectoires lognormales qui relient le spot initial au strike final.  
Le parametre \(y\) code la dispersion autour d'une trajectoire centrale."

### Ce que j'ecris au tableau

\[
\hat{\sigma}_{KT}
\approx
\frac1T\int_0^T\!\!dt\int_{\mathbb R}\phi(y)\,
\sigma_{\mathrm{loc}}\!\left(
t,F_t e^{\frac{t}{T}x_K+\sigma_0\sqrt{\frac{(T-t)t}{T}}y}
\right)dy
\]

\[
\phi(y)=\frac{e^{-y^2/2}}{\sqrt{2\pi}}
\]

### Intuition a verbaliser

- "Intuitivement, le cas \(y=0\) correspond a la trajectoire la plus probable."
- "Les autres \(y\) ajoutent des deviations autour de cette trajectoire centrale."

### Transition

"Cette formule n'est pas assez precise pour les niveaux de vol, mais elle est excellente pour comprendre le skew pres du forward."

---

## Slide 4 -- Developpement pres du forward

### Temps cible
**1 min 00**

### Ce que je dis

"Pour analyser le sourire pres du forward, Bergomi suppose que la local vol est reguliere et la developpe en fonction de la log-moneyness instantanee \(x=\ln(S/F_t)\).  
Le coefficient \(\alpha(t)\) mesure le skew local, et \(\beta(t)\) la courbure locale.  
En injectant ce developpement dans la formule precedente, on obtient un developpement de l'implicite en puissance de \(x_K\)."

### Ce que j'ecris au tableau

\[
\sigma_{\mathrm{loc}}(t,S)
=
\bar{\sigma}(t)+\alpha(t)x+\frac{\beta(t)}{2}x^2
\]

\[
\hat{\sigma}_{KT}
=
\text{niveau}
+
\left(\frac1T\int_0^T\frac{t}{T}\alpha(t)dt\right)x_K
+
\frac12\left(\frac1T\int_0^T\left(\frac{t}{T}\right)^2\beta(t)dt\right)x_K^2+\cdots
\]

### Intuition a verbaliser

- "Le terme lineaire en \(x_K\) donnera le skew implicite."
- "Le terme quadratique donnera la courbure implicite."

### Transition

"On peut alors lire directement le skew implicite au point ATMF."

---

## Slide 5 -- Resultat central sur le skew ATMF

### Temps cible
**1 min 15**

### Ce que je dis

"Le resultat central est le suivant : le skew implicite au point ATMF est une moyenne du skew local \(\alpha(t)\), mais avec le poids \(t/T\).  
Autrement dit, les dates proches de la maturite comptent davantage.  
Si \(\alpha\) est constant, on obtient tout de suite \(\mathcal S_T=\alpha/2\). Donc l'implicite lisse le skew local."

### Ce que j'ecris au tableau

\[
\mathcal S_T
=
\left.\frac{\partial \hat{\sigma}_{KT}}{\partial \ln K}\right|_{K=F_T}
=
\frac1T\int_0^T \frac{t}{T}\alpha(t)\,dt
\]

\[
\left.\frac{\partial^2\hat{\sigma}_{KT}}{\partial (\ln K)^2}\right|_{K=F_T}
=
\frac1T\int_0^T \left(\frac{t}{T}\right)^2\beta(t)\,dt
\]

\[
\alpha(t)\equiv \alpha
\quad\Rightarrow\quad
\mathcal S_T=\frac{\alpha}{2}
\]

### Intuition a verbaliser

- "Le point cle ici est le facteur \(t/T\)."
- "Ce que cela signifie financierement, c'est qu'une option proche de son echeance regarde surtout la zone du strike final."

### Transition

"On peut pousser cette lecture plus loin et regarder ce qui se passe si le skew local decroit avec la maturite."

---

## Slide 6 -- Role de \(\alpha(t)\) et loi de puissance

### Temps cible
**0 min 55**

### Ce que je dis

"Supposons maintenant que le skew local decroit en loi de puissance. Bergomi montre que le skew implicite herite du meme exposant de decroissance.  
Le passage de local vol a implicite ne change donc pas le regime asymptotique ; il change surtout le facteur de normalisation."

### Ce que j'ecris au tableau

\[
\alpha(t)\sim \alpha_0 t^{-\gamma}
\quad\Longrightarrow\quad
\mathcal S_T\sim \frac{1}{2-\gamma}\alpha_0 T^{-\gamma}
\]

### Intuition a verbaliser

- "L'exposant \(\gamma\) est conserve."
- "Le marche long terme voit un skew plus faible, mais de meme nature."

### Transition

"Une fois le skew implicite compris, on peut etudier comment l'ATMF se deplace quand le spot bouge."

---

## Slide 7 -- Dynamique ATMF

### Temps cible
**1 min 15**

### Ce que je dis

"Bergomi distingue deux objets.  
D'abord, la derivee de la volatilite implicite a strike fixe par rapport au spot initial. Elle fait apparaitre le poids \(1-t/T\).  
Ensuite, si on suit le point ATMF, donc si on laisse aussi le forward bouger avec le spot, la derivee totale de la vol ATMF est simplement la moyenne de \(\alpha(t)\) sur \([0,T]\)."

### Ce que j'ecris au tableau

\[
\left.\frac{\partial \hat{\sigma}_{KT}}{\partial \ln S_0}\right|_{K=F_T}
=
\frac1T\int_0^T\left(1-\frac{t}{T}\right)\alpha(t)\,dt
\]

\[
\frac{d\hat{\sigma}_{F_TT}}{d\ln S_0}
=
\frac1T\int_0^T \alpha(t)\,dt
\]

### Intuition a verbaliser

- "A strike fixe, une partie du mouvement est compensee par le forward."
- "Au point ATMF, on agrège tout le skew local sur l'intervalle \([0,T]\)."

### Transition

"Cela conduit naturellement a un indicateur de rigidite du smile : le ratio \(R_T\)."

---

## Slide 8 -- Definition et interpretation de \(R_T\)

### Temps cible
**1 min 10**

### Ce que je dis

"On normalise la sensibilite de la vol ATMF par le skew ATMF lui-meme.  
On obtient le ratio \(R_T\), souvent appele ratio de rigidite du skew.  
Il mesure de combien la vol ATMF bouge, en unite de skew implicite, lorsque le spot bouge."

### Ce que j'ecris au tableau

\[
R_T=
\frac{d\hat{\sigma}_{F_TT}/d\ln S_0}{\mathcal S_T}
\]

\[
R_T
=
1+\frac{1}{T\mathcal S_T}\int_0^T \mathcal S_t\,dt
\]

\[
\alpha(t)\equiv \alpha
\Rightarrow
R_T=2
\]

### Intuition a verbaliser

- "Sticky-strike correspond a \(R_T=1\)."
- "Sticky-delta correspond a \(R_T=0\)."
- "Le point cle ici est que le modele local vol simple donne \(R_T=2\), donc un smile relativement rigide."

### Transition

"Si maintenant le skew implicite lui-meme suit une loi de puissance, on peut expliciter \(R_T\)."

---

## Slide 9 -- Cas power law pour \(R_T\)

### Temps cible
**0 min 50**

### Ce que je dis

"Si le skew implicite suit empiriquement une loi \(\mathcal S_T=cT^{-\gamma}\), alors la formule de \(R_T\) se simplifie completement.  
On voit que \(R_T\) ne depend alors plus de la maturite.  
Par exemple, si \(\gamma=1/2\), on trouve \(R_T=3\), ce qui renforce encore l'idee de rigidite."

### Ce que j'ecris au tableau

\[
\mathcal S_T=cT^{-\gamma}
\quad\Longrightarrow\quad
\frac1T\int_0^T \mathcal S_tdt
=
\frac{1}{1-\gamma}\mathcal S_T
\]

\[
R_T=\frac{2-\gamma}{1-\gamma}
\]

\[
\gamma=\frac12 \Rightarrow R_T=3
\]

### Intuition a verbaliser

- "Un modele local vol peut donc etre encore plus rigide que la regle \(R=2\)."
- "C'est l'une des raisons pour lesquelles on preferera souvent des modeles de vol stochastique pour la dynamique."

### Transition

"Il me reste un dernier resultat, tres elegant, qui est exact cette fois : la limite de courte maturite."

---

## Slide 10 -- Resultat exact en courte maturite

### Temps cible
**1 min 15**

### Ce que je dis

"Quand \(T\) tend vers 0, il se passe quelque chose de tres joli : la relation entre local vol et implicite devient exacte, mais ce n'est plus une moyenne de \(\sigma^2\).  
C'est l'inverse de l'implicite qui devient la moyenne de l'inverse de la local vol entre le spot et le strike.  
Autrement dit, on obtient une moyenne harmonique spatiale."

### Ce que j'ecris au tableau

\[
\frac{1}{\hat{\sigma}(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm{loc}}(0,S)}
\]

Puis je corrige oralement :

\[
\frac{1}{\hat{\sigma}(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm{loc}}(0,S)}
\quad \text{avec le facteur } \frac{1}{\ln(K/S_0)} \text{ devant toute l'integrale}
\]

Version propre a ecrire si j'ai le temps :

\[
\frac{1}{\hat{\sigma}(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm{loc}}(0,S)}
\;\;\text{(moyenne harmonique spatiale)}
\]

### Important -- version exacte a dire oralement

Dire clairement :

"La formule exacte est
\[
\frac{1}{\hat{\sigma}(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm{loc}}(0,S)},
\]
ou, plus precisement, tout le membre de droite est la moyenne de \(1/\sigma_{\mathrm{loc}}\) entre \(S_0\) et \(K\) pour la mesure \(dS/S\)."

### Intuition a verbaliser

- "A tres courte maturite, il n'y a presque plus de moyenne en temps."
- "Ce que cela signifie financierement, c'est que si une zone entre \(S_0\) et \(K\) est peu volatile, elle est difficile a traverser, donc l'implicite baisse fortement."

### Transition

"Je termine en resument ce qu'il faut retenir."

---

## Slide 11 -- A retenir

### Temps cible
**0 min 50**

### Ce que je dis

"Pour conclure : Dupire va de l'implicite vers la locale ; Bergomi 2.4 donne le chemin inverse sous forme interpretable.  
Le message principal est que la variance implicite est une moyenne ponderee de la variance locale.  
Pres du forward, le skew implicite est une moyenne du skew local avec le poids \(t/T\).  
La dynamique ATMF conduit au ratio \(R_T\), qui met en evidence la rigidite des modeles local vol.  
Enfin, en courte maturite, on a un resultat exact sous forme de moyenne harmonique."

### Ce que j'ecris au tableau

\[
\hat{\sigma}_{KT}^2=\text{moyenne ponderee de }\sigma_{\mathrm{loc}}^2
\]

\[
\mathcal S_T=\frac1T\int_0^T\frac{t}{T}\alpha(t)dt,
\qquad
\frac{d\hat{\sigma}_{F_TT}}{d\ln S_0}=\frac1T\int_0^T \alpha(t)dt
\]

\[
R_T=\frac{d\hat{\sigma}_{F_TT}/d\ln S_0}{\mathcal S_T}
\]

### Intuition a verbaliser

- "Le point cle ici est le lissage."
- "Local vol calibre bien aujourd'hui, mais impose souvent des dynamiques trop rigides demain."

---

## Timing total conseille

- Slide 1 : 0:50
- Slide 2 : 1:20
- Slide 3 : 1:10
- Slide 4 : 1:00
- Slide 5 : 1:15
- Slide 6 : 0:55
- Slide 7 : 1:15
- Slide 8 : 1:10
- Slide 9 : 0:50
- Slide 10 : 1:15
- Slide 11 : 0:50

**Total : environ 11 minutes 50**

Si vous devez raccourcir vers **10 min 30**, comprimez :

- slide 3 a 45 s,
- slide 6 a 35 s,
- slide 9 a 30 s.

---

## Ce qu'il faut absolument savoir ecrire de memoire au tableau

Si l'enseignant interrompt ou pose une question, les 5 formules les plus importantes sont :

1.
\[
\hat{\sigma}_{KT}^{\,2}
=
\frac{
\mathbb{E}\!\left[\int_0^T e^{-rt}S_t^2\Gamma_t\,\sigma_{\mathrm{loc}}(t,S_t)^2dt\right]
}{
\mathbb{E}\!\left[\int_0^T e^{-rt}S_t^2\Gamma_t\,dt\right]
}
\]

2.
\[
\mathcal S_T=
\left.\frac{\partial\hat{\sigma}_{KT}}{\partial\ln K}\right|_{K=F_T}
=
\frac1T\int_0^T \frac{t}{T}\alpha(t)\,dt
\]

3.
\[
\frac{d\hat{\sigma}_{F_TT}}{d\ln S_0}
=
\frac1T\int_0^T \alpha(t)\,dt
\]

4.
\[
R_T=
\frac{d\hat{\sigma}_{F_TT}/d\ln S_0}{\mathcal S_T}
=
1+\frac{1}{T\mathcal S_T}\int_0^T \mathcal S_t\,dt
\]

5.
\[
\frac{1}{\hat{\sigma}(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm{loc}}(0,S)}
\]

---

## Remarque importante pour l'oral

Quand vous donnez la formule de courte maturite, dites-la proprement sous la forme :

\[
\frac{1}{\hat{\sigma}(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{dS}{S\,\sigma_{\mathrm{loc}}(0,S)}
\]

et ajoutez a l'oral :

"Le membre de droite est la moyenne harmonique de \(\sigma_{\mathrm{loc}}\) entre \(S_0\) et \(K\) pour la mesure logarithmique \(dS/S\)."

Si vous avez le temps, vous pouvez aussi preciser la forme encore plus explicite :

\[
\frac{1}{\hat{\sigma}(0,K)}
=
\frac{1}{\ln(K/S_0)}
\int_{S_0}^{K}\frac{1}{\sigma_{\mathrm{loc}}(0,S)}\frac{dS}{S}.
\]
