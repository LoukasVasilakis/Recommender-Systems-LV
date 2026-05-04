# Recommender Systems - Themenliste

RSs sind ein nützliches Werkzeug, um mit Information Überfluss umzugehen. Ihre Aufgabe ist dem Nutzer neue Objekten zu zeigen, die relevant zu
seiner Aufgabe sind. Ihre Ziele sind:

- Erhöhen Sie die Anzahl der verkauften Artikel
- Verkaufen Sie vielfältigere Artikel
- Steigern Sie die Zufriedenheit Ihrer Nutzer
- Erhöhen Sie die Nutzerbindung
- Besser verstehen, was der Benutzer möchte

## Typical input data for Recommender Systems (RS)

Empfehlungssysteme basieren im Allgemeinen auf drei Hauptkategorien von Eingabedaten:

### 1) User Data

 - Benutzerprofile (Alter, Standort, usw.)
 - Explizites Feedback (Bewertungen, Likes, Rezensionen)
 - Implizites Verhalten (Klicks, Browserverlauf, Käufe)

### 2) Item Data

- Metadaten (Genre, Kategorie, Preis, Tags)
- Inhaltliche Merkmale (Text, Bilder, Produktbeschreibungen)

### 3) Interaktions- oder Transaktionsdaten

- Kaufhistorie
- Bewertungsmatrizen
- Klickstromdaten
- Anzeige-/Hörverlauf

## Rating scales

**Explizite Ratings** sind Bewertungen, die **Nutzer bewusst abgeben** wie Sternebewertungen 1-5 oder Like/Dislike-Angaben.
Der Vorteil ist, dass sie eine klare Präferenz ausdrücken, der Nachteil ist, dass viele Nutzer nur wenige Bewertungen abgeben.

**Implizite Ratings** entstehen aus dem Verhalten der Nuter, **ohne dass sie aktiv Bewertung abgeben**.
Beispiele sind Klicks, Käufe, Streaming-Dauer usw. Vorteil ist, dass diese Daten meist in grösserer Menge
vorhanden sind, der Nachteil ist, dass die keine tatsächliche Präferenz eines Nutzers zeigen.

## Types of Recommender Systems
Empfehlungssysteme unterscheiden sich hinsichtlich des verwendeten Wissens und insbesondere hinsichtlich des Empfehlungsalgorithmus.
Hohe Auszeichnung:

| Non-Personalised  | Personalised                                                                                                                          |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------|
 | Diese empfehlen jedem Nutzer dieselben Artikel, ohne individuelle Vorlieben zu berücksichtigen. | Diese generieren spezifische Empfehlungen für jeden Nutzer, indem sie Informationen über dessen Präferenzen oder Verhalten verwenden. |
| Beliebste Artikel | Content-based                                                                                                                         |
| Bestseller        | Collaborative filtering                                                                                                               |
| Aktuelle Trends | Noch weitere                                                                                                                          |

**Content based:**
Das System lernt, Artikel zu empfehlen, die denen ähnlich sind,
die dem Nutzer in der Vergangenheit gefallen haben. Zum Beispiel Wenn ein Nutzer einen Film aus dem Genre Komödie positiv bewertet hat,
kann das System lernen, andere Filme aus diesem Genre zu empfehlen.


**Zu diesem Beispiel** haben wir die Filmen; Superman v Batam (Adventure, Super-Hero), Guardians of the Galaxy (Comedy, Adventure, Super-Hero,
Sci-fi), Captain America: Civil War (Comedy, Super-Hero).

**Input User Ratings / User-Rating Matrix URM**

| Film                      | User Input |
|---------------------------|------------|
| Superman v Batam          | 2 |
 | Guardians of the Galaxy   | 10 |
 | Captain America Civil War | 8 |

$$X$$

**Movies Matrix  / Item Content Matrix ICM**

| Film | Comedy | Adventure | Super-Hero | Sci-fi |
|------|--------|-----------|------------|--------|
 | Superman v Batam | 0 | 1         | 1 | 0 |
| Guardians of the Galaxy | 1 | 1 | 1 | 1 |
 | Captain America Civil War | 1 | 0 | 1 | 0 |

$$=$$

**Weighted genre Matrix**

