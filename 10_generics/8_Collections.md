# Collections
## Nadelen van arrays
We hebben al eerder gezien hoe we arrays kunnen gebruiken om gegevens van hetzelfde type in ��n datastructuur voor te stellen. Hoewel arrays handig zijn, hebben ze ook een aantal nadelen:

* Je weet niet altijd van tevoren hoe groot de array zal worden, nochtans moeten we op voorhand ruimte reserveren door op te geven hoe groot de array moet zijn. Hierdoor kan het voorkomen dat we veel computergeheugen reserveren voor een array die mogelijk nooit volledig gevuld zal zijn.
* Omgekeerd kan het ook zijn dat we onze array te klein defini�ren en we dus niet alle data kwijtraken in de array. We kunnen dit, omslachtig, oplossen door een nieuwe, grotere array te defini�ren en daar de oude naar te kopi�ren, inclusief te nieuwe data.
* Wanneer de array value-types bevat (int, double, struct, etc.) en deze array kopi�ren naar een nieuwe array waar we vervolgens een waarde aanpassen, dan zal enkel de waarde in de gekopieerde array veranderen, niet in de originele. Indien we echter een array van objecten (referentietypes) maken en in een kopie een referentie aanpassen, dan zal deze ook aangepast worden in de originele array.

## Collecties: geavanceerde arrays

Collecties zijn speciale objecten waarin een reeks van andere objecten kan worden gerefereerd. Daar collecties zelf ook objecten zijn, heeft dit een paar gevolgen:

* Collecties moeten ge�nstantieerd worden voor te gebruiken zijn (m.b.v. new keyword)
* Collecties hebben properties die onder andere het aantal objecten in collectie weergeeft.
* Collecties hebben methoden om onder andere objecten toe te voegen en verwijderen
* Daar collecties beschreven zijn in klassen kan je de typische object-geori�nteerde eigenschappen hierop toepassen: je kan collecties overerven, encapsuleren (composiet), etc.

