# Recommender Systems – Mini Challenge FS26

Dieses Repository enthält meine Lösung der Mini Challenge im Modul **Recommender Systems (RSY)** im Frühlingssemester 2026. Ziel der Challenge ist es, ausgehend vom **MovieLens-Datensatz** mehrere Empfehlungssysteme von Grund auf zu implementieren, sie systematisch zu evaluieren, ihre Hyperparameter zu optimieren und die Verfahren am Ende auf einem zurückgehaltenen Testset gegeneinander zu vergleichen.

Neben dem ausführbaren Notebook `RSY_MiniChallenge_FS26.ipynb` liegt dem Projekt eine theoretische Zusammenfassung (`Theorie.md`) bei, in der die wichtigsten Konzepte rund um Recommender Systems aufgearbeitet sind und die als Referenz für die im Notebook umgesetzten Verfahren dient.

## Inhalt und Zielsetzung

Recommender Systems (RS) sind ein nützliches Werkzeug, um mit Information Overflow umzugehen. Ihre Aufgabe ist es, dem Nutzer neue Objekte zu zeigen, die für seine Aufgabe relevant sind. Typische Ziele sind die Erhöhung der Anzahl verkaufter Artikel, das Ausspielen vielfältigerer Artikel, eine höhere Nutzerzufriedenheit, eine stärkere Nutzerbindung und ein besseres Verständnis dafür, was der Nutzer eigentlich möchte.

In diesem Projekt werden die folgenden Bausteine eines RS abgedeckt: explorative Analyse des MovieLens-Datensatzes, Aufbau eines Baseline-Predictors, mehrere Varianten von Collaborative Filtering (item- und user-basiert mit Cosine- und Pearson-Ähnlichkeit, jeweils zentriert und nicht zentriert), ein modellbasiertes Verfahren mit Funk-SVD (`surprise`-Bibliothek) sowie kreative Erweiterungen wie ein Actor-Boost-Modell und eine MMR-basierte Diversitäts-Re-Ranking-Variante.

## Datensatz

Verwendet wird der frei verfügbare **MovieLens "ml-latest-small"**-Subset. Dieser Datensatz beschreibt 5-Sterne-Bewertungen und Free-Text-Tags eines Movie-Recommendation-Service und enthält 100'836 Bewertungen sowie 3'683 Tag-Anwendungen über 9'742 Filme von 610 Nutzern, erstellt zwischen dem 29. März 1996 und dem 24. September 2018.

Der Datensatz besteht aus drei Hauptkomponenten: `movies` (Filme mit `movieId`, Titel und Genres), `ratings` (Bewertungen pro `userId`/`movieId`) und `tags` (von Nutzern vergebene Schlagwörter). Zusätzlich wird ein selbst erstellter Datensatz `movies_actors.csv` verwendet, der pro Film bis zu fünf bekannte Schauspieler enthält und vom Actor-Boost-Modell genutzt wird.

Ein zentrales Merkmal des Datensatzes ist seine **Sparsity** von rund 98.3 %, also der Anteil fehlender Einträge in der User-Item-Matrix. Diese Eigenschaft motiviert viele der getroffenen Designentscheidungen, etwa den Einsatz von Funk-SVD oder die Annahme *Missing as Random* (fehlende Bewertungen werden nicht als implizite Negativ-Signale interpretiert).

## Theoretische Grundlagen

Die Datei `Theorie.md` fasst die theoretische Basis hinter den implementierten Verfahren zusammen. Behandelt werden typische Eingabedaten (User-, Item- und Interaktionsdaten), explizites versus implizites Feedback, der Unterschied zwischen personalisierten und nicht-personalisierten Empfehlungen, Content-based Filtering anhand einer Item-Content-Matrix mit gewichtetem User-Profil sowie Collaborative Filtering mit Cosine-Similarity und Pearson-Korrelation. Ergänzt wird dies durch Model-based-Ansätze (klassische SVD und Funk-SVD inklusive Bias-Termen), die Annahmen *Missing as Random* gegenüber *Missing as Negative*, eine Übersicht der wichtigsten Evaluationsmetriken (MAE, RMSE, Precision, Recall, Novelty, Diversity, Serendipity, Coverage), Offline- und Online-Evaluationsmethoden inklusive *All-but-n*-Approach, Community-based RS, Context-Aware Collaborative Filtering (CARS), den Damping-Effekt sowie Train/Validation/Test-Aufteilung mit Cross Validation.