| Film                      | Comedy        | Adventure | Super-Hero | Sci-fi |
|---------------------------|---------------|-----|------------|--------|
 | Superman v Batam          | 0             | 2   | 2          | 0      |
| Guardians of the Galaxy   | 10            | 10  | 10         | 10     |
 | Captain America Civil War | 8             | 0   | 8          | 0      |   
 | User Profile              | 18            | 12  | 20         | 10 |
 | Normiert User Profile     | 18 / 60 = 0.3 | 0.2 | 0.33 | 0.16 |

**Candidate Matrix**

| Film                            | Comedy | Adventure | Super-Hero | Sci-fi |
|---------------------------------|--------|-----------|------------|--------|
 | Hitchhiker's guide to the galaxy | 1      | 1         | 0 | 1 |
 | Batman begins | 0      | 0         | 1 | 0 |
 | Spiderman | 1      | 0         | 1 | 0| 
 | $X$ | 0.3    | 0.2       | 0.33 | 0.16 |
 | Hitchhiker's guide to the galaxy | 0.3    | 0.2       | 0 | 0.16 |
 | Batman begins | 0      | 0         | 0.33 | 0 |
 | Spiderman | 0.3    | 0         | 0.33 | 0 |

**Recommender Matrix**

| Film | Weighted Aggregate      | User Rating Vorhersage |                      
|------|-------------------------|------------------------------------|
|Hitchhiker's guide to the galaxy | 0.3 + 0.2 + 0.16 = 0.66 | 6.6 |
 | Batman begins | 0.33                    | 3.3 |
 | Spiderman | 0.63                    | 6.3 |

Anhand dieser Technik das System würde diesem User Hitchhiker's guide to the galaxy empfehlen,
aber ein Film aus einem Genre wie Drama wäre nie empfohlen. 

**Collaborative filtering:**
Dieses Problem kann mit einem anderen System gelöst werden.
**Collaborative filtering**, basiert nicht auf den Eigenschaften der Filme (Genres, Features), sondern auf dem Verhalten vieler Nutzer.

Die Grundidee ist, dass Nutzer mit ähnlichem Bewertungsverhalten vermutlich auch in Zukunft ähnliche Filme mögen.
Das System sucht daher nach anderen Nutzern mit ähnlichen Bewertungen und verwendet deren Präferenzen, um neue Empfehlungen zu generieren.

**Zu diesem Beispiel** haben wir eine 5 Users mit ähnlinchen "Kauf-/Filmgeschichte":

| User | Superman v Batman | Titanic | Twilight | Spiderman | Batman begins | Schindler's list |
|------|-------------------|---------|----------|-----------|---------------|------------------|
 | A    | 0                 | 1       | 5        | 0         | 0             | 0                |
 | B    | 4                 | 3       | 0        | 0         | 5             | 0                |
| C    | 5                 | 5       | 1        | 3         | 4             | 0                |
 | D    | 2                 | 0       | 0        | 0         | 3             | 5                |
 | E | 0                 | 3       | 1        | 0         | 0             | 4                |
 | F | 0                 | 4       | 0        | 4         | 0             | 5                |

Um die Ähnlichkeit zwischen den Filmen zu berechnen können wir verschiedene Methoden verwenden:

**Cosine Similarity** vergleicht den Winkel zwischen zwei Bewertungsvektoren. Fehlende Werte werden nicht durch Nullen ersetzt;
stattdessen wird die Ähnlichkeit nur für die Teilmenge der Benutzer (oder Elemente) berechnet, die beide Entitäten bewertet haben.


Superman v Batam: $ i = (0,4,5,2,0,0)$

Titanic: $ j = (1,3,5,0,3,4) $ und so weiter

$$ sim = \frac{i * j}{||i||*||j||}  $$

1) Skalarprodukt zwischen den zwei Filmvektoren berechnen:

$i * j = 0 * 1 + 4 * 3 + 5 * 5 + 2 * 0 + 0 * 3 + 0 * 4 = 37$

2) Normen berechnen

$ ||i|| = \sqrt{0^2+4^2+5^2+2^2+0^2+0^2} = 3\sqrt{5}~~,~~ ||j|| = \sqrt{1^2+3^2+5^2+0^2+3^2+4^2} = 2\sqrt{15}$

3) Similarity berechnen