Er bestaan een hele resem collecties en je hebt er zelfs al van gebruikt, namelijk de List<>-klasse (weliswaar de generic variant, de niet-generieke versie heet ArrayList). Volgende tabel geeft een beschrijving van de meest gebruikte collecties die in de System.Collections.Namespace zitten [Bron MSDN](http://msdn.microsoft.com/en-us/library/system.collections.aspx)

Klasse	| Beschrijving
------  | ------------
ArrayList| 	Standaard collectie die dynamisch kan groeien (zelfde als de List<> generieke variant).
BitArray|	Array die enkel Booleans (true/false) bevat. Handig om dus een bit-voorstelling te doen.
Hashtable|	Ieder element bevat een waarde en een bijhorende sleutel als index.
Queue|	Stelt een first-in, first-out (FIFO) lijst voor.
SortedList|	Soortgelijk als Hashtable, een sleutel stelt de index voor, elementen worden echter onmiddellijk gerangschikt .
Stack	Stelt een Last-in, first-out (LIFO) lijst voor.

We zullen een aantal van de eerder vernoemde collecte-klassen verderop bekijken, maar we zullen onmiddellijk de generieke varianten bekijken omdat die veel handiger zijn. Het is namelijk zo dat generic collecties meestal bruikbaarder en veiliger zijn dan de gewone collecties die we zonet kort hebben beschreven. Generic collecties zijn �strongly types� en bieden een betere veiligheid op gebied van �type safety�, hun gebruik is echter quasi identiek dan dat van de niet generieke collecties.

### Collection namespace

As je een collection-klasse wilt gebruiken, vergeet dan niet de ``System.Collection`` namespace toe te voegen. Indien je een generieke collection nodig hebt (zie verder) dan dien je de ``System.Collections.Generic`` namespace te gebruiken

### Type-safety
Een niet-generieke colletion is **niet type-safe** . Een generieke collection is dat wel.

Volgende 2 code-voorbeelden tonen dit.

In het niet-generieke geval zal deze code compileren maar **tijdens de uitvoer** zal de laatste lijn een fout (Exception) geven::
```java
ArrayList nietGeneriekeList=new ArrayList();
string naam= "Tim";
nietGeneriekeList.Add(naam);
int leeftijd = (int)nietGeneriekeList[0];
```

Bij een generieke collection zal er bij soortgelijk code een compiler-error optreden (gedrag dat meestal wenselijk is) en de code zal dus niet gecompileerd kunnen worden:
```java
List<string> generiekeList=new List<string>();
string naam= "Tim";
generiekeList.Add(naam);
int leeftijd = (int)nietGeneriekeList[0]; // compilererror: cannot convert type string to in
```

# Generic collections
Generic collecties zijn �strongly typed� en bieden een betere veiligheid op gebied van �type safety�, hun gebruik is echter quasi identiek aan dat van de niet generieke collecties.

We hebben reeds 1 generic colletie-klasse veel gebruikt, namelijk de List<>-klasse, waarbij we tussen de haakjes het type opgaven dat de List kon bevatten , bv List of List. Met andere woorden, je hebt reeds vorig academiejaar generics gebruikt zonder het goed en wel te beseffen!

Een volledig overzicht van alle mogelijk generic collections vind je hier terug [MSDN](http://msdn.microsoft.com/en-us/library/system.collections.generic.aspx).

We beschrijven nu de werking van een aantal typische collecties, merk op dat deze werking quasi identiek is als die voor de niet-generische versie.

## List collectie

Een List collectie is de meest standaard collectie die je kan beschouwen als een veiligere variant op een een doodnormale array. Via de Add(T item) methode kan je elementen toevoegen aan de lijst. In volgende voorbeeld maken we een List aan die objecten van het type string mag bevatten:

```java
List<String> myStringList = new List<String>();
``` 
myStringList.Add("This is the first item in my list!");
Het leuke van een List is dat je deze ook kan gebruiken als een gewone array, waarbij je mbv de indexer elementen kan aanroepen. Stel bijvoorbeeld dat we een lijst hebben met minstens 4 strings in. Volgende code toont hoe we de string op positie 3 kunnen uitlezen en hoe we die op positie 2 overschrijven:

```java
Console.WriteLine(myStringList[3]);
myStringList[2] = "andere zin";`
```
Interessante methoden en properties voorts zijn:

* ``Count``: property die teruggeeft hoeveel elementen in de lijst aanwezig zijn.
* ``Clear() :methode die de volledige lijst leegmaakt
* ``Insert()``: methode om element op specifieke plaats in lijst toe te voegen, bijvoorbeeld:
```java
myStringList.Insert(3,�A fourth sentence�); 
````
voegt de string toe op de verde (3+1) plek.
 * ``Contain(T item)``: geef als parameter een specifiek object mee (van het type dat de List<> bevat) om te weten te komen of dat specifieke object in de List<> terug te vinden is. Indien ja dan zal true worden teruggeven.
* ``IndexOf(T item)``: geeft de index terug van het element item in de rij. Indien deze niet in de lijst aanwezig is dan wordt 0 teruggegeven.

### Foreach loops
Je kan met een eenvoudige for of while-loop over een list-object itereren, maar het gebruik van een foreach-loop is toch handiger. Dit is dan ook de meestgebruikte operatie om eenvoudig (je kan ook Linq overwegen!) en snel een bepaald stuk code toe te passen op ieder element van de lijst:

```java
List<int> integerList=new List<int>();
integerList.Add(2);
integerList.Add(3);
integerList.Add(7);

foreach(int prime in integerList)
{
   Console.WriteLine(prime);
}

## Queue<> collectie
Een queue (Ned. : rij) stelt een FIFO-lijst voor. Een queue stelt dus de rijen voor die we in het echte leven ook hebben wanneer we bijvoorbeeld aanschuiven aan een ticketverkoop. Met deze klasse kunnen we zo�n rij simuleren en ervoor zorgen dat steeds het �eerste/oudste� element in de rij als eerste wordt behandeld. Nieuwe elementen worden achteraan de rij toegevoegd.

We gebruiken 2 methoden om met een queue te werken:

* ``Enqueue(T item)``: Voeg een item achteraan de queue toe.
* ``Dequeue()``: geeft het eerste element in de queue terug en verwijdert dit element vervolgens uit de queue.

Voorbeeld:


```java
Queue<string> theQueue =  new Queue<string>();
theQueue.Enqueue("I arrived here first!");
theQueue.Enqueue("I arrived second.");
theQueue.Enqueue("I'm last");
 
Console.WriteLine(theQueue.Dequeue());
Console.WriteLine(theQueue.Dequeue());
```

Dit zal op het scherm tonen:
```
I arrived here first!
I arrived here second.
```

#### Peek
Een andere interessante methode is de **Peek()** methode; hiermee kunnen we kijken in de queue wat het eerste element is, zonder het te verwijderen.

### Stack collectie
Daar waar een queue first-in-first-out is, is een stack last-in first out. M.a.w. het recentst toegevoegde element zal steeds vooraan staan en als eerste verwerkt worden. Je kan dit vergelijken met een stapel waar je steeds bovenop een nieuw object legt.

![](/assets/10_generics/generics3.png)

We gebruiken volgende 2 methoden om met een stack te werken:

* ``Push(T item)``: plaats een nieuw item bovenop de stapel
* ``Pop()``: geeft het bovenste element in de stack terug en verwijdert vervolgens dit element van de stack
Voorbeeld:

```java
Stack<string> theStack =  new Stack<string>();
theStack.Push("I arrived here first!");
theStack.Push("I arrived second.");
theStack.Push("I'm last");
 
Console.WriteLine(theStack.Pop());
Console.WriteLine(theStack.Pop());
```
Dit zal dus het volgende resultaat geven:
```
I'm last
I arrived second.
``` 


## Dictionary<TKey,TValue> collectie

In de niet generieke-colletions heb je de Hashtable, de generische versie is de dictionary. In een dictionary wordt ieder element voorgesteld door een index en de waarde van het element. De sleutel moet een unieke waarde zijn zodat het element kan opgevraagd worden uit de dictionary aan de hand van deze sleutel.

Bij de declaratie van de dictionary dien je op te geven wat het type van de key zal zijn , alsook het type van de waarde (value). In het volgende voorbeeld maken we een dictionary van klanten, iedere klant heeft een unieke ID (de key) alsook een naam (die niet noodzakelijk uniek is):

```
Dictionary<int, string> customers = new Dictionary<int, string>();
customers.Add(123, "Tim Dams");
customers.Add(6463, "James Bond");
customers.Add(666, "The beast");
customers.Add(700, "James Bond");
``` 

Merk op dat je niet verplicht bent om een int als key te gebruiken, dit mag eender wat zijn, zelfs een klasse.

We kunnen nu met behulp van een foreach-loop alle elementen tonen (merk op dat de foreach-constructie voor eender welke type collectie kan gebruikt worden om doorheen een array te lopen):

```java
foreach (var item in customers)
{
    Console.WriteLine(item.Key+ "\t:"+item.Value);
}
```

We kunnen echter ook een specifiek element opvragen aan de hand van de key. Stel dat we de waarde (naam) van de klant met key (id) gelijk aan 123 willen tonen:

```java
Console.WriteLine(customers[123]);
```

De key werkt dus net als de index bij gewone arrays, alleen heeft de key nu geen relatie meer met de positie van het element in de collectie.

### Eender welk type voor key en value

De key kan zelfs een string zijn en de waarde een ander type. In het volgende voorbeeld hebben we eerder een klasse Student aangemaakt. We maken nu een student aan en voegen deze toe aan de studentenLijst. Vervolgens willen we de leeftijd van een bepaalde student tonen op het scherm en vervolgens verwijderen we deze student:

```java
Dictionary<string, Student> studentenLijst = new Dictionary<string, Student>();
Student stud= new Student();
stud.Naam= "Tim";stud.Leeftijd=24;
studentenLijst.Add("AB12",stud);
Console.WriteLine(studentenLijst["AB12"].Leeftijd);
studentenLijst.Remove("AB12");
```