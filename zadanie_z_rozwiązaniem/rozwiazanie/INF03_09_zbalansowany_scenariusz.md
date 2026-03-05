# Scenariusz Szkoleniowy do Egzaminu INF.03 (Arkusz 09 - Czerwiec 2024 / Galeria Zdjęć)

Niniejszy poradnik został przygotowany z myślą o inżynieryjnym i bezstresowym podejściu do rozwiązania zadania egzaminacyjnego "Biuro Turystyczne" (galeria zdjęć). Dowiesz się z niego nie tylko, jak poprawnie napisać kod, ale też jak uniknąć błędów, mądrze planować pracę i ratować się, gdy coś pójdzie nie tak.

Metodyka opiera się na 6 filarach: **Szablonie Bazowym, Testowaniu Wstępnym, Treningu Błędów, Diagnozie F12, Mapach Myśli oraz Alternatywach Nowoczesnych.**

---

## KROK 1: Szablon Bazowy (Boilerplate) i Operacje Bazodanowe

Kluczem do pewnego startu na egzaminie jest utworzenie stabilnego fundamentu dla swojego projektu. Zamiast chaotycznie patrzeć na złożone makiety, przygotuj niezbędne pliki i zabezpiecz darmowe punkty za poprawną strukturę.

1. Na swoim koncie utwórz na pulpicie docelowy folder z Twoim numerem zdającego.
2. Od razu utwórz komplet pustych plików, wymaganych w zadaniu: `galeria.html`, `styl.css`, `skrypt.js`.
3. Wypełnij główny plik `galeria.html` bazowym szkieletem (tzw. Boilerplate), powiązanym prawidłowo z CSS i JS. Arkusz ten wymaga jedynie logiki frontendowej, więc w `galeria.html` skupimy się na środowisku JS/HTML5.

**Niezbędny Szablon Bazowy widoku:**

```html
<!DOCTYPE html>
<html lang="pl">
  <head>
    <meta charset="UTF-8" />
    <title>Biuro turystyczne</title>
    <!-- Zabezpiecz punkty za ostylowanie - podepnij plik CSS od razu -->
    <link rel="stylesheet" href="styl.css" />
  </head>
  <body>
    <!-- Miejsce na główny układ bloków -->

    <!-- Wywołanie skryptu wkładamy na końcu pliku (nad zamykającym </body>) -->
    <!-- Gwarantuje to, że skrypt uruchomi się dopiero, gdy obrazki będą już na stronie -->
    <script src="skrypt.js"></script>
  </body>
</html>
```

### Konfiguracja i Kwerendy (phpMyAdmin)

W tle działa baza danych przygotowana przez CKE. Należy podłączyć plik `.sql` w XAMPP.
Uruchom `phpMyAdmin`, stwórz nową bazę danych o nazwie `wycieczki` z kodowaniem np. `utf8_unicode_ci` i wykonaj import pliku `wycieczki.sql`. Wykonaj następujące sprawdzenia z polecenia:

- **Pobranie miejsca i dni (cena poniżej 1000zł):** `SELECT miejsce, liczbaDni FROM wycieczki WHERE cena < 1000;`
- **Średnia cena z grupowaniem i aliasem:** `SELECT liczbaDni, AVG(cena) AS "sredniaCena" FROM wycieczki GROUP BY liczbaDni;`
- **Łączenie relacyjne (relacja Join, wycieczki pow. 500zł):** `SELECT w.miejsce, z.nazwa FROM wycieczki w JOIN zdjecia z ON w.id = z.Wycieczki_id WHERE w.cena > 500;`
- **Utworzenie wirtualnego konta pracowniczego:** `CREATE USER 'Ewa'@'localhost' IDENTIFIED BY 'Ewa!Ewa';` Zapisz te reguły w dokumencie `kwerendy.txt` i przygotuj polecone w arkuszu zrzuty ekranów.

> [!TIP]
> **[Alternatywa Nowoczesna] Połączenie z bazą (Backend)**
> Choć to zadanie nie ma w kodzie PHP, często musisz je użyć na egzaminach łączonych. Egzamin dopuszcza użycie funkcji proceduralnych `mysqli_...`. W branży używa się dziś podejścia obiektowego **PDO**. Jest przejrzystsze i znacznie bardziej uodpornione na wstrzykiwanie obcych zapytań SQL przez hakerów.
> **Gotowy kod PDO do podłączenia:**
>
> ```php
> <?php
> try {
>     $con = new PDO('mysql:host=localhost;dbname=wycieczki;charset=utf8', 'root', '');
> } catch(PDOException $e) {
>     echo "Błąd logowania bazy.";
> }
> ?>
> ```