$ sim = \frac{ 37}{3\sqrt{5} *2\sqrt{15}} = \frac{37\sqrt{3}}{90} \approx 0.71$

Nachdem dieses Verfahren zwischen allen Vektoren berechnet wurde, kann die **Similarity Matrix** gebildet werden:

| Film      | Superman v Batman | Titanic | Twilight | Spiderman | Batman begins | Schindler's list |
| --------- | ----------------- | ------- | -------- | --------- | ------------- | ---------------- |
| Superman v Batman | 1.00              | 0.71    | 0.14     | 0.45      | 0.97          | 0.18             |
| Titanic   | 0.71              | 1.00    | 0.32     | 0.80      | 0.64          | 0.51             |
| Twilight  | 0.14              | 0.32    | 1.00     | 0.12      | 0.11          | 0.09             |
| Spiderman | 0.45              | 0.80    | 0.12     | 1.00      | 0.34          | 0.49             |
| Batman begins | 0.97              | 0.64    | 0.11     | 0.34      | 1.00          | 0.26             |
| Schindler's list | 0.18              | 0.51    | 0.09     | 0.49      | 0.26          | 1.00             |

So sehen wir zum Beispiel, dass zwei Filmen aus unterschiedlichen Filmgenren wie Spiderman und Titanic
haben eine starke similarity und das System wird das berücksichtigen.

Die **Pearson-Korrelation** reduziert bestimmte Bewertungs-Biases, indem sie die Bewertungen zunächst um ihren Mittelwert zentriert.
Dadurch wird nicht die absolute Bewertung verglichen, sondern die relative Abweichung vom Durchschnitt.
So können ähnliche Präferenzmuster erkannt werden, auch wenn Nutzer oder Filme **unterschiedliche durchschnittliche Bewertungsniveaus aufweisen**.

$$ sim(i,j) = \frac{ \sum^n_{u=1}(r_{u,i}-\bar{r_i})(r_{u,j}-\bar{r_j})}{\sqrt{\sum^n_{u=1}(r_{u,i}-\bar{r_i})^2}*\sqrt{\sum^n_{u=1}(r_{u,j}-\bar{r_j})^2}} $$

- $r_{u,i}$: Bewertung von User $u$ für Film $i$
- $r_{u,j}$: Bewertung von User $u$ für Film $j$
- $\bar{r_i}$: Durschschnittsbewertung von Film $i$
- $\bar{r_j}$: Durschschnittsbewertung von Film $j$

Wie zuvor verwenden wir die Filmvektoren Superman v Batman: $i=(0,4,5,2,0,0)$, Titanic: $j=(1,3,5,0,3,4)$

1. Mittelwerte berechnen und Vektoren zentrieren
$$ \bar{r_i} = \frac{0+4+5+2+0+0}{6} \approx \underline{1.83}~~,~~\bar{r_j} = \frac{1+3+5+0+3+4}{6} \approx \underline{2.67} $$
$$ i - \bar{r_i} = (-1.83, 2.17, 3.17, 0.17, -1.83, -1.83)~~,~~ j-\bar{r_j} = (-1.67, 0.33, 2.33, -2.67, 0.33, 1.33) $$

2.  Zähler berechnen $\sum^6_{u=1}(r_{u,i}-\bar{r_i})(r_{u,j}-\bar{r_j})$

$$ (-1.83 * -1.67) + (2.17 * 0.33) + (3.17 * 2.33) + (0.17 * -2.67) + (-1.83 * 0.33) + (-1.83 * 1.33) \approx \underline{7.67}$$ 

3. Nenner berechnen $\sum^6_{u=1}(r_{u,i}-\bar{r_i})^2~~,~~\sum^6_{u=1}(r_{u,j}-\bar{r_j})^2$

$$ \sum^6_{u=1}(r_{u,i}-\bar{r_i})^2 = ((-1.83)^2+2.17^2+3.17^2+0.17^2+(-1.83)^2+(-1.83)^2) = \underline{24.83} $$
$$ \sum^6_{u=1}(r_{u,j}-\bar{r_j})^2 = ((-1.67)^2+0.33^2+2.33^2+(-2.67)^2+0.33^2+1.33^2) = \underline{17.33}$$

4. Pearson korrelation berechnen

