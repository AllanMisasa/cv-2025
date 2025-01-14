## Agenda

* Hvorfor neurale net?
* Hvad er et neuralt net?
* Opbygning af Convolutional Neural Network (CNN)
	* Lag og filtre
	* Aktiveringsfunktioner
* Basal custom CNN til klassificering i VSCode
	* Gennemgang af teori via eksempel
	* Hvordan vi kan forbedre en CNN

---

## Hvorfor neurale net?

* Kan genkende objekter ud fra mere komplekse mønstre
* Kan også segmentere baseret på lærte mønstre
* Er ofte mere robuste til finde objekter end simplere detekteringsmetoder


---

## Ulemper ved neurale net

* Komplekst at forstå hvad de kigger på (vi bruger GradCam senere til at løse dette mysterium)
* Kræver mange resourcer, både data input og computerkraft
* Svært at troubleshoot hvis de ikke performer ordentligt 

---

## Hvad er et neuralt net?'

![[Pasted image 20250113181824.png]]

---

## Hvad er et neuralt net?

* https://www.youtube.com/watch?v=aircAruvnKk&list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi
* Et netværk af interconnected nodes, inspireret af en simplificering af neuroner i hjernen
* Ekstremt simplificeret - vores neuroner bearbejder det vi ser, input går gennem lag der vægter vigtigheden af signalerne og til sidst laver en vurdering

---

## Hvad er....

* Fungerer egentligt som flere lag af filtre der langsomt finder ud af hvad der går igen i en række billeder
* Til sidst efter nok iterationer ved den hvad den skal kigge efter
	* Hvis din data er retvisende
	* Hvis lagene er nøje udvalgt til formålet
	* Hvis du har nok data, hvis du har trænet nok... osv...


---
## Hvad kan et neuralt net?

* 2 basale evner for convolutional neural networks i computer vision:
	* **Klassificering** - sige hvad et eller flere objekter på billedet er, og differentiere mellem flere typer objekter
	* **Segmentering** - sige hvor ting er, uden at vide hvad den ting er
	* Maaaange flere... https://paperswithcode.com/methods/area/computer-vision

---

## Krav

* Data der kan fungere som eksempler på det objekt du gerne vil kunne genkende/segmentere
	* Mange, i god kvalitet
	* Med variation i objektet
* En computer med nok resourcer, helst et grafikkort...

---

## Start på convolution

https://upload.wikimedia.org/wikipedia/commons/1/19/2D_Convolution_Animation.gif

Vi bruger https://www.tensorflow.org/tutorials/images/cnn som eksempel

---

## Convolutional neural network

* Vi skal først træne en model til at genkende objekter
* Dette gør vi ved at bruge convolutions samt max pooling på input billeder af flere omgange
* Modellen lærer **vægtene** der kan vægte pixels i input signalet - 
	* Giver de udslag? E.g. hvor på billedet er der meget rødt, meget lyst, meget grønt etc.
	* Hvor mange pixels plejer at være tætte af hver farve?

---

## Definitioner

* **Vægte**: et floating point tal vi multiplicerer på pixels i inputbilledet. Et netværk af vægte er resultatet af træning.
* **Model:** Både arkitekturen af vægtene og resultatet af træning. Vi kan efter træning bruge de kalkulerede vægte til at klassificere billeder.
* **Lag/layer**: Hvis netværket er 2D, så er hvert lag hver 1D "lag" af vægte input billedets pixels skal igennem
* **Output shape**: Billedets opløsning efter det er gået igennem et lag af vægte

---

## Træning - typisk lagdeling

![[Screenshot from 2025-01-13 18-39-03.png]]

---
## Convolution kernel

![[Pasted image 20250113184233.png]]

---
## Conv2D

* Disse lag laver convolutions på billedet og giver et output der er meget mindre end billedet
* Øverste i ovenstående giver for et input 32 30x30 billeder - altså 32 mindre billeder, som hver er inputtet der har været igennem forskellige filtre
* Ingen strategi, bare 32 forskellige convolution kernels
* Conv2D sammen med pooling er for at ekstrahere features, altså vigtige pixels

---

## Max pooling i 2D - visualisering

![[Pasted image 20250113132420.png]]

---

## Max pooling i 2D - forklaring

* Downsampling mellem lag baseret på lokale maksima
* Lad os bruge et eksempel på en 4x4 matrice, og vi har et 2x2 filter (convolution) som stepper over billedet med et stride på 2 (vi påfører 2x2 vinduet efter at gå 2 pixels til højre)
* For hver 2x2 patch tager vi den maksimale værdi og kasserer resten

----

## Effekt af max pool - visualisering

![[Pasted image 20250113132439.png]]

---

## Flatten

* Gør det sidste 3D array efter convolutions til et 1D array
* Klargøring til dense layers hvor vi kategoriserer det endelige resultat

---

## Dense layers

* Dense betyder at fra sidste lag er alle vægte forbundet til de antal vægte defineret i det dense lag
* Så i eksemplet har vi 1024 felter tilbage fra convolutions, som alle er krydsforbundet til de 64 vægte i dense lag

---

## Dense layers visuelt

![[Pasted image 20250113184805.png]]

---

## Dense layer forklaret

* De er fuldt forbundne på kryds og tvært - for vi prøver nu at finde sammenhængene mellem de resterende vægte vi har tilbage efter convolutions og max pooling
* Tænk på det som at du har filtreret ned til de farver, de mønstre du kan genkende efter at have set 100vis af billeder af det samme dyr f.eks.
* Hvis du tilfældigt kigger alle eksempler af mønstrende igennem for at se sammenhængen
* Dette er et dense lag

---

## Endelig klassificering

* Vi ender med 10 vægte i det sidste lag - 1 for hver kategori i vores data
* Den vægt der er højest efter forward pass af billedet er den endelige klassificerede kategori

---

**Nu til en pause og så en gennemgang via kode i VSCode på tavlen**