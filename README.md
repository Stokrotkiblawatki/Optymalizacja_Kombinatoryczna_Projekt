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

# Zastosowania sieci Petriego

Można przyjąć, że miejsca z żetonami to chwilowe stany układu. Przejścia to przetwarzanie danych lub fizycznych materiałów a żetony to dane lub materiały.  
Sieci Petriego znajdują zastosowanie przede wszystkim w automatyce i analizie danych.

# Implementacja składowych kolorowanych sieci Petriego

Miejsce:
```
class Miejsce:
    def __init__(self, nazwa_miejsca, typ_miejsca, poczatkowy, liczba):
        self.nazwa = nazwa_miejsca
        self.typ = typ_miejsca
        self.znaczniki = [poczatkowy, liczba]
        self.gotowe = False
        self.polaczenia_z_miejsca_dokads = []
        self.polaczenia_do_miejsca_skads = []
```        