## Aufbau des Notebooks

Das Notebook ist entlang der zehn Aufgabenblöcke der Mini Challenge strukturiert.

In **Aufgabe 1** wird der Datensatz tiefgehend exploriert: Datenqualität (Datentypen, Duplikate, fehlende Werte) wird geprüft und bereinigt, anschliessend werden Top-20-Listen nach Durchschnittsbewertung und Anzahl Bewertungen erstellt, der Bewertungsbereich verifiziert, Genres analysiert, Sparsity berechnet und mehrere verteilende Plots (Bewertungen pro User, Bewertungsverteilung über Zeit und Genre, Heatmaps der User-Item-Matrix) erzeugt.

In **Aufgabe 2** wird ein Baseline-Predictor nach Ekstrand/Riedl/Konstan implementiert: $b_{u,i} = \mu + b_u + b_i$. User- und Item-Bias werden regularisiert mit den Hyperparametern $\beta_u$ und $\beta_i$ gelernt, und ein Random-Recommender dient als Sanity-Check.

In **Aufgabe 3** wird die Datenaufteilung umgesetzt: Die ersten 20 % der Nutzer werden als Test-Set zurückgehalten und bis Aufgabe 10 nicht angefasst. Auf den verbleibenden 80 % wird eine 5-fache Cross Validation mit *All-but-n*-Masking ($n=11$) etabliert. Zusätzlich werden alle Evaluationsmetriken (MAE, RMSE, Precision@N, Recall@N, Novelty, Diversity, Coverage) als wiederverwendbare Funktionen bereitgestellt.

In **Aufgabe 4** werden die Hyperparameter $\beta_u$ und $\beta_i$ des Baseline-Modells per Grid Search über mehrere Werte optimiert und MAE, RMSE, Precision@N und Recall@N als Heatmaps visualisiert. Die Wahl der Zielmetrik wird begründet.

In **Aufgabe 5** entstehen acht KNN-basierte Collaborative-Filtering-Varianten: item- und user-basiert, jeweils mit Cosine- und Pearson-Similarity, jeweils mit und ohne User-Mean-Centering. Die Empfehlungen werden für die Beispiel-User 3, 5 und 7 ausgegeben.

In **Aufgabe 6** wird der Hyperparameter $K$ (Anzahl Nachbarn) für alle acht KNN-Modelle optimiert; RMSE, Precision@N und Recall@N werden als Funktion von $K$ geplottet und die Modelle werden inhaltlich verglichen.

In **Aufgabe 7** kommt die `surprise`-Bibliothek mit Funk-SVD zum Einsatz. Die Vorhersageformel
$\hat{r}_{ui} = \mu + b_u + b_i + p_u^\top q_i$
wird kurz erklärt, anschliessend werden die latenten Filmfaktoren mit UMAP auf zwei Dimensionen reduziert und interaktiv visualisiert.

In **Aufgabe 8** wird der Hyperparameter $k$ (`n_factors`) der SVD optimiert. RMSE, Precision@N und Recall@N werden als Funktion von $k$ untersucht und die Performance bei optimalem $k$ entlang aller Metriken ausgewiesen.

**Aufgabe 9** ist als offene Forschungsaufgabe angelegt. Drei zusätzliche Recommender werden gebaut und optimiert: eine SVD-Variante mit alternativer Bias-Konfiguration (`fast_lr_high_reg`), ein Baseline-Recommender mit *Actor-Boost*, der Filme bevorzugt, in denen häufig besetzte Schauspieler mitspielen, sowie ein diversitätsoptimierter Baseline-Recommender mit MMR-Re-Ranking, das einen Kompromiss zwischen Relevanz und Diversität herstellt.

In **Aufgabe 10** werden schliesslich alle gebauten RS – inklusive Random- und Baseline-Recommender – auf dem zu Beginn zurückgehaltenen Testset evaluiert. Die Ergebnisse werden tabellarisch und als vergleichende Plots gegenübergestellt und die Bedeutung eines unangetasteten Test-Sets bis zum Ende des Benchmarks wird diskutiert.

## Evaluationsmetriken