$$ sim(i,j) = \frac{7.67}{\sqrt{24.83} * \sqrt{17.1045}} = 0.37$$

## Model-based RS: SVD (Singular Value Decomposition)

Die KNN-basierten Collaborative-Filtering-Methoden gehören zu den **Memory-based** Verfahren: Sie speichern die gesamte User-Rating-Matrix und suchen zur Vorhersagezeit ähnliche Nutzer oder Filme. SVD ist hingegen ein **Model-based** Verfahren. Anstatt direkt mit der grossen, dünn besetzten Matrix zu arbeiten, lernt das Modell eine **kompakte Repräsentation** in nur wenigen latenten Dimensionen (*latent factors*). Diese Faktoren entsprechen **keinen** expliziten Genres wie "Komödie" oder "Action", sondern werden automatisch gelernt — sie können Konzepte wie "alt vs. neu", "düster vs. fröhlich" oder beliebige Mischungen davon kodieren.

### Grundidee der Matrixfaktorisierung

Die **User-Rating-Matrix R** (Grösse $m \times n$, also Nutzer × Filme) soll durch das Produkt zweier kleinerer Matrizen approximiert werden:

$$ R \approx P \cdot Q^T $$

- $P$: User-Faktor-Matrix ($m \times k$), jede Zeile ist ein Nutzer-Vektor $p_u$
- $Q$: Item-Faktor-Matrix ($n \times k$), jede Zeile ist ein Film-Vektor $q_i$
- $k$: Anzahl latenter Faktoren (Hyperparameter `n_factors`)

Da typischerweise $k \ll m, n$ gilt, wird die ursprüngliche Information **komprimiert**.

**Zu diesem Beispiel** haben wir 4 Nutzer und 4 Filme:

| User | Inception | Titanic | Avengers | Notebook |
|------|-----------|---------|----------|----------|
| A    | 5         | 1       | 5        | 1        |
| B    | 4         | 2       | 4        | 2        |
| C    | 1         | 5       | 1        | 5        |
| D    | 2         | 4       | 2        | 4        |

Hier sind klar zwei Muster zu erkennen: A und B mögen Action (Inception, Avengers), C und D mögen Drama (Titanic, Notebook). Mit $k = 2$ kann SVD die Matrix etwa so zerlegen:

**Mathematisches Vorgehen**

### 1. Ausgangsmatrix R

$ R =
[[5, 1, 5, 1],
 [4, 2, 4, 2],
 [1, 5, 1, 5],
 [2, 4, 2, 4]] $

### 2. Annahme: Matrix-Faktorisierung

Wir approximieren:

$R ≈ P · Q^T$

mit:
- $P$: User × Faktoren (4×2)
- $Q$: Item × Faktoren (4×2)

### 3. Gewählte latente Faktoren

Faktor 1 = "Action"  
Faktor 2 = "Drama"

**P (User-Faktoren)**

### 4. User-Faktoren (P)


| User | Faktor 1 ("Action") | Faktor 2 ("Drama") |
|------|---------------------|--------------------|
| A    | 2.2                 | 0.4                |
| B    | 1.8                 | 0.7                |
| C    | 0.4                 | 2.2                |
| D    | 0.7                 | 1.8                |

### 5. Item-Faktoren (Q)

**Q (Item-Faktoren)**

| Film      | Faktor 1 ("Action") | Faktor 2 ("Drama") |
|-----------|---------------------|--------------------|
| Inception | 2.2                 | 0.3                |
| Titanic   | 0.3                 | 2.2                |
| Avengers  | 2.2                 | 0.3                |
| Notebook  | 0.3                 | 2.2                |


### 6. Rekonstruktion: R_hat = P · Q^T

Formel für einzelne Einträge:

$\hat{r}(u, i) = p_u · q_i$

### 7. Beispielrechnung (User A, Film Inception)

$p_A$ = (2.2, 0.4)
$q_Inception$ = (2.2, 0.3)

$\hat{r}$ = 2.2*2.2 + 0.4*0.3
      = 4.84 + 0.12
      = 4.96 ≈ 5

### 8. Interpretation

- Hoher Wert in Faktor 1 → Action-Präferenz
- Hoher Wert in Faktor 2 → Drama-Präferenz

