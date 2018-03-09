<h1>Oppgave3</h1>
Nei, pcen er ikke treg. Jeg har lagt til time.Sleep() noen steder slik at det blir lettere å teste SIGINT i d). 
<h2>a)</h2>
<h3>addup.go</h3>
Jeg har valgt å bruke tre channels i denne oppgaven, dette kun for å litt grundigere ilustrere bruken av dem. 
C1 og C2 lagrer hvert sitt tall fra cmd input, C3 lagrer summen når C1 og C2 har blitt summert. <br>
Bruker plassene i os.Args ([]) for å lese inn tallene som blir skrivet i cmd. konverterer dem til ints.<br>
funksjonA bruker tall fra stdin som argument, og dytter dem inn i hver sin channel. Her kalles og en .wait(wg) for å forsikre om at ikke programmet går videre før funksjonB har summert og dyttet summen i C3, hvor den så setter wg.done. Når tallet er summert i funksjonB, henter funksjonA ut tallet fra C3 og printer det ut. <br>
Det er og en annen waitgroup i funksjonA (wg2) som venter på at tallet blir printet ut. <br>
<br>
Verdt å nevne: funksjonA er bygd opp som en egen funksjon med goroutine i mainbodyen, men kjøres uten "go" fra mainfunksjonen. For å tydeligere illustrere goroutines kan en bare fjerne goroutinen i funksjonA, skrive "go funksjonA" i main, og legge til wg2.Add og wg2.Done før og etter... Blir helt samme resultat og bruk av goroutines uansett. 
<br>
<h2>b)</h2>
Slik jeg har tolket oppgaven skal addtofile lese to tall fra cmd og legge disse inn i en tekstfil. sumfromfile skal tallene fra samme tekstfilen, og skrive summen inn i tekstfilen. addtofile skal også printe ut summen etter den har blitt summert.
<h5>Så dette er recap av løsningen min:</h5>
addtofile må kjøres først med to tall som parametre eks. addtofile 2 2. sumfromfile leser tallene, summerer dem, og skriver summen i tekstfilen. Nå må addtofile kjøres UTEN parametre, og vil da printe ut summen som nå står i tekstfilen. <br>
Altså: om en skriver addtofile med to tall, vil den skrive tallene i fil, om en skriver addtofile UTEN parametre, vil den printe det som står i tekstfilen. 
<h3>addtofile.go</h3>
For å sjekke at inputen i cmd er med to tall, eller uten parametre, bruker jeg en if-else med len(os.Args) som condition, dette sjekker lengden på slicen som blir opprettet når en bruker os.Args. <br>
om len(os.Args)==3 vil en vil med navn "numbers.txt.lock" bli opprettet med tallene som innhold. Om filen allerede finnes vil den bli overridet til å inneholde de nye tallene. <br>
om len(os.Args)==1, altså en KUN skriver "addtofile" i cmd, vil det som står i filen bli printet ut. (Ja, om det er flere tall vil alt bli printet ut, men poenget er at den vil skrive ut summen om sumfromfile har blit brukt...)<br>
Hvis len(os.Args) er hverken 1 eller 3 vil det bli printet en del setninger om hva som kan være galt. 
<h3>sumfromfile.go</h3>
Dette programmet leser "numbers.txt.lock", gjør innholdet til string (fra []byte), og splitter stringsa. Jeg brukte direkte plassene i slicen til å få tak i tallene først, men uten å splitte ender dette opp med å kun få tak i enkle siffre. Når jeg bruker string.Split blir tallene lagret som de er. <br>
Har så en if-setning som sjekker om det er to tall i filen, om det er det,, vil den kjøre funksjonen sumBytes og skrive summen inn i "numbers.txt.lock". Om det IKKE er 2 tall i filen, vil det bli printet noe setninger om hva som kan ha gått galt.<br>
Mer detaljer om koden står i koden. 
<h2>c) </h2>
<h4>Error handling i addup.go</h4>
I addup.go er første errorhandling i strconv.Atoi(linje24). Her vil det komme en error-melding dersom en skriver noe annet en tall. Det vil da komme en "invalid syntax" melding. <br>
Om en skriver noe annet en to tall som argument, vil det komme en "index out of range" melding, da koden prøver å lagre os.Args[1] og os.Args[2], som da ikke finnes. <br>
<h4> error handling i sumfromfile.go og addtofile.go</h4>
I denne filen laget jeg en egen funksjon for error-handlingen, så jeg slipper å skrive if-setningen hver gang jeg skriver noe med en error type.<br>
sumfromfile.go inneholder de tidligere nevnte error typene, med samme errorhandling som i addtofile.go.
I addtofile.go har ioutil.ReadFile en errortype. Denne erroren blir trigget om programmet ikke finner filen som blir spesifisert.<br>
os.Create i addtofile.go har også en errortype. Ifølge librariet vil denne funksjonen oppdage PathErrors. Altså om en prøver å lage en fil på et sted som ikke er muligt. <br>





<h2>d)</h2>
SIGINT håndtering er lagt til i egne funksjoner som kjøres i mainen til alle programmene i a) og b).<br>
<h2>e) IKKE DONE</h2>







