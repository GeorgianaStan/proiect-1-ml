# Proiect 1 – Predicția plecării clienților (Telco Customer Churn)

## Setul de date
- **Set:** Telco Customer Churn (IBM, varianta folosită și pe Kaggle)
- **Sursă:** https://raw.githubusercontent.com/IBM/telco-customer-churn-on-icp4d/master/data/Telco-Customer-Churn.csv
- **Ce prezic:** dacă un client părăsește operatorul de telefonie (coloana `Churn`: Yes / No)
- **Dimensiune:** 7.043 de clienți, 21 de coloane; 26,5% dintre clienți pleacă

## Răspunsuri

**1. E clasificare sau regresie? Ce greșeală a modelului ar costa mai mult în realitate?**
Este o problemă de clasificare binară, deoarece vrem să prezicem dacă un client pleacă (`Yes`) sau rămâne (`No`). Cea mai costisitoare greșeală este un fals negativ: modelul prezice că un client rămâne, dar acesta pleacă, iar firma pierde ocazia de a-l păstra. Un fals pozitiv poate duce la oferirea inutilă a unei reduceri unui client care ar fi rămas oricum. De aceea, este important să identificăm cât mai mulți clienți care pleacă, urmărind metrica recall pentru clasa `Yes`.

**2. De ce împărțim datele înainte de scalare?**
Împărțim datele înainte de scalare ca să calculăm media și deviația standard doar din setul de antrenare. Dacă am scala înainte, informații din setul de test ar influența pregătirea datelor și evaluarea ar fi mai puțin corectă.

**3. Ce scor are un model care nu știe nimic?**
Baseline-ul are accuracy de 0,735, dar recall și F1 pentru clasa `Yes` sunt 0, deoarece prezice că toți clienții rămân și nu identifică nicio plecare. De aceea, accuracy singur poate induce în eroare când clasele sunt dezechilibrate.

**4. Care model a ieșit mai bine și de ce credeți că s-a întâmplat asta?**
În rezultatele afișate, Random Forest are cel mai mare F1 (0,633), foarte puțin peste GridSearch (0,632), arborele de decizie (0,618) și regresia logistică (0,614). Diferențele sunt mici, iar varianta optimizată nu a îmbunătățit F1 pe acest test față de Random Forest inițial. Probabil, Random Forest a obținut un rezultat puțin mai bun deoarece combină predicțiile mai multor arbori și poate surprinde relații mai complexe dintre caracteristici, reducând dependența de un singur arbore.

**5. De ce ați ales metrica principală pe care ați ales-o?**
Aleg recall pentru clasa `Yes` ca metrică principală, deoarece un fals negativ înseamnă să nu identificăm un client care urmează să plece. F1 rămâne util pentru comparația modelelor, fiindcă ține cont atât de recall, cât și de precision.

**6. Concluzie: ce model recomand, cât bate baseline-ul și o limitare**
Aș recomanda Random Forest dintre modelele testate, deoarece are cel mai mare F1 afișat (0,633) și recall de 0,78 pentru clienții care pleacă, mult peste baseline-ul cu recall 0. Deși arborele de decizie are un recall puțin mai mare, aleg Random Forest pentru echilibrul mai bun dintre recall și precision, reflectat de F1-ul superior; dacă obiectivul ar fi exclusiv identificarea cât mai multor clienți care pleacă, ar trebui comparat cu atenție și arborele. Totuși, precision-ul de 0,53 implică alarme false, iar analiza ar trebui validată și pe alte date înainte de folosirea în producție.

**Model recomandat:** Random Forest

## Lucrul cu AI
- **Ce am cerut agentului:** Am lucrat atât cu Claude, cât și cu ChatGPT pentru a înțelege mai bine pașii proiectului și pentru a formula explicațiile cât mai corect. Am pus și întrebări suplimentare atunci când nu înțelegeam anumite erori legate de biblioteci sau aveam nevoie de ajutor pentru interpretarea graficelor și a rezultatelor.
- **Ce am schimbat din ce mi-a dat:** Am reformulat unele explicații cu propriile mele cuvinte și am cerut, unde a fost cazul, variante mai scurte și mai clare. Am urmărit să înțeleg ce fac, nu doar să preiau răspunsurile oferite de AI.
- **O greșeală a AI-ului pe care am observat-o:** La un moment dat, codul selecta modelul considerat cel mai bun în funcție de scorul F1, deși pentru proiectul meu stabilisem că recall-ul este metrica principală. Am observat neconcordanța și am ținut cont de obiectivul proiectului atunci când am evaluat modelele.