Beispiel:
- User A: (2.2, 0.4) → bevorzugt Action
- User C: (0.4, 2.2) → bevorzugt Drama

Die Vorhersage für User A und Inception ist das Skalarprodukt der zugehörigen Vektoren:

$$ \hat{r}_{A,Inception} = p_A \cdot q_{Inception}^T = 2.2 \cdot 2.2 + 0.4 \cdot 0.3 = 4.84 + 0.12 \approx 4.96 $$

Das stimmt sehr gut mit dem tatsächlichen Wert $5$ überein.

### Funk-SVD (Surprise-Implementierung)

Die klassische lineare-Algebra-SVD funktioniert nur auf **voll besetzten** Matrizen. Da die User-Rating-Matrix aber **stark sparse** ist (im MovieLens-Datensatz ca. 98%), wird in der Praxis die **Funk-SVD** verwendet (von Simon Funk während des Netflix-Prize 2006 entwickelt). Diese erweitert die Vorhersageformel zusätzlich um Bias-Terme:

$$ \hat{r}_{u,i} = \mu + b_u + b_i + p_u \cdot q_i^T $$

- $\mu$: globaler Durchschnitt aller Bewertungen
- $b_u$: User-Bias (bewertet dieser Nutzer generell höher/niedriger?)
- $b_i$: Item-Bias (wird dieser Film generell höher/niedriger bewertet?)
- $p_u \cdot q_i^T$: Skalarprodukt der latenten Vektoren (das eigentliche "SVD-Stück")

Die Parameter $b_u, b_i, p_u, q_i$ werden mit **Stochastic Gradient Descent (SGD)** **nur auf den beobachteten Bewertungen** gelernt — fehlende Werte werden also nicht durch Nullen ersetzt, sondern einfach ignoriert. Optimiert wird die regularisierte quadratische Fehlerfunktion:

$$ \min \sum_{(u,i) \in R}(r_{u,i} - \hat{r}_{u,i})^2 + \lambda(b_u^2 + b_i^2 + ||p_u||^2 + ||q_i||^2) $$

Der Regularisierungsterm $\lambda$ verhindert Overfitting, indem zu grosse Parameterwerte bestraft werden — konzeptuell ähnlich zum **Damping-Effekt** weiter unten.

**Vorhersage-Beispiel mit Bias**

Angenommen, der globale Mittelwert sei $\mu = 3.5$, der User-Bias von A sei $b_A = 0.2$ und der Item-Bias von Inception sei $b_{Inception} = 0.4$. Damit das Resultat im Bewertungs-Bereich bleibt, sind die latenten Vektoren in der Praxis **deutlich kleiner** als oben, z. B. $p_A = (0.5, 0.1)$ und $q_{Inception} = (0.6, 0.05)$:

$$ \hat{r}_{A, Inception} = 3.5 + 0.2 + 0.4 + (0.5 \cdot 0.6 + 0.1 \cdot 0.05) = 3.5 + 0.2 + 0.4 + 0.305 \approx 4.41 $$

Die latenten Vektoren modellieren also nur die **Abweichung** vom Bias-Modell ($\mu + b_u + b_i$), und das Ergebnis bleibt im erlaubten Wertebereich (z. B. 0.5 - 5.0).

### Vor- und Nachteile

- **Vorteile:** Geht sehr gut mit Sparsity um, da nur beobachtete Werte verwendet werden. Komprimiert die Information in $k$ Dimensionen, wodurch Vorhersagen schnell sind (nur ein Skalarprodukt). Kann generalisieren und auch für User-Item-Paare Vorhersagen treffen, für die KNN keine Nachbarn finden würde.
- **Nachteile:** Latente Faktoren sind **nicht direkt interpretierbar** (Black-Box). Das **Cold-Start-Problem** bleibt: für komplett neue User oder Items kann kein Vektor gelernt werden. Die Wahl von $k$ ist ein wichtiger Hyperparameter, der optimiert werden muss (zu klein → Underfitting, zu gross → Overfitting + längere Trainingszeit).



## Missing as **Random** gegen Missing as **Negative**

Fehlende Bewertungen oder Interaktionen treten häufig auf. Annahmen zur Interpretation dieser fehlenden Daten sind **Missing as Random**
und **Missing as Negative** (wie oben gemacht).

