# soko-quiz-pawn
Quiz system in MySQL, general knowledge questions for the SA:MP server.  
_Developed by Dragan Avdic (Dragi)_

Kviz s pitanjima iz opste kulture se pokrece na svakih 15 minuta i imate 30 sekundi da odgovorite komandom `/kviz <odgovor>`. Ko prvi odgovori dobija novcanu nagradu. (Random $ 300-600) Svakom korisniku se belezi broj tacnih odgovora u `quiz_stats` tabelu.  
- correct_answers - za mesecnu listu  
- total_correct_answers - za sve vreme

Pitanja i odgovore lako mozete dodavati/izmenjivati putem phpMyAdmina ili druge platforme. Za dodavanje izaberete tabelu `quiz_questions > Insert` samo `quesiton` i `answer` popunite i nista drugo ne cackajte tu! 

Kolona `used` se automatski azurira na `1` cim se pitanje izabere za kviz da ne bi doslo do duplciiranja. U nastavku objasnjeno.

`/kviztop10` - Komanda za TOP listu igraca s tacnim odgovorima (Mesecno, Sve vreme)<br><br>

MySQL baza sadrzi 2 EVENTA: (Posaljtie upit bazi: SHOW EVENTS;)
- Resetuje na svaka 2 SATA vec postavljena pitanja da mogu ponovo da se koriste (Zbog random dupliciranja)
- Resetuje na svaki 1 MESEC top 10 listu MESECNU

Koriscenje biblioteke:
- a_mysql
- sscanf2
- Pawn.CMD
- uf
- timerfix <br><br>
  
> [!TIP]
> Na sta bi trebalo da obratite paznju i sta mozete unaprediti:
- Tabela `quiz_stats` sadrzi UNIQUE kljuc za `p_name` kolonu. Znaci nema dupliciranja redova.
- Potrebno je da napraviti spoljni kljuc `(quiz_stats.p_name)` da referencira `(players.username)` sa vasom glavnom tabelom gde su vam registrovani korisnici, npr. `players`.
- Dodati ogranicenje `ON DELETE CASCADE ON UPDATE CASCADE`.

S ovim podesavanjima necete imati problem s podacima i kad obrise igraca obrisace se i u quiz tabeli. Ako igrac ne postoji u tabeli `players` ne moze se ni dodati u `quiz` tabelu. Podaci moraju da se bezbede!!! _Slusajte Dragija! ;)_  <br><br>

Ako zelite npr. da na odredjeno vreme ispisete svim korisnicima ko je prvi u top10 tabeli,
napravite tajemr, funkciju i koristite ovaj SQL upit:
```
"SELECT p_name, correct_answers FROM quiz_stats WHERE correct_answers > 0 ORDER BY correct_answers DESC LIMIT 1"
```
I posle uzmete index 0 i smestite u neku variablu vasu, formatirate, ispisete i to je to.  

Fotografije:  
- [Slika 1](https://i.ibb.co/VwhQR7Z/sa-mp-497.png)  
- [Slika 2](https://i.ibb.co/0KCftc4/sa-mp-499.png)  
- [Slika 3](https://i.ibb.co/GsnNpcL/sa-mp-501.png)
