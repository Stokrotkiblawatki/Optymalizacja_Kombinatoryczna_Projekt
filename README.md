# Optymalizacja_Kombinatoryczna_Projekt

Imię i nazwisko: Wiktoria Szweda  
Nr grupy: I7.2  
Nr indeksu: 145355  

# Sieci Petriego
Sieć Petriego składa się z miejsc, tranzycji i krawędzi skierowanych.  
Aby dostać się do danego stanu układu potrzebne są "żetony", które mogą przemieszczać się po krawędziach grafu.   
Kolorowane sieci Petriego rozróżniają żetony, które mogą przyjmować określone typy.  
Różne rozszerzenia kolorowanych sieci Petriego:  
* priorytetowe - rozszerza się kolorowane sieci Petriego o priorytety dla poszczególnych tranzycji, w przypadku, gdy wiele tranzycji jest aktywnych (przykładowo, gdy aktywne są dwie tranzycje, to pierwsza zostanie wykonana tranzycja o niższym priorytecie, co pozwala na ograniczenie przestrzeni realizacji); w przypadku, gdy dwie tranzycje aktywne mają ten sam priorytet, wybór tranzycji jest losowy  
* czasowe- sieci, w których można określić opóźnienie aktywności tranzycji, czyli tranzycja staje się aktywna nie tylko po spełnieniu wymogów co do znaczników, ale również po odczekaniu określonej ilości czasu (przykładowo, jeśli w momencie A w miejscach wyjściowych są odpowiednie znaczniki, to dopiero w momencie B=A+opóźnienie, ta tranzycja będzie mogła być wykonana (o ile nadal jest odpowiednia ilość tokenów))
* stochastyczne- zmodyfikowane sieci czasowe, opóźnienie jest tu zmienną losową o zadanym rozkładzie  
* hierarchiczne- sposób na wyróżnianie niektórych modułów i odwoływanie się do pewnych podsieci, będących samymi w sobie sieciami Petriego, należy wtedy określić wejście i wyjście z podsieci oraz tranzycję modelującą złożoność podsieci; pozwala na uproszczenie bardziej złożonych sieci  
* czasu rzeczywistego- występują pętle czasowe, dostępności znaczników ulegają zmianie, znaczniki mogą mieć ograniczenia dotyczące wieku, występują bardziej skomplikowane zależności czasowe, czas jest płynny i uwzględniany przez wszystkie elementy sieci  

## Zastosowania sieci Petriego

Można przyjąć, że miejsca z żetonami to chwilowe stany układu. Przejścia to przetwarzanie danych lub fizycznych materiałów a żetony to dane lub materiały.  
Sieci Petriego znajdują zastosowanie przede wszystkim w automatyce i analizie danych.

# Implementacja kolorowanych sieci Petriego