Bei der **Missing as Random (MAR)** Annahme wird davon ausgegangen, dass fehlende Bewertungen zufällig fehlen und keine direkte Information
über die Präferenzen eines Nutzers enthalten. Ein nicht bewertetes Item bedeutet also nicht automatisch, dass der Nutzer es nicht mag. **Also 
fehlende Bewertungen werden nicht mit einem 0 erstetzt und werden nicht in die Ähnlichkeitsberechnung einbezogen.**

Bei der **Missing as Negartive** Annahme hingegen werden fehlende Interaktionen als impliziertes negatives Feedback interpretiert. Ein nicht bewertet
, nicht angeklickt Item, interessiert dem User nicht. **Also fehlende Bewertungen werden mit einem 0 ersetzt und werden in die Ähnlichkeitsberechnung einbezogen.**

## Evaluation Metrics

**Mean Squared Error (MSE)** wird verwendet, wenn ein Empfehlungssystem User-Ratings für Filme vorhersagt.
Die Metrik misst den durchschnittlichen quadratischen Unterschied zwischen den vorhergesagten Ratings und den tatsächlichen User-Ratings.
Dadurch kann bewertet werden, wie genau das Modell die Präferenzen der Nutzer vorhersagt:

$$ \frac{1}{n} \sum^n_{i=1}(y_i-\hat{y_i})^2$$
- $y_i$: tatsächlicher Wert (z.B. echtes User-Rating)
- $\hat{y_i}$: Vorhersage


- Vorteile: Die Formel ist leicht zu implementieren und zu berechnen und die Funktion ist einfach zu differenzieren, deswegen kann sie gut in Optimierungsalgorithmen wie Gradient Descent verwendet werden.
- Nachteile: Da Fehler quadriert werden, können einzelne grosse Fehler das Ergebnis stark verzerren.

**Accuarcy and Recall**, wenn das System Items empfiehlt statt Ratings vorherzusagen.
Um Precision und Recall zu verstehen, muss man drei Mengen unterscheiden:

- Empfohlene Items (recommended items): Das sind alle Filme, die das Recommender-System dem Nutzer vorschlägt.
- Relevante Items (relevant items): Das sind Filme, die der Nutzer tatsächlich gekauft / hoch bewertet hat.
- Relevante empfohlene Items (relevant recommended items): Das sind die Filme, die sowohl empfohlen wurden als auch tatsächlich relevant sind.

**Accuracy** Wie viele der Items waren korrekt empfohlen?
$$Accuracy = \frac{relevante~~empfohlene~~Items}{empfohlene~~Items} $$

**Recall** Wie viele der guten Items hat das System überhaupt entdeckt?
$$ Recall = \frac{relevante~~empfohlene~~Items}{relevante~~Items}$$

- Vorteile: Diese Metriken funktionieren gut, wenn das System eine Liste von empfohlenen Items (z.B. Top-10 Filme) erstellt.
- Nachteile: Die Metriken unterscheiden nicht, wie sehr ein Nutzer ein Item mag, sondern nur, ob es relevant ist oder nicht.

**Novelty** in RS beschreibt wie viel "Unbekannt" oder unfamiliär sind die empfohlenen Objekte dem Nützer.

- Life-level novelty: Objekten die, der Nützer has nie in seinem Leben "getroffen" hat, das ist die strengste
Definition von Novelty, aber schwierig zu messen.
- System-level novelty: Objekten die, der Nützer innerhalb der Daten des Systems nie "getroffen" hat,
abgeleitet aus ihren vergangenen Interaktionen oder ihrem Konsumverlauf.
- Recommendation list-level novelty: Konzentriert sich darauf, wiederholte oder überflüssige Elemente in der Empfehlungsliste zu vermeiden.

$$R_u \cap I_u = \emptyset$$

- $R_u$: Liste der empfohlenen Objekten
- $I_u$: Liste der Objekten des Nützers
- $I$: Liste aller Objekten im System

**Diversity** in RS sorgt um die Diversität der Objekten in der Liste der empfohlenen Objekten.
$$ div = 1 - \frac{i * j}{||i||*||j||} $$

