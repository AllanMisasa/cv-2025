## Dag 2 - Computer Vision

---

## Artikler

* https://docs.opencv.org/4.x/d5/daf/tutorial_py_histogram_equalization.html
* https://docs.opencv.org/4.x/d4/d1b/tutorial_histogram_equalization.html


---
## Agenda

* Målet med billedbehandling i Computer Vision
* Histogram equalization (kontrastjustering)
* Annoteringer
* Cropping og gridding


---

## Målene med billedehandling

* Gøre det lettere at isolere/detektere objektet der er target
* Enten **subtraktivt** - f.eks. ved at fjerne støj, fjerne farver, mønstre, intensiteter, der ikke er del af target
* Eller **additivt** - f.eks. ved at fremhæve objektet, f.eks. gennem kontrast, opdele farver, øge bestemte intensiteter

---

## Histogram equalization

* Hvis et billede har lav kontrast er det svært at skelne mellem farver/intensiter. 
* Det kan ses på et histogram - hvis de fleste pixels er tæt forbundet, er rækkevidden af intensiteter ikke så høj
* Histogram equalization er så en metode til at sørge for at de bliver fordelt ud på alle intensiteter 
* Læs mere i artikler

---

## Opgave 0

* Lav histogrammer på billederne til din portefølge ligesom i går (hvis du har dem), ellers så find billeder med forskellige kontrastforhold (ud fra dit eget syn)
* Lav histogram equalization på hver af dem
* Er det som forventet? Blev kontrasten forøget? Hvorfor tror du?


---

## Annoteringer

* Du kan annotere, dvs. tegne objekter på billedet ud fra et centralt punkt eller en firkant for at fremhæve noget
* https://learnopencv.com/annotating-images-using-opencv/
* Vi sætter bare start og slutpunkt for streger og rektangler
* Centerpunkt skal tilføjes for cirkulære/runde objekter

---

## Opgave 1

* Vi har allerede brugt farvefiltre. Prøv at finde ud af efter isolering af en farve, hvilke pixels der indeholder den farve (på et billede hvor kun et objekt er af den farve). 
* Ud fra disse pixels kan du vælge hjørner og derfor har du allerede start og slutpunkt for en rektangel
* Tegn så en rektangel der fremhæver hvor objektet er i billedet

---

## Patching

* Det kan være lettere at finde disse regions of interest hvis man deler billedet op i et gitter
* https://learnopencv.com/cropping-an-image-using-opencv/
* Så ved at definere en bestemt størrelse på hvert felt af gitteret kan vi splitte billedet op i felter
* Det skal dog gå op; der skal kunne være et heltal felter af den angivede størrelse, så modulusoperatoren er anbefalet for at finde felters størrelse

---
## Opgave 2

*  Opsplit dit billede i grids ligesom i det sidst eksempel i: https://learnopencv.com/cropping-an-image-using-opencv/
* Analyser farver og intensiteter i hver patch enten i dit eget billede eller papegøje billedet
* Kan du udelukke patches bare ud fra farver/intensiteter om de indeholder "the object of interest?" 
* Kan du finde patches hvor du er 100% på at object of interest er i baseret bare på analysen?

---

## Histogram equalization - CLAHE metoden

* Histogram equalization har en stor begrænsning - kalkulationen er over hele billedet for at indstille kontrasten
* Kan resultere i at visse regioner ikke bliver kontrastjusteret ordentligt
* CLAHE er så en kombination af patching/tiling - lav gitter, og lav histogram equalization for hvert felt/patch i stedet for over hele billedet på én gang
* Så f.eks. hvis en region er meget lys i forhold til resten, bliver de to regioner justeret passende til deres lokale forhold

---

## Opgave 3

* Find et billede hvor der er lokale forskelle i lysforhold. 
* Brug CLAHE metoden
	* Python: https://docs.opencv.org/4.x/d5/daf/tutorial_py_histogram_equalization.html
	* C++: https://docs.opencv.org/3.4/d6/db6/classcv_1_1CLAHE.html 
* Sammenlign CLAHE med normal histogram equalization