---

## KROK 2: Logika Interfejsu (HTML) i Testowanie Wstępne

Gdy szkielet jest gotowy, budujemy układ strony, ściśle trzymając się obrysu z makiety.

```html
<header id="baner"><h1>Zwiedzamy Polskę</h1></header>

<!-- Blok obsługujący strzałkę 'poprzednie'. Klasycznie - znak mniejszy to zmienna wcięta &lt; -->
<nav id="lewy">
  <button onclick="poprzednie()">&lt;</button>
</nav>

<main id="srodkowy">
  <!-- Tu osadzono tzw. Aktywne zdjęcie o wymuszonym ID "glowneZdjece" by łatwo je modyfikować w JS -->
  <img src="1.jpg" alt="Aktywne zdjęcie" id="glowneZdjece" />
</main>

<!-- Blok obsługujący strzałkę 'następne', znak większy to encja &gt; -->
<aside id="prawy">
  <button onclick="nastepne()">&gt;</button>
</aside>

<!-- Blok mini-galerii ładujący na raz wszystko -->
<section id="miniatur">
  <img src="1.jpg" alt="Gdańsk" />
  <img src="2.jpg" alt="Kraków" />
  <img src="3.jpg" alt="Niedzica" />
  <img src="4.jpg" alt="Pieniny" />
  <img src="5.jpg" alt="Szklarska Poręba" />
  <img src="6.jpg" alt="Tatry" />
  <img src="7.jpg" alt="Wrocław" />
</section>

<footer id="stopki">
  <h3>Autorem galerii jest:</h3>
  <p>TWÓJ NUMER PESEL</p>
  <a href="http://pixabay.com" target="_blank">Więcej zdjęć</a>
</footer>
```

### 🔬 Testowanie Wstępne powiązań skryptu

Nigdy nie pisz całego złożonego skryptu JavaScript w ciemno! Należy sprawdzić, czy stworzony przycisk HTML poprawnie odwołuje do zadeklarowanej funkcji zapisanej w osobnym pliku `skrypt.js`.
Otwórz czysty plik `skrypt.js` i stwórz pojedynczy komunikat zwrotny przeglądarki, wykonujący testowe wyświetlanie prostego tekstu na ekran:

```javascript
function poprzednie() {
  alert("Przycisk połączony ze skryptem poprawnie!");
}
```

Kliknij lewy boczny przycisk na stronie html. Pokazał Ci się alarm w oknie? Znaczy to, że link przez parametr `onclick="poprzednie()"` funkcjonuje idealnie, plik JS jest zaczytany i czas na kodowanie głównej funkcjonalności bez ryzyka błędów ścieżki pliku.

---

## KROK 3: Określanie Reguł CSS i Trening Błędów

Największym testem inżynieryjnym podczas używania kaskadowych arkuszy stylów (CSS) na egzaminach INF.03 jest zarządzanie przestrzenią z wykorzystaniem opływania (`float: left;`). Ustawimy szerokości skrajne okien na 15%, a środkowe na 70%.

### 🔥 Trening Błędów: Błąd Przylegania Bloków

Jeśli wpiszesz własności lewitowania elementów (`float`) tylko do bocznych kolumn i zignorujesz pozostałe bloki, blok pod spodem (tutaj w zadaniu to sekcja miniatur `#miniatur`) "wśliźnie" się i wejdzie pod spód sekcji głównych, tworząc chaotyczny wygląd, gdzie sekcja naglówkowa i dół mieszają się razem.

- **Diagnoza inżynieryjna:** Mechanizm `float` wyłącza element z naturalnego nurtu strony. Blok umieszczony fizycznie bezpośrednio pod opływającym uważa, że ma do czynienia z pustką wzdłuż głównej pionowej osi dokumentu.
- **Procedura Naprawcza:** Na "upadający" blok przyległy lub tuż nad nim wprowadzamy bezwzględne żądanie czyszczenia stref lewitowania `clear: both;`. Przepchnie to dolny blok, zmuszając go do grzecznego startowania w linii dopiero poniżej najniższego barierowego punktu.