**Unexcpectidness** in RS bezieht sich darauf, wie stark die empfohlenen Elemente von den Erwartungen des Benutzers abweichen,
wodurch ein Überraschungseffekt entsteht, indem offensichtliche oder vorhersehbare Vorschläge vermieden werden.

**Serendipity** bezieht sich in RS auf die Bereitstellung nützlicher und relevanter Elemente, die für den Benutzer sowohl unerwartet
als auch angenehm überraschend sind.
 $$ ser_u = \frac{|(R_u \ E_u) \cap Rel_u |}{R_u} $$

- $E_u$: erwartet Objekten
- $Rel_u$: relevante Objekten (User rating > Threshhold rating)

**Coverage** misst wie viele von den Objekten im System kann das RS empfehlen.
$ cov = \frac{|I_p|}{I} $

- $I_p$: Objekten, die das RS empfehlen kann


## Evaluation Methods

**Online Methode**:
Einfach sehen, ob der User "gekauft" oder "gecklikt" hat, was die Methode empfohlen hat.


**Offline Methode**:
Offline Methoden sind wichtig, weil wir nur dieversprechende Algorithmen in teueren so Online-Methoden testen wollen.

### All but $n$ approach
Der *All-but-$n$*-Ansatz ist ein Evaluationsverfahren, bei dem für jeden Nutzer nur $n$ Interaktionen als Eingabe beibehalten werden,
während alle übrigen Interaktionen ausgeblendet und als Ground Truth zur Bewertung der Empfehlungsleistung verwendet werden.

Typishe wahl für die Hyper-Parameter $n$:
- 5 - 10 → Cold-Start Simulation
- 10 - 20 → Stabiler Evaluation



## Community-based RS

Es gibt Hinweise darauf, dass Menschen sich eher auf Empfehlungen ihrer Freunde verlassen als auf Empfehlungen 
von ähnlichen, aber anonymen Personen. Die Ähnlichkeitsmasse oder Aggregationsmethoden von Collaborative- und Community-based RSs
können gleich sein (Cosine Similarity, Pearson-Korrelation), aber sie unterscheiden sich in der **Struktur der Datenaggregation**.
Diese RS-Modelle sammeln Informationen über die sozialen Beziehungen der Nutzer
und bilden aus diesen Gemeinschaften Nutzerbewertungsvektoren. Ein Problem dieses Systems ist die **Cold-start Situation**, 
d.h. wenn die Nutzer nicht genügend Bewertungen abgegeben haben, um die Ähnlichkeit mit anderen Nutzern zu berechnen.

## Kontext

**Context-Aware Collaborative Filtering (CARS)** erweitert klassische Collaborative-Filtering-Methoden, indem zusätzlich zum User und Item
auch Kontextinformationen in den Empfehlungsprozess einbezogen werden. Kontext kann z. B. Zeit, Ort, Stimmung, Begleitung oder Gerät sein. Ziel ist es,
Empfehlungen zu geben, die besser zur aktuellen Nutzungssituation passen.

Es gibt drei grundlegende Ansätze, Kontext in Recommender Systems zu integrieren:

1. **Reduction-based (Pre-filtering)**
Beim Pre-Filtering werden nur die Daten verwendet,
die zum aktuellen Kontext passen. Das bedeutet, dass vor der Empfehlungserstellung
der Datensatz gefiltert wird. Beispielsweise könnten nur Bewertungen genutzt werden,
die im gleichen Kontext entstanden sind (z. B. Filme, die Nutzer am Wochenende bewertet haben).
Anschliessend wird ein normales Collaborative-Filtering-Verfahren auf diesem reduzierten Datensatz angewendet.

2. **Contextual Post-filtering**
Beim Post-Filtering wird das Empfehlungssystem zunächst ohne Kontext ausgeführt.
Erst nachdem eine Liste von Empfehlungen erstellt wurde, wird der Kontext verwendet,
um diese Liste zu filtern oder neu zu gewichten. Dadurch bleiben nur die Empfehlungen übrig,
die im aktuellen Kontext relevant sind.

3. **Contextual Modeling**
Dieser Ansatz integriert den Kontext direkt in das Vorhersagemodell.
Kontext wird dabei als zusätzlicher Faktor neben User und Item in die Modellierung aufgenommen (z. B. als zusätzliche Dimension in einer Matrix oder in faktorisierungsbasierten Modellen).
Dadurch kann das Modell lernen, wie sich Nutzerpräferenzen abhängig vom Kontext verändern. Dieser Ansatz gilt als der komplexeste, aber auch leistungsfähigste.

