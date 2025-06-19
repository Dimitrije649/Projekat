Pac-Man (Projektni zadatak)  			
Dimitrije Džonić II5 
Ukratko o samoj igri:
Pac-Man je video igra jurenja u lavirintu; 
igrač kontroliše istoimeni lik kroz zatvoreni lavirint. Cilj igre je da pojedete sve tačke postavljene u lavirintu, izbegavajući četiri duha u boji koji ga jure.
Objašnjenje programa:

Pogram se deli na 3 dela 

Svi resursi:
Za početak postoje nivoi koji se grade od simbola koji su prethodno određeni
Simboli:
* `_` prazno
* `#` zid
* `$` zid koji drži duha zarobljenim
* `.` tačke koje Pac-Man skuplja
* `@` Pac-Man (sam lik sa kojim se  upravlja)
* `&` duh koji juri
* `%` zarobljeni duh
Primer jednog nivoa
############################
#............##............#
#.####.#####.##.#####.####.#
#.####.#####.##.#####.####.#
#.####.#####.##.#####.####.#
#..........................#
#.####.##.########.##.####.#
#.####.##.########.##.####.#
#......##....##....##......#
######.#####_##_#####.######
######.#####_##_#####.######
######.##____&_____##.######
######.##_###_####_##.######
######.##_###_####_##.######
______.___###$####___.______
######.##_###%####_##.######
######.##_########_##.######
######.##__________##.######
######.##_########_##.######
######.##_########_##.######
#............##............#
#.####.#####.##.#####.####.#
#.####.#####.##.#####.####.#
#...##.......@........##...#
###.##.##.########.##.##.###
###.##.##.########.##.##.###
#......##....##....##......#
#.##########.##.##########.#
#.##########.##.##########.#
#..........................#
############################
############################
```
Nakon samih nivoa imamo likove, sam izgled zidova i zvukove :
duhovi :       
Pac-Man:  
zidovi:    
Animacije samih likova su slike njih koje se brzo menjaju (                 )
Srce same igre: Klasa GameEngine
Ova klasa je srce igre. Sadrži informacije o nivou, kreira objekte na nivou, pokreće glavnu petlju igre i obrađuje sudare između objekata.

Metoda __init__
Inicijalizuje osnovne parametre igre, kao što su:

levelPelletRemaining: broj preostalih kuglica u nivou.
levelObjects: dvodimenzionalna lista objekata nivoa (28x32), svaki objekat je instanca klase levelObject.
movingObjectPacman: instanca klase movingObject koja predstavlja Pac-Mana.
movingObjectGhosts: lista instanci klase movingObject koja predstavlja duhove (4 duha).
levelObjectNamesPassable: lista imena objekata kroz koje se može proći (prazno, kuglica, power-up).
Metoda levelGenerate
Generiše nivo na osnovu tekstualnog fajla:
Otvara fajl sa nivoom.
Čita fajl liniju po liniju.
Na osnovu karaktera u fajlu (_, #, $, . itd.), postavlja odgovarajuće objekte na nivo.
Metoda encounterFixed
Proverava šta se nalazi na određenoj relativnoj koordinati (x, y):
Ako je prazno, vraća "empty".
Ako je kuglica, vraća "pellet".
Ako je power-up, vraća "powerup".
Metoda encounterMoving
Proverava da li se Pac-Man susreće sa duhovima:
Ako se Pac-Man susreće sa aktivnim duhom, vraća "dead".
Inače vraća "alive".
Metoda loopFunction
Glavna petlja igre koja pokreće kretanje Pac-Mana i duhova:
Pac-Man se pomera u sledećem koraku.
Pac-Man se pomera na trenutnu poziciju.
Duhovi se pomeraju u sledećem koraku, ako su aktivni.
Klasa levelObject
Predstavlja objekte u nivou (zidove, kuglice, power-upove):
name: ime objekta.
isDestroyed: status da li je objekat uništen.
Klasa movingObject
Predstavlja pokretne objekte (Pac-Man, duhovi):
name: ime objekta.
isActive: status da li je objekat aktivan.
isCaged: status da li je duh u kavezu.
dirCurrent, dirNext, dirOpposite: trenutni, sledeći i suprotni pravac kretanja.
dirEdgePassed: status da li je objekat prešao ivicu.
coordinateRel, coordinateAbs: relativne i apsolutne koordinate objekta.
Metoda MoveNextGhost
Određuje sledeći pravac kretanja duha:
Proverava dostupne pravce kretanja.
Odabira novi pravac na osnovu dostupnih pravaca i trenutnog pravca kretanja.
Metoda MoveNext
Određuje da li Pac-Man može da se pomeri u sledećem pravcu:
Proverava dostupnost kretanja u odabranom pravcu (levo, desno, gore, dole).
Ako je sledeći pravac dostupan, postavlja trenutni pravac na sledeći pravac.
Metoda MoveCurrent
Pomera Pac-Mana u trenutnom pravcu:

Ažurira apsolutne i relativne koordinate Pac-Mana na osnovu trenutnog pravca kretanja.
Proverava sudare sa objektima koji blokiraju put.
Zaključak
Ovaj kod implementira osnovne funkcionalnosti igre Pac-Man. Klasa GameEngine upravlja glavnim petljama igre i generiše nivoe, dok klase levelObject i movingObject predstavljaju objekte u igri i omogućavaju njihovo kretanje i interakciju.
Sam program:
Importi: Kod importuje potrebne biblioteke:

Tk, Label, Entry, Button, PhotoImage, messagebox, END, Canvas iz tkinter za GUI elemente.
Timer iz threading za upravljanje petljama igre.
field iz data za upravljanje poljem igre.
os i pygame za interakciju sa operativnim sistemom i zvučnim efektima.
Klasa MainEngine: Ova klasa upravlja inicijalizacijom igre, resursima, widgetima i logikom igre.

Inicijalizacija
Metoda __init__: Postavlja glavni prozor, inicijalizuje varijable za trenutni nivo, status igre, rezultat i živote.
Metoda __initResource: Učitava sve potrebne spriteove i zvuke za igru iz foldera resource.
Metoda __initWidgets: Postavlja GUI elemente kao što su labeli, polja za unos, dugmad i canvas za prikaz igre. Takođe povezuje događaje sa tastature za kontrolu igre.
Metoda __initLevelSelect: Prikazuje interfejs za odabir nivoa i pokreće glavnu petlju Tkinter-a.
Upravljanje Nivom
Metoda lvSelect: Rukuje odabirom nivoa i validacijom.
Metoda __initLevelOnce: Inicijalizuje nivo samo jednom, uklanjajući interfejs za odabir nivoa i prikazuje canvas za igru.
Metoda __initLevel: Generiše nivo, postavlja objekte igre na canvas i započinje fazu "Get Ready".
Kontrole Igre
Metode za Odgovor na Unos: Rukuju unosima sa tastature za kontrolu kretanja Pac-Mana:
inputResponseLeft, inputResponseRight, inputResponseUp, inputResponseDown: Menjaju pravac Pac-Man-a.
inputResponseEsc: Zaustavlja petlju igre i prikazuje poruku "Game Over".
inputResponseReturn: Pokreće igru ako je nivo generisan, ali još nije započet.
Faze Igre
Metoda __initLevelStarting: Prikazuje poruku "Get Ready" sa efektom treptanja pre početka igre.
Metoda gameStartingTrigger: Pokreće glavnu petlju igre aktiviranjem metode loopFunction periodično.
Glavna Petlja Igre
Metoda loopFunction: Ažurira stanje igre, uključujući pozicije Pac-Mana i duhova, i proverava događaje poput susreta sa duhovima.
Metoda spritePacman: Animira kretanje Pac-Mana na osnovu njegovog pravca i ažurira njegovu poziciju na canvasu.
Upravljanje Resursima
Spriteovi i Zvuci: Spriteovi za Pac-Man-a i duhove se učitavaju i dodeljuju na osnovu njihovog stanja i pravca. Zvučni efekti se učitavaju za različite događaje u igri.
Rukovanje Greškama
Kod uključuje rukovanje greškama za nevažeće unose nivoa i nedostajuće fajlove nivoa, prikazujući odgovarajuće poruke korisniku.
Primer Glavne Petlje
Evo kako glavna petlja igre funkcioniše:

Inicijalizacija: Resursi se učitavaju, a GUI se postavlja.
Odabir Nivoa: Igrač bira nivo.
Generisanje Nivoa: Izabrani nivo se generiše i prikazuje.
Početak Igre: Prikazuje se poruka "Get Ready", a glavna petlja igre počinje.
Glavna Petlja: Pac-Man i duhovi se kreću na osnovu korisničkog unosa i logike igre. Igra proverava susrete i kontinuirano ažurira stanje igre.
Struktura ovog koda omogućava funkcionalnu Pac-Man igru sa osnovnim odabirom nivoa, animacijom spriteova i zvučnim efektima. Svaka metoda je odgovorna za specifičan deo toka igre, što osigurava modularnost i lako održavanje.
Literatura:
Korišćene su sledeće skripte radi reference:
https://github.com/BaggerFast/Pacman
I dodatna pomoć:
https://www.youtube.com/watch?v=9H27CimgPsQ&pp=ygUOcGFjIG1hbiBweWdhbWU%3D