**Kod Bezpiecznego Układu `styl.css`:**

```css
body,
* {
  font-family: "Georgia", serif;
  color: white;
}

#baner,
#stopki {
  background-color: Maroon;
  text-align: center;
  padding: 2px;
}

/* Strażnik prawidłowego ułożenia layoutu (likwidacja "ślizgania w dół") */
#miniatur {
  background-color: Maroon;
  height: 70px;
  clear: both; /* TO ROZWIĄZUJE BŁĄD PRZYLEGANIA BLOKÓW WYŻEJ */
}

/* Włączenie podziału na procentowe kolumny i wymuszenie ich opływu */
#lewy,
#srodkowy,
#prawy {
  background-color: LightSalmon;
  height: 527px;
  float: left;
}
#lewy,
#prawy {
  width: 15%;
}
#srodkowy {
  width: 70%;
}

/* Dostrajanie elementów guzików zgodnie z instrukcją */
button {
  background-color: LightSalmon;
  color: Maroon;
  border: none;
  font-size: 400%;
  display: block;
  margin: 0 auto;
  margin-top: 210px;
}

/* Animacja i skalowanie centralnego zdjęcia przy użyciu Transition CKE */
#srodkowy img {
  display: block;
  margin: 0 auto;
  margin-top: 45px;
  transform: scale(1);
  transition: transform 5s; /* Animacja trwająca 5 sekund rozszerzenia */
}
#srodkowy img:hover {
  transform: scale(1.2);
}

/* Płynna klatkowa animacja miniatur wprowadzająca suwak do strony z @keyframes */
#miniatur img {
  height: 70px;
  margin-left: 0px;
  animation: wprowadz 4s ease-out;
}
@keyframes wprowadz {
  0% {
    margin-left: 50px;
  }
  100% {
    margin-left: 0px;
  }
}
```

> [!TIP]
> **[Alternatywa Nowoczesna] Moduł układu CSS Flexbox**
> Wykorzystywanie pływania `float` do organizowania głównych bloków odeszło z produkcji webowych wiele lat temu z powodu właśnie takich frustrujących błędów przylegania. Zostało to zastąpione elastycznym modułem Flex. Wprowadzenie elementu otulającego sekcje uwalnia z przymusu czyszczenia floatów. Flex automatycznie wymienia wielkość i organizuje osie X oraz Y.
>
> **Gotowy kod Flexboxa (do zastąpienia przestarzałego float):**
> Należy w HTML otulić 3 sekcje nawigacji divem bazowym np: `<div id="ukladKorytarza"> <nav id="lewy">.. <main..> <aside..> </div>`, po czym w CSS dopisać:
>
> ```css
> #ukladKorytarza {
>   display: flex; /* Aktywuj tryb siatki horyzontalnej */
>   flex-direction: row;
>   /* Brak konieczności stosowania Clear zarówno w górze ani poniżej! */
> }
> #lewy,
> #prawy {
>   width: 15%;
>   height: 527px;
>   background-color: LightSalmon;
> }
> #srodkowy {
>   width: 70%;
>   height: 527px;
>   background-color: LightSalmon;
> }
> ```

---

## KROK 4: Logika Skryptowa, Zastosowanie F12 i Mapy Myśli

Nasze zadanie po naciśnięciu strzałek powoduje proste rotowanie zdjęcia docelowego.

### 🧰 Zastosowanie Narzędzi Deweloperskich (Konsola F12)

Rozpoczynając pisanie logiki, zapamiętaj złotą regułę: **Aplikacje JavaScript "psują się po cichu"**. Jeżeli naciśniesz strzałkę, a zdjęcie się nie podmieni, prawdopodobnie nie zrobiłeś na stronie nic poza wpisaniem jednej, złej litery we wbudowanej komendzie z biblioteki DOM.

- Zamiast sprawdzać kod z czystej dedukcji – wciśnij klawisz **F12** w przeglądarce. Wejdź na czerwoną zakładkę **Console**.
- Jeśli przykładowo wpisałeś w kod usterkę `document.getElementsById(...)` (błąd metody używającej litery 's' w środku w połączeniu z dopiskiem jednorazowego obiektu 'Id'), wyświetlony zostanie komunikat błędu silnika renderującego o treści: `Uncaught TypeError: document.getElementsById is not a function`.
- Konkretna informacja typu "To nie jest znana przeglądarce funkcja" jasno nakierowuje Cię, że to zwykła literówka systemowa, uwalniając Cię od szukania winy w złym przekazywaniu samej w sobie matematycznej struktury algorytmu.

