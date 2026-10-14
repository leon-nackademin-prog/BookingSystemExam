## Bugg: Skiftlägeskänslighet

Jag valde Fel nummer 3 i uppgiften. "Sökningen är skiftlägeskänslig" Det gick inte att söka på en kund såvida man inte skrev namnet 
100% korrekt alltså skiftlägeskänsligt. 


### Hur återskapade du felet?
Jag körde testet Search_WhenSearchTermHasDifferentCasings_ReturnsBooking


### Vilket resultat fick du?
Resultatet av testet blev false (nekades) eftersom den returnerade en tom lista istället för en lista med 1 filtrerad kund. Eftersom Contain
metoden inte blev godkänd så blev resultatet som returnerades "false" och listan stod tom.


### Vilket resultat skulle du ha fått?
Jag skulle ha fått tillbaka 1 kund från listan med den matchande namnet


### Vilken del av koden orsakade felet?
Inom kodblocket för metoden Search() såg ut ungefär såhär innan ändringen. När denna kod kördes så användes enbart Contains() metoden som filtrering. 
Då den är case sensitive så nekade den texten om det skiljde stora och små bokstäver. Jag uppdaterade filtret med en stringcomparison.OrdinalIgnoreCase så 
att den skulle fungera då den nu tar emot stora och små bokstäver (inte längre skiftlägeskänslig)

public IReadOnlyList<Booking> Search(string searchTerm)
    {
        return repository.GetAll()
            .Where(booking =>
                !booking.IsCancelled &&
                booking.CustomerName.Contains(searchTerm)
            .ToList();
    }
Koden uppdaterades då med "booking.CustomerName.Contains(searchTerm, StringComparison.OrdinalIgnoreCase))"
                
          
### Varför gav koden ett felaktigt resultat?
Kodens funktion Contain sökte bokstavligt talat efter samma ord så 
om texten skiljde sig med stora eller små bokstäver så identifierade
inte Contain metoden den som samma pga skillnaden på stora/små bokstäver.
Det var inte fel på värdet som skickades in "avanoria" men eftersom 
kundens namn skrivs "Avanoria" så kände den inte igen pga versalen i början på namnet. Contain gjorde precis som den ska göra, det var jag
som måste lägga till extra filtrering så den ser stora och små bokstäver
som samma.


### Vad ändrade du?
I metoden Search så fungerade filtreringen så att den kollade först så bokningen inte är avbokad samt att namnet innehåller sökresultatet som vi skickade med i testet. Den sökte orddagrant med Contains så jag la till en 
stringcomparison på strängen med StringComparison.OrdinalIgnoreCase.



### Hur kontrollerade du att ändringen fungerade?
Jag provade att köra testet Search_WhenSearchTermHasDifferentCasings_ReturnsBooking igen och det gick igenom då testet fick tillbaka vad den förväntade sig, den specifika kunden i en lista.



### Vilket automatiserat test skyddar mot att felet återkommer?
Testet Search_WhenSearchTermHasDifferentCasings_ReturnsBooking skyddar eftersom den dubbelkollar i testet ifall en bokning som har versaler i sig kan bli hittat även om 
användaren skriver in namnet med enbart små bokstäver.