Kod:
```
miejsca = []
tranzycje = []
typy = []
polaczenia = []

class Typ:
    def __init__(self, nazwa, podtypy):
        self.nazwa = nazwa
        self.podtypy = podtypy

class Miejsce:
    def __init__(self, nazwa_miejsca, typ_miejsca, poczatkowy, liczba):
        self.nazwa = nazwa_miejsca
        self.typ = typ_miejsca
        self.znaczniki = [poczatkowy, liczba]
        self.gotowe = False
        self.polaczenia_z_miejsca_dokads = []
        self.polaczenia_do_miejsca_skads = []


class Polaczenie:
    def __init__(self, z, do, warunki):
        self.poczatek = z
        self.koniec = do
        self.typ = ""
        self.warunki = warunki

class Tranzycja:
    def __init__(self, nazwa_tranzycji):
        self.nazwa = nazwa_tranzycji
        self.aktywna = False
        self.polaczenia_z_tranzycji_dokads = []
        self.polaczenia_do_tranzycji_skads = []


def dodaj_miejsce(nazwa, typ, poczatkowy, liczba):
    """Nazwa miejsca musi być unikalna, typ musi już istnieć,
    początkowy to element należący do typu typ, element musi już istnieć,
    liczba to liczba elementów początkowy, powinna być większa od 0"""
    nowe_miejsce = Miejsce(nazwa, typ, poczatkowy, liczba)
    miejsca.append(nowe_miejsce)
    return nowe_miejsce

def dodaj_tranzycje(nazwa):
    nowa_tranzycja = Tranzycja(nazwa)
    tranzycje.append(nowa_tranzycja)
    return nowa_tranzycja

def dodaj_typ(nazwa, lista_podtypow):
    """lista_podtypow w formacie [a,b,c] oznacza elementy
    należące do tworzonego typu"""
    nowy_typ = Typ(nazwa, lista_podtypow)
    typy.append(nowy_typ)
    return nowy_typ


def dodaj_polaczenie_z_miejsca_do_tranzycji(obiekt_miejsce, obiekt_tranzycja, warunki):
    """Warunki należy podać w postaci [nazwa_elementu, liczba elementów]"""
    nowe_polaczenie = Polaczenie(obiekt_miejsce, obiekt_tranzycja, warunki)
    obiekt_tranzycja.polaczenia_do_tranzycji_skads.append(nowe_polaczenie)
    obiekt_miejsce.polaczenia_z_miejsca_dokads.append(nowe_polaczenie)
    polaczenia.append(nowe_polaczenie)
    return nowe_polaczenie

def dodaj_polaczenie_z_tranzycji_do_miejsca(obiekt_tranzycja, obiekt_miejsce, warunki):
    """Warunki należy podać w postaci [nazwa_elementu, liczba elementów]"""
    nowe_polaczenie = Polaczenie(obiekt_tranzycja, obiekt_miejsce, warunki)
    obiekt_tranzycja.polaczenia_z_tranzycji_dokads.append(nowe_polaczenie)
    obiekt_miejsce.polaczenia_do_miejsca_skads.append(nowe_polaczenie)
    polaczenia.append(nowe_polaczenie)
    return nowe_polaczenie

def podsumuj():
    print("MIEJSCA")
    for i in range(len(miejsca)):
        print("Miejsce", i + 1)
        print("Nazwa:", miejsca[i].nazwa)
        print("Typ:", miejsca[i].typ)
        print("Znaczniki początkowe:", miejsca[i].znaczniki[0])
        print("Liczba znaczników początkowych:", miejsca[i].znaczniki[1])
        for j in range(len(miejsca[i].polaczenia_z_miejsca_dokads)):
            print("Połączenia z:", miejsca[i].polaczenia_z_miejsca_dokads[j].koniec.nazwa)
        for j in range(len(miejsca[i].polaczenia_do_miejsca_skads)):
            print("Połączenia do:", miejsca[i].polaczenia_do_miejsca_skads[j].poczatek.nazwa)
        print("Aktualna liczba znacznikow:", miejsca[i].znaczniki)
    print("TYPY")
    for i in range(len(typy)):
        print("Typ", i + 1)
        print("Nazwa typu:", typy[i].nazwa)
        print("Składowe typu:", typy[i].podtypy)
    print("TRANZYCJE:")
    for i in range(len(tranzycje)):
        print("Tranzycja", i + 1)
        print("Nazwa:", tranzycje[i].nazwa)
        for j in range(len(tranzycje[i].polaczenia_z_tranzycji_dokads)):
            print("Połączenia z:", tranzycje[i].polaczenia_z_tranzycji_dokads[j].koniec.nazwa)
        for j in range(len(tranzycje[i].polaczenia_do_tranzycji_skads)):
            print("Połączenia do:", tranzycje[i].polaczenia_do_tranzycji_skads[j].poczatek.nazwa)
    print("POŁĄCZENIA:")
    for j in range(len(polaczenia)):
        print("Połączenie", j + 1)
        print("Połaczenie z", polaczenia[j].poczatek.nazwa, "do", polaczenia[j].koniec.nazwa)
        print("Warunek uatywnienia:", polaczenia[j].warunki)
    return

def sprawdz_czy_miejsce_gotowe(miejsce):
    if miejsce.polaczenia_z_miejsca_dokads != []:
        if miejsce.polaczenia_z_miejsca_dokads[0].warunki[0] == miejsce.znaczniki[0]:
            if miejsce.polaczenia_z_miejsca_dokads[0].warunki[1] <= miejsce.znaczniki[1]:
                if miejsce.znaczniki[1] > 0:
                    miejsce.gotowe = True
    return

def sprawdz_gotowosc(tranzycja):
    for i in range(len(tranzycja.polaczenia_do_tranzycji_skads)):
        if tranzycja.polaczenia_do_tranzycji_skads[i].poczatek.gotowe == False:
            return False
    return True

def dzialaj(tranzycja):
    if sprawdz_gotowosc(tranzycja) == True:
        tranzycja.aktywna = True
        print("Tranzycja aktywna:", tranzycja.nazwa)
        for i in range(len(tranzycja.polaczenia_do_tranzycji_skads)):
            tranzycja.polaczenia_do_tranzycji_skads[i].poczatek.znaczniki[1] -= tranzycja.polaczenia_do_tranzycji_skads[i].warunki[1]
        for i in range(len(tranzycja.polaczenia_z_tranzycji_dokads)):
            tranzycja.polaczenia_z_tranzycji_dokads[i].koniec.znaczniki[1] += tranzycja.polaczenia_z_tranzycji_dokads[i].warunki[1]
        tranzycja.aktywna = False
        for j in range(len(miejsca)):
            miejsca[j].gotowe = False
    return

def prezentuj():
    for i in range(len(miejsca)):
        if i == len(miejsca)-1:
            print(miejsca[i].nazwa, "\t")
        else:
            print(miejsca[i].nazwa, "\t", end = " ")
    for i in range(len(miejsca)):
        if i == len(miejsca)-1:
            print(miejsca[i].znaczniki, "\t")
        else:
            print(miejsca[i].znaczniki, "\t", end = " ")
    print("\n")
    return

def symulacja():
    podsumuj()
    print("\n")
    i = 0
    prezentuj()
    while (i < 5):
        for j in range(len(tranzycje)):
            for k in range(len(miejsca)):
                sprawdz_czy_miejsce_gotowe(miejsca[k])
            dzialaj(tranzycje[j])
            prezentuj()
        i += 1
```        
Przykładowe dane wejściowe:
```
dodaj_typ("A", ["a","b","c"])
miejsce1 = dodaj_miejsce("Miejsce1", "A", "a", 3)
miejsce2 = dodaj_miejsce("Miejsce2", "A", "b", 3)
miejsce3 = dodaj_miejsce("Miejsce3", "A", "c", 1)
tranzycja1 = dodaj_tranzycje("Tranzycja1")
tranzycja2 = dodaj_tranzycje("Tranzycja2")
dodaj_polaczenie_z_tranzycji_do_miejsca(tranzycja1, miejsce3, ["c", 2])
dodaj_polaczenie_z_miejsca_do_tranzycji(miejsce2, tranzycja1, ["b", 1])
dodaj_polaczenie_z_miejsca_do_tranzycji(miejsce1, tranzycja1, ["a", 1])
dodaj_polaczenie_z_miejsca_do_tranzycji(miejsce3, tranzycja2, ["c", 1])
dodaj_polaczenie_z_tranzycji_do_miejsca(tranzycja2, miejsce1, ["a", 1])

symulacja()
```
Graf ilustrujący sieć stworzoną danymi wejściowymi:

![alt text](https://github.com/Stokrotkiblawatki/Optymalizacja_Kombinatoryczna_Projekt/blob/main/petri_net_1.png?raw=true)

Dane wyjściowe dla powyższych danych wejściowych:
```
MIEJSCA
Miejsce 1
Nazwa: Miejsce1
Typ: A
Znaczniki początkowe: a
Liczba znaczników początkowych: 3
Połączenia z: Tranzycja1
Połączenia do: Tranzycja2
Aktualna liczba znacznikow: ['a', 3]
Miejsce 2
Nazwa: Miejsce2
Typ: A
Znaczniki początkowe: b
Liczba znaczników początkowych: 3
Połączenia z: Tranzycja1
Aktualna liczba znacznikow: ['b', 3]
Miejsce 3
Nazwa: Miejsce3
Typ: A
Znaczniki początkowe: c
Liczba znaczników początkowych: 1
Połączenia z: Tranzycja2
Połączenia do: Tranzycja1
Aktualna liczba znacznikow: ['c', 1]
TYPY
Typ 1
Nazwa typu: A
Składowe typu: ['a', 'b', 'c']
TRANZYCJE:
Tranzycja 1
Nazwa: Tranzycja1
Połączenia z: Miejsce3
Połączenia do: Miejsce2
Połączenia do: Miejsce1
Tranzycja 2
Nazwa: Tranzycja2
Połączenia z: Miejsce1
Połączenia do: Miejsce3
POŁĄCZENIA:
Połączenie 1
Połaczenie z Tranzycja1 do Miejsce3
Warunek uatywnienia: ['c', 2]
Połączenie 2
Połaczenie z Miejsce2 do Tranzycja1
Warunek uatywnienia: ['b', 1]
Połączenie 3
Połaczenie z Miejsce1 do Tranzycja1
Warunek uatywnienia: ['a', 1]
Połączenie 4
Połaczenie z Miejsce3 do Tranzycja2
Warunek uatywnienia: ['c', 1]
Połączenie 5
Połaczenie z Tranzycja2 do Miejsce1
Warunek uatywnienia: ['a', 1]


Miejsce1 	 Miejsce2 	 Miejsce3 	
['a', 3] 	 ['b', 3] 	 ['c', 1] 	


Tranzycja aktywna: Tranzycja1
Miejsce1 	 Miejsce2 	 Miejsce3 	
['a', 2] 	 ['b', 2] 	 ['c', 3] 	


Tranzycja aktywna: Tranzycja2
Miejsce1 	 Miejsce2 	 Miejsce3 	
['a', 3] 	 ['b', 2] 	 ['c', 2] 	


Tranzycja aktywna: Tranzycja1
Miejsce1 	 Miejsce2 	 Miejsce3 	
['a', 2] 	 ['b', 1] 	 ['c', 4] 	


Tranzycja aktywna: Tranzycja2
Miejsce1 	 Miejsce2 	 Miejsce3 	
['a', 3] 	 ['b', 1] 	 ['c', 3] 	


Tranzycja aktywna: Tranzycja1
Miejsce1 	 Miejsce2 	 Miejsce3 	
['a', 2] 	 ['b', 0] 	 ['c', 5] 	


Tranzycja aktywna: Tranzycja2
Miejsce1 	 Miejsce2 	 Miejsce3 	
['a', 3] 	 ['b', 0] 	 ['c', 4] 	


Miejsce1 	 Miejsce2 	 Miejsce3 	
['a', 3] 	 ['b', 0] 	 ['c', 4] 	


Tranzycja aktywna: Tranzycja2
Miejsce1 	 Miejsce2 	 Miejsce3 	
['a', 4] 	 ['b', 0] 	 ['c', 3] 	


Miejsce1 	 Miejsce2 	 Miejsce3 	
['a', 4] 	 ['b', 0] 	 ['c', 3] 	


Tranzycja aktywna: Tranzycja2
Miejsce1 	 Miejsce2 	 Miejsce3 	
['a', 5] 	 ['b', 0] 	 ['c', 2] 	



Process finished with exit code 0
```