### 🗺 Mapy Myśli Algorytmicznych

Ustaw przed zadeklarowaniem działania skryptu mikro-kroki swojego algorytmu. Oddziel logikę na fragmenty mniejsze, pożerając problem kawałek po kawałku. Błędem wywołującym panikę jest pisanie całości z zamysłem że obok numeru musi od razu iść plik z logiką stringa.

```javascript
/* KROKI ALGORYTMU ZMIANY OBRAZKA W SLIDERZE
    Zmienna robocza globalna: Pamietaj ktory obraz aktualnie wyświetlasz (startowo to fot 1).

    Funkcja Akcji 'Następne':
    1. Cel kontrolny i warunki brzegowe: czy jest to już fot. 7? 
       Jeśli tak to wciśnięcie następnego ma wymusić "reset i odnowę", a więc narzuc z boku nr 1.
    2. Jeżeli nie: dopisz do rotacji po prostu powiększone oczko (wartość + 1).
    3. Przycisk działa. Pozostaje jedynie nadpisać wyświetlany obraz. Narzuć atrybut src z wytyczną ścieżki ("nrZdjecia" + ".jpg") do zdjęcia głównego na domenie.
*/
```

### Kod docelowy funkcji modyfikującej (w pliku skrypt.js):

```javascript
let wybranySlajd = 1;

function nastepne() {
  if (wybranySlajd === 7) {
    wybranySlajd = 1; // zapięcie pętli galerii na krańcu blatu w przód
  } else {
    wybranySlajd++;
  }
  // Przypisanie atrybutu .src fizycznemu elementowi strony.
  document.getElementById("glowneZdjece").src = wybranySlajd + ".jpg";
}

function poprzednie() {
  if (wybranySlajd === 1) {
    wybranySlajd = 7; // odbicie pętli przy dolnej podstawie w tył
  } else {
    wybranySlajd--;
  }

  document.getElementById("glowneZdjece").src = wybranySlajd + ".jpg";
}
```

> [!TIP]
> **[Alternatywa Nowoczesna] Obiekty Listenujące (addEventListener)**
> Deklarowanie uruchomień z wewnątrz HTMLa (za pomocą `onclick="..."`) zaciska twarde powiązanie ze zdarzeniami na poziomie layoutu. Inżynierzy dążą do "Separation of concerns" - gdzie plik z mark-upem HTML ma w ogóle "nie wiedzieć", że istnieje cokolwiek takiego w nim jak moduł wyzwolonych zmienności. Dokonuje się tego stosowaniem zewnętrznych Listenerów, wbudowanych całkowicie tylko do izolowanego zewnętrznie kodu `skrypt.js`.
>
> **Gotowy kod wariantu zdarzeń asynchronicznych (podmień zawartość js):**
> Zakładając, ze nadałeś w obiekcie DOM guzika lewego w HTML parametr id na `<button id="przyciskTyl">`, wpisujesz:
>
> ```javascript
> let wybranySlajd2 = 1;
> const objGlowne = document.getElementById("glowneZdjece");
>
> // Podpina moduł działania słuchający cichego uderzenia na guziku
> document.getElementById("przyciskTyl").addEventListener("click", function () {
>   wybranySlajd2 = wybranySlajd2 === 1 ? 7 : wybranySlajd2 - 1;
>   objGlowne.src = wybranySlajd2 + ".jpg";
> });
>
> document
>   .getElementById("przyciskPrzod")
>   .addEventListener("click", function () {
>     wybranySlajd2 = wybranySlajd2 === 7 ? 1 : wybranySlajd2 + 1;
>     objGlowne.src = wybranySlajd2 + ".jpg";
>   });
> ```

Pamiętając o sztywnych wytycznych rozładowywania napięć algorytmicznych - unikniesz potknięć, paniki po awarii jednego atrybutu lub uszkodzeń logiki aplikacji. Ten arkusz został w ten sposób uproszczony do ustandaryzowanego minimum zawodowego. Powodzenia.