Die im Notebook eingesetzten Metriken folgen der Übersicht aus `Theorie.md`. **MAE** und **RMSE** messen die Vorhersagegenauigkeit auf Rating-Ebene, während **Precision@N** und **Recall@N** auf der Empfehlungsliste arbeiten. Ergänzt werden diese durch **Novelty** (wie unbekannt sind die empfohlenen Items für den Nutzer), **Diversity** (wie unterschiedlich sind die Items innerhalb einer Empfehlungsliste) und **Coverage** (welcher Anteil aller Items kann das System überhaupt empfehlen). Für die Hyperparametersuche wurde RMSE als primäre Zielmetrik gewählt, da sie mathematisch differenzierbar ist und grosse Fehler überproportional bestraft, was bei Rating-Vorhersagen sinnvoll ist.

## Reproduzierbarkeit

Die gesamte Lösung ist in **Python** implementiert. Wo nicht anders gefordert, kommen ausschliesslich `numpy`, `pandas`, `matplotlib` und `seaborn` zum Einsatz. Für den modellbasierten Teil (Aufgabe 7 ff.) werden zusätzlich `scikit-surprise`, `umap-learn`, `plotly` und `cython` verwendet.

Die folgenden Schritte reichen aus, um das Notebook lokal auszuführen:

```bash
# Repository klonen
git clone <repo-url>
cd <repo-name>

# (Empfohlen) Virtuelle Umgebung anlegen
python -m venv .venv
source .venv/bin/activate          # Linux / macOS
.venv\Scripts\activate             # Windows

# Abhängigkeiten installieren
pip install numpy pandas matplotlib seaborn scikit-surprise umap-learn plotly cython jupyter

# Notebook starten
jupyter notebook RSY_MiniChallenge_FS26.ipynb
```

Der MovieLens-Subset wird unter `./Data/ml-latest-small/` erwartet (Originaldaten von [grouplens.org](https://grouplens.org/datasets/movielens/)). Die Datei `movies_actors.csv` liegt im selben Ordner und wird vom Actor-Boost-Modell in Aufgabe 9 eingelesen.

## Projektstruktur

```
rsymcfs-26
├── MiniChallenge   
│    ├── Data/
│    │   └── ml-latest-small/
│    │        ├── links.csv
│    │        ├── movies.csv
│    │        ├── movies_actors.csv     # Selbst erstellter Schauspieler-Datensatz
│    │        ├── ratings.csv
│    │        ├── README.txt
│    │        └── tags.csv
│    └── RSY_MiniChallenge_FS26.ipynb   # Hauptnotebook mit allen Aufgaben
├── Theorie.md                          # Theoretische Zusammenfassung der Konzepte 
└── README.md                    
```

## Autor

**Loukas Vasilakis** – Mini Challenge im Modul *Recommender Systems (RSY)*, FHNW, Frühlingssemester 2026.

## Danksagung und Quellen

Dieses Projekt verwendet den **MovieLens "ml-latest-small"**-Datensatz, der vom **GroupLens Research Project** der University of Minnesota bereitgestellt wird. Der Datensatz beschreibt 5-Sterne-Bewertungen und Free-Text-Tags von [MovieLens](http://movielens.org), einem Filmempfehlungsdienst, und wurde am 26. September 2018 generiert. Er enthält 100'836 Bewertungen und 3'683 Tag-Anwendungen über 9'742 Filme von 610 Nutzern.

Die Verwendung erfolgt ausschliesslich zu Forschungs- und Ausbildungszwecken im Rahmen der Mini Challenge des Moduls *Recommender Systems (RSY)* an der FHNW. Es besteht keinerlei Verbindung zur University of Minnesota oder zur GroupLens Research Group, und es wird keine Befürwortung durch diese Institutionen impliziert. Der Datensatz unterliegt den Lizenzbedingungen von GroupLens, die unter <http://grouplens.org/datasets/> einsehbar sind.

Der ergänzende Datensatz `movies_actors.csv` (Schauspielerinformationen für das Actor-Boost-Modell in Aufgabe 9) wurde mithilfe eines ChatGPT-Agents erzeugt und ist nicht Teil des offiziellen MovieLens-Datensatzes.

### Zitation

> F. Maxwell Harper and Joseph A. Konstan. 2015. *The MovieLens Datasets: History and Context.* ACM Transactions on Interactive Intelligent Systems (TiiS) 5, 4: 19:1–19:19. <https://doi.org/10.1145/2827872>

## Lizenz

```
MIT License

Copyright (c) 2026 Loukas Vasilakis

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