## Damping effect

Der Damping-Effekt (auch Regularisierung oder Shrinking) ist ein wichtiges Konzept in Recommender Systems, insbesondere im Collaborative Filtering.
Er adressiert ein zentrales Problem: unzuverlässige Schätzungen bei wenigen Datenpunkten, also zum Beispiel Filmen mit einem 5/5 Rating, die eigentlich
nur 6-7 Reviews haben. Der Damping effect pennalisiert, den Film mit sehr wenig Reviews, damit er nicht über einen Film mit 4/5 Rating und 500 Reviews empfehlen wird.

### Damping auf Similarity:

$$ sim_{damped}(i,j) = \frac{n_{ij}}{n_{ij} + \lambda} *sim(i,j) $$

- $sim(i,j)$: ursprüngliche Ähnlichkeit (z.B. Cosine oder Pearson)
- $n_{ij}$: Anzahl gemeinsamer Bewertungen
- $lambda$: Dämpfungsparameter (Hyperparameter)

**Beispiel**

| Fall | $sim(i,j)$ | $n_{i,j}$ | $\lambda = 10$           | Gedämpfte Similarity |
|------|------------|-----------|--------------------------|---------------------|
 |Wenig Daten| 1.0 | 2 | $\frac{2}{12} = 0.17$    | $1 * 0.17 = 0.17$   |
| Viele Daten | 0.8 | 100 | $\frac{100}{110} = 0.91$ | $0.8 * 0.91 = 0.73$ |

**Interpertration**

Die Ähnlichkeit zwischen zwei Filmen wird gedämpft, **Richtung 0** gezogen, da die zwei Filmen nur wenig gemeinsame Daten (Reviews) haben.
Dadurch entstehen **robuster Nachbarschaften**, die bessere Empfehlungen sind.

### Damping bei Rating

$$ \hat{r} = \frac{ \bar{r_i} * n_i + \lambda * \mu}{n_{i} + \lambda} $$

- $\hat{r_i}$: gedämpfte Durchschnittbewertung
- $\bar{r_i}$: Durschnittbewertung für Film $i$
- $n_i$: Anzahl Bewertungen für Film $i$
- $\mu$: globaler Durschsnitt aller Filmen, kann auch globaler Durschsnitt pro Genre sein
- $\lambda$: Dämpfungsparameter (Hyperparameter)

**Beispiel**

| Fall        | Durchschnitt $ \bar{r}_i$ | $n_i$ | $\lambda = 10$    | Gedämpfte Bewertung $\hat{r}_i$                                      |
| ----------- | ------------------------- | ----- | ----------------- | -------------------------------------------------------------------- |
| Wenig Daten | 9.0                       | 2     | $\frac{2}{12}$    | $\frac{2 \cdot 9 + 10 \cdot 5}{12} = \frac{18 + 50}{12} = 5.67$      |
| Viele Daten | 8.0                       | 100   | $\frac{100}{110}$ | $\frac{100 \cdot 8 + 10 \cdot 5}{110} = \frac{800 + 50}{110} = 7.73$ |

**Interpretation**

Die Durschschnittsbewertung eines Films wird gedämpft, **Richtung globalen mittel $\mu$** gezogen, da er nur wenig Bewertungen hat.
Dadurch entstehen stabiler Vorhersagen über wie beliebt ist ein Film.


## Dataset partitioning (training / validation split)

Bei der Erstellung eines RS soll der Datensatz in zwei Teile aufgeteilt werden.
Der **Trainingdatensatz** wird verwendet, um das Modell zu trainieren und die Parameter zu lernen.
Der **Validierungsdatensatz** wird anschlieesend genutzt, um die Leistung des Modells auf
bisher ungesehenen Daten zu überprüfen und Hyperparameter zu optimieren.

### Cross validation

A technique to evaluate a model more reliably. Instead of splitting the data once into train/validation,
we repeat this process multiple times. This ensures:
- every user is evaluated multiple times
- the model is not dependent on one lucky/unlucky split
- results are more robust and generalizable


