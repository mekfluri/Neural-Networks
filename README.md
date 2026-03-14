# Klasifikacija zadovoljstva putnika u avio-saobraćaju primenom veštačkih neuronskih mreža

## Opis projekta
Ovaj rad obuhvata razvoj i implementaciju modela veštačkih neuronskih mreža za predviđanje zadovoljstva putnika na osnovu različitih parametara leta i usluga. Cilj istraživanja je identifikacija ključnih faktora koji utiču na korisničko iskustvo i kreiranje stabilnog klasifikatora za automatizovanu analizu.

## Skup podataka
Korišćen je javno dostupan skup podataka sa platforme **Kaggle** (*Airline Passenger Satisfaction*). Dataset sadrži informacije o preko 100.000 putnika, uključujući:
* **Demografske podatke:** pol, starost, tip putnika.
* **Specifičnosti leta:** klasa, distanca, tip putovanja.
* **Ocene usluga:** online boarding, udobnost sedišta, wifi usluga, čistoća, itd.
* **Tehničke parametre:** kašnjenja u polasku i dolasku.

## Preprocesiranje podataka
Pre same implementacije modela, izvršena je detaljna priprema podataka:
* **Uklanjanje nepotrebnih atributa:** Kolone poput `id` i tehničkih indeksa su izbačene jer nemaju prediktivnu vrednost.
* **Obrada nedostajućih vrednosti:** Nedostajuće vrednosti u koloni o kašnjenju dolaska popunjene su medijanom trening seta kako bi se izbeglo curenje podataka (*data leakage*).
* **Kodiranje kategorijalnih varijabli:** Primenjeni su `LabelEncoder` za binarne atribute i `One-Hot Encoding` za kolonu `Class`.
* **Skaliranje:** Svi numerički podaci su normalizovani u opseg od 0 do 1 pomoću minimalnih i maksimalnih vrednosti kako bi se obezbedila stabilnost učenja.

## Arhitektura neuronske mreže
U okviru projekta su testirane i upoređene tri arhitekture:
1.  **Deep Model (128 -> 64 -> 32 neurona):** Fokusiran na dubinsku ekstrakciju karakteristika.
2.  **Wide Model (256 -> 128 neurona):** Široka struktura za analizu visoke varijanse podataka.
3.  **Shallow Model (32 -> 16 neurona):** Jednostavnija arhitektura za brzu obradu.

Kao pobednička arhitektura izabran je **Deep Model** koji koristi:
* **Aktivacione funkcije:** `ReLU` za skrivene slojeve i `Sigmoid` za izlazni sloj.
* **Regularizaciju:** `Dropout` od 20% do 30% radi sprečavanja preprilagođavanja (*overfitting*).
* **Optimizator:** `Adam` sa funkcijom gubitka `binary_crossentropy`.

## Rezultati i evaluacija
Model je treniran uz mehanizam **Early Stopping**, koji je prekinuo proces čim je gubitak na validacionom skupu prestao da opada. Postignuti su sledeći rezultati:
* **Ukupna tačnost (Accuracy):** 96%
* **Preciznost (Precision):** 0.97 (za klasu zadovoljnih putnika)
* **Odziv (Recall):** 0.98 (za klasu nezadovoljnih putnika)

Analiza matrice konfuzije pokazuje minimalan broj lažno pozitivnih i lažno negativnih rezultata, što potvrđuje visoku pouzdanost modela u realnim uslovima.

## Korišćene tehnologije
* **Jezik:** Python
* **Biblioteke:** TensorFlow/Keras, scikit-learn, Pandas, NumPy, Matplotlib, Seaborn