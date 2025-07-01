# Projekat: Prepoznavanje saobraćajnih znakova korišćenjem dubokih neuronskih mreža

Ovaj projekat implementira kompletan tok rada za klasifikaciju saobraćajnih znakova, obuhvatajući faze učitavanja i preprocesiranja podataka, definisanja i treniranja dubokog konvolucionog modela, evaluacije, augmentacije podataka, kao i vizualizacije rezultata učenja. Svaki deo sistema je pažljivo modularizovan radi jasnoće i lakše održivosti.

---

## 1. Učitavanje podataka iz pickle fajla

Funkcija `load_pickled_data` omogućava selektivno učitavanje podataka iz pickle fajlova, pružajući fleksibilnost pri rukovanju velikim datasetima sa različitim atributima. Time se postiže efikasnost u memorijskoj potrošnji i ubrzava priprema podataka za dalju obradu.

---

## 2. Inicijalno učitavanje i organizacija podataka

Kod za učitavanje CSV fajla sa opisom klasa (imena saobraćajnih znakova) i pickle fajlova sa slikama i labelama predstavlja osnovu za pripremu dataset-a. Detektuju se ključni atributi kao što su veličina ulaznih slika, broj klasa i broj primera u različitim podskupovima, što je neophodno za pravilno definisanje modela i trening procedure.

---

## 3. Vizualizacija uzoraka i analize distribucije

Modul za vizualizaciju omogućava prikaz nasumično odabranih primera iz svake klase, kao i histogram distribucije broja uzoraka po klasama. Ova analiza pomaže u identifikaciji potencijalnih problema sa neuravnoteženošću dataset-a, što je od ključnog značaja za uspešno treniranje modela.

---

## 4. Pomoćne funkcije za merenje vremena i prikaz progres barova

Implementirane funkcije služe za praćenje trajanja treninga i prikaz napretka u terminalu u realnom vremenu. Time se olakšava monitoring procesa treniranja, naročito kod dugotrajnih eksperimenta.

---

## 5. Napredno preprocesiranje podataka

Funkcija `preprocess_dataset` pretvara RGB slike u sivu skalu korišćenjem standardnih perceptivnih težinskih faktora, skalira vrednosti piksela na interval [0, 1] i primenjuje adaptivno histogram izjednačavanje (CLAHE) za poboljšanje kontrasta, što može značajno doprineti otpornosti modela na varijacije osvetljenja. Takođe vrši one-hot enkodiranje labela i mešanje podataka radi smanjenja preklapanja u toku treniranja.

---

## 6. Dinamička augmentacija podataka tokom treninga

Klasa `AugmentedSignsBatchIterator` implementira online augmentaciju podataka koja uključuje nasumične rotacije i projekcijske transformacije sa kontrolisanim intenzitetom i verovatnoćom. Ovaj pristup poboljšava generalizaciju modela time što emulira različite uslove posmatranja saobraćajnih znakova u realnom svetu.

---

## 7. Struktura i parametri modela

`Parameters` namedtuple centralizuje sve hiperparametre modela, uključujući arhitekturu konvolucionih slojeva, veličine potpuno povezanih slojeva, strategije regularizacije, optimizacione postavke i opcije treninga. Ova organizacija omogućava jednostavno upravljanje eksperimentima i reprodukovanje rezultata.

---

## 8. Upravljanje skladištenjem i organizacija fajlova

Klasa `Paths` automatski kreira i održava strukturirane direktorijume za čuvanje modela, istorije treninga i vizuelnih prikaza učenja. Generisanje jedinstvenih imena fajlova na osnovu konfiguracije modela olakšava upravljanje višestrukim verzijama i eksperimentima.

---

## 9. Implementacija ranog zaustavljanja treninga

Klasa `EarlyStopping` omogućava efikasnu kontrolu trening procesa tako što nadgleda metriku validacije i prekida trening ukoliko nema poboljšanja u definisanom broju epoha (patience). Takođe vraća parametre modela na poslednje najbolje stanje, sprečavajući overfitting i nepotrebno trošenje resursa.

---

## 10. Jednostavan sistem logovanja

`SimpleModelLogger` pruža jasan i koncizan ispis informacija o dataset-u, arhitekturi i parametrima modela. Ovakav pristup doprinosi transparentnosti i lakšem praćenju eksperimenta.

---

## 11. Konstrukcija konvolucionog modela u TensorFlow-u

Definisani su modularni blokovi:

- `fully_connected` i `fully_connected_relu` omogućavaju kreiranje dense slojeva sa i bez ReLU aktivacije.
- `conv_relu` realizuje konvolucioni sloj sa ReLU aktivacijom, uključujući inicijalizaciju težina Xavier metodom.
- `pool` predstavlja max pooling sloj za smanjenje dimenzionalnosti.
- `model_pass` implementira kompletan forward pass kroz konvolucione i potpuno povezane slojeve, uz korišćenje dropout regularizacije, pri čemu se višestruki nivoi ekstrakcije karakteristika kombinuju za poboljšanje performansi.

---

## 12. Vizualizacija procesa učenja

Funkcije `plot_curve` i `plot_learning_curves` omogućavaju detaljnu analizu metrika treninga i validacije kroz vreme. Korišćenje logaritamske skale za gubitak omogućava bolji uvid u progres u ranijim fazama treninga, dok poređenje tačnosti olakšava procenu generalizacije modela.

---

## 13. Iteracija kroz dataset u batch-evima

Batch iterator implementiran je kako bi se obezbedila efikasna obrada podataka u manjim paketima, što je ključna komponenta za skalabilnost treninga na velikim skupovima podataka. Omogućava i opciju mešanja podataka radi boljeg učenja.

---

## 14. Evaluacija modela po batch-evima

Funkcija za evaluaciju omogućava procenu tačnosti i gubitka modela na proizvoljnim skupovima podataka, čime se obezbeđuje objektivna metrika performansi koja može poslužiti za odlučivanje o daljim koracima u treningu ili podešavanju hiperparametara.

---

# Preporučeni tok rada

1. **Učitavanje i priprema podataka:** Koristeći funkcije za učitavanje pickle fajlova i CSV opisa, potom primenjujući napredne tehnike preprocesiranja i augmentacije.
2. **Konfiguracija modela:** Definisati sve neophodne parametre modela i treninga koristeći `Parameters`.
3. **Organizacija resursa:** Inicijalizovati `Paths` objekat za organizaciju rezultata.
4. **Definisanje modela:** Kreirati TensorFlow model prema specifikacijama.
5. **Trening:** Koristiti batch iterator sa augmentacijama, uz logovanje i rano zaustavljanje.
6. **Evaluacija i analiza:** Pratiti metrike tokom treninga i vizualizovati rezultate pomoću grafika.
7. **Eksperimenti:** Prilagođavati parametre i arhitekturu na osnovu dobijenih rezultata.

---

## Tehnički zahtevi

- Python 3.7 ili noviji
- TensorFlow 1.x (preporučeno, uz moguće prilagođavanje za TF 2.x)
- NumPy
- Pandas
- scikit-image
- matplotlib



