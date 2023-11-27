from random import randint, choice

# Dane bohatera
imię = input('Podaj imię swojego bohatera: ')
życie = 100
mana = 100

# Funkcje ataków i przywracania
def zwykły_atak():
    return randint(3, 10)

def fire_ball():
    global mana
    if mana < 10:
        print("-" * 40)
        print("Nie masz wystarczającej ilości many!")
        return 0
    mana -= 10
    return randint(13, 20)

def lodowate_ostrze():
    global mana
    if mana < 15:
        print("-" * 40)
        print("Nie masz wystarczającej ilości many!")
        return 0
    mana -= 15
    return randint(10, 15)

def uderzenie_pioruna():
    global mana
    if mana < 20:
        print("-" * 40)
        print("Nie masz wystarczającej ilości many!")
        return 0
    mana -= 20
    return randint(15, 25)

def medytacja():
    global mana
    przyrost_many = randint(15, 25)
    mana += przyrost_many
    print(f"Praktykujesz medytację i odzyskujesz {przyrost_many} many.")
    return -(0.5 * przyrost_many)  # Zwróć ujemną wartość, aby oznaczyć, że to jest regeneracja many

def uzdrowienie():
    global mana, życie
    if mana < 30:
        print("-" * 40)
        print("Nie masz wystarczającej ilości many!")
        return 0
    mana -= 30
    przyrost_zdrowia = randint(15, 25)
    życie_po_uzdrowieniu = życie + przyrost_zdrowia
    if życie_po_uzdrowieniu > 100:
        przyrost_zdrowia = 100 - życie  # Przywróć tylko tyle, aby życie wynosiło 100
    życie += przyrost_zdrowia
    print(f"Leczysz się i odzyskujesz {przyrost_zdrowia} punktów życia.")
    return -(0.2 * przyrost_zdrowia)

def wybierz_atak():
    print('a/A - Wykonaj Normalny Atak')
    print('b/B - Fire ball!')
    print('c/C - Lodowate ostrze!')
    print('d/D - Uderzenie pioruna!')
    print('m/M - Medytacja (odzyskaj manę)')
    print('u/U - Uzdrowienie (leczenie)')
    co = input().upper()
    if co == 'A':
        return zwykły_atak()
    elif co == 'B':
        return fire_ball()
    elif co == 'C':
        return lodowate_ostrze()
    elif co == 'D':
        return uderzenie_pioruna()
    elif co == 'M':
        return medytacja()
    elif co == 'U':
        return uzdrowienie()
    else:
        print("Nie wybrano akcji")
        return 0
    
def wyswietl_motywacje():
    motywacje = [
        "Niech twoja odwaga będzie silniejsza niż strach!",
        "Pamiętaj, że nawet najdłuższa podróż zaczyna się od jednego kroku.",
        "Zawsze jest czas na nowy początek. Daj z siebie wszystko!",
        "Twoja wytrwałość jest kluczem do sukcesu.",
        "Wiem, że potrafisz! Idź na całość!",
        "Każda porażka to lekcja. Nigdy się nie poddawaj!",
        "Twoja siła tkwi w determinacji. Nie rezygnuj!",
        "Zmiany zaczynają się wtedy, gdy jesteś gotów je zaakceptować.",
        "Życie to walka, ale ty jesteś silniejszy niż myślisz!",
        "Pamiętaj, żeby celebrować każdy mały postęp. To buduje motywację!",
    ]
    print(choice(motywacje))

# Nowe funkcje - Sklep i zdobywanie poziomów
złoto = 0
poziom = 1
doświadczenie = 0

def sklep():
    global złoto, życie, mana
    print("Witaj w sklepie!")
    print("1. Mikstura życia (50 złota) - Przywraca 30 punktów życia.")
    print("2. Mikstura many (50 złota) - Przywraca 30 many.")
    print("3. Powrót do walki")
    
    wybór = input("Wybierz opcję (1/2/3): ")
    
    if wybór == '1':
        if złoto >= 50:
            złoto -= 50
            życie += 30
            print("Kupiłeś Miksturę życia! Przywrócono 30 punktów życia.")
        else:
            print("Nie masz wystarczającej ilości złota.")
    elif wybór == '2':
        if złoto >= 50:
            złoto -= 50
            mana += 30
            print("Kupiłeś Miksturę many! Przywrócono 30 many.")
        else:
            print("Nie masz wystarczającej ilości złota.")
    elif wybór == '3':
        print("Powrót do walki.")
    else:
        print("Nieprawidłowy wybór.")

def zdobyj_doświadczenie():
    global doświadczenie, poziom, złoto
    doświadczenie += 20
    złoto += 10
    print(f"Zdobyłeś 20 punktów doświadczenia i 10 złota!")

    if doświadczenie >= 100:
        awans_na_poziom()

def awans_na_poziom():
    global poziom, doświadczenie
    poziom += 1
    doświadczenie = 0
    print(f"Gratulacje! Awansowałeś na poziom {poziom}!")

# Funkcja losująca przeciwnika
def losowy_przeciwnik():
    przeciwnicy = [
        ["Mały Goblin", 15, 3, 0],
        ["Nimfa Wodna", 10, 3, 0],
        ["Ognisty Smok", 30, 5, 2],
        ["Lodowy Yeti", 25, 4, 1],
        ["Cień Mroku", 20, 5, 3],
        ["Gigantyczny Skorpion", 35, 6, 2],
        ["Mędrzec Zakonu", 40, 7, 5],
        ["Mechaniczny Golem", 50, 8, 3],
        ["Krwawa Wiedźma", 45, 7, 4],
        ["Zmutowany Minotaur", 55, 10, 2],
        ["Arcymag Ciemności", 60, 12, 6],
    ]
    return choice(przeciwnicy)

# Pętla główna gry
liczba_pokonanych_przeciwników = 0

while życie > 0:
    przeciwnik = losowy_przeciwnik()
    print("-" * 40)

    while przeciwnik[1] > 0 and życie > 0:
        print(f"{imię} walczy teraz z {przeciwnik[0]}")
        print(f"Przeciwnik ma {przeciwnik[1]} HP i zadaje Ci {przeciwnik[2]} obrażeń")

        życie -= przeciwnik[2]
        if życie <= 0:
            break
    
        wyswietl_motywacje()
        print(f"Masz {życie} HP i {mana} many")
        atak = wybierz_atak()

        if isinstance(atak, int):
            przeciwnik[1] -= atak
            print(f"Zadałeś {atak} obrażeń")
        else:
            mana += atak

    if przeciwnik[1] <= 0:
        print('Zabiłeś przeciwnika!!!')
        liczba_pokonanych_przeciwników += 1
        zdobyj_doświadczenie()

    # Po pokonaniu przeciwnika sprawdź, czy bohater chce odwiedzić sklep
    wybór_sklepu = input("Czy chcesz odwiedzić sklep? (tak/nie): ").lower()
    if wybór_sklepu == 'tak':
        sklep()

print("-" * 40)
print("KONIEC GRY!")
print(f"Zabiłeś {liczba_pokonanych_przeciwników} przeciwników na poziomie {poziom}.")
print(f"Aktualny poziom: {poziom}, Złoto: {złoto}, Doświadczenie: {doświadczenie}")
