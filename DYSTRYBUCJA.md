# Instrukcja dystrybucji — Wpis godzin do ClickUp

Aplikacja to **pojedynczy plik `WpisGodzinCU.exe`** (PyInstaller, z wbudowanym Pythonem).
Użytkownik **nie instaluje niczego** i **nie potrzebuje uprawnień administratora** — kopiuje
plik i klika.

## 1. Skąd wziąć plik

Najnowszy plik jest w **Releases** repozytorium:

➡️ **https://github.com/TMA-Automation/Wpis-godzin-CU-release/releases/latest**
→ sekcja **Assets** → pobierz **`WpisGodzinCU.exe`**.

## 2. Gdzie umieścić plik — WAŻNE

Skopiuj `WpisGodzinCU.exe` do **lokalnego folderu, który NIE jest synchronizowany
z OneDrive**, np.:

```
C:\WpisGodzinCU\WpisGodzinCU.exe
```

i zrób skrót na pulpit (skrót może być na pulpicie — sam plik nie).

> ⚠ **NIE uruchamiaj exe z OneDrive, Pulpitu ani Dokumentów!**
> W firmie Pulpit i Dokumenty są przekierowane do OneDrive. OneDrive blokuje/synchronizuje
> plik w trakcie uruchamiania, co kończy się błędami typu
> „Failed to load Python DLL" albo „Permission denied". Folder `C:\WpisGodzinCU\`
> jest bezpieczny. (Wspólny **plik danych** `config.json` MOŻE być na OneDrive —
> problem dotyczy tylko samego `.exe`.)

## 3. Pierwsze uruchomienie (każdy użytkownik)

1. Uruchom `WpisGodzinCU.exe`.
2. Kliknij koło ⚙ (Ustawienia):
   - **Token API ClickUp** — każdy wkleja **swój** token
     (ClickUp → Settings → Apps → API Token).
   - **Wykryj** → wybierz **Team ID**.
   - (Zespołowo) **Wspólny plik danych** — wskaż ten sam dla wszystkich (p. niżej).
3. Na ekranie głównym wskaż **plik/folder z godzinami** (domyślnie folder KADRY).

## 4. Typowe problemy przy pierwszym uruchomieniu

| Komunikat | Przyczyna | Rozwiązanie |
|---|---|---|
| „Windows ochronił Twój komputer" (SmartScreen) | exe niepodpisany cyfrowo | **Więcej informacji → Uruchom mimo to** (jednorazowe) |
| „Failed to load Python DLL … LoadLibrary" | brak Microsoft Visual C++ Redistributable **lub** exe leży na OneDrive | 1) przenieś exe do `C:\WpisGodzinCU\`; 2) zainstaluj **VC++ 2015–2022 x64**: https://aka.ms/vs/17/release/vc_redist.x64.exe |
| „Permission denied: …OneDrive…\\….exe" | exe uruchamiany z folderu OneDrive (np. Pulpit) | przenieś exe do `C:\WpisGodzinCU\` i uruchom stamtąd |
| „System Windows nie może uzyskać dostępu do… pliku" | plik zablokowany po pobraniu / polityka blokuje exe w Pobranych | prawy klik → Właściwości → **Odblokuj**; przenieś poza folder Pobrane |
| Plik znika po pobraniu | antywirus / kwarantanna | wyjątek u działu IT |

## 5. Wspólne dane w zespole (opcjonalnie)

Aby wszyscy mieli **te same projekty, wpisy i szablony** (a token i ścieżki zostały
prywatne):

1. Wybierz jedną lokalizację na **wspólny plik**, np.
   `...\OneDrive\TMA\WpisGodzinCU\config.json` (albo na dysku sieciowym `\\serwer\...`).
2. Każdy w ⚙ Ustawienia → **„Wspólny plik danych"** wskazuje **dokładnie ten sam** plik.
3. Od teraz projekty/wpisy/szablony są wspólne. Baner na górze pokazuje, czy Twoje dane
   są **zsynchronizowane** ze wspólnym plikiem; przyciski **„⬇ Pobierz z wspólnego"**
   i **„Różnice…"** pozwalają porównać i wyrównać stan.

**Równoczesna praca:** kto pierwszy otworzy program — ten edytuje; kolejne osoby wchodzą
w **tryb tylko‑do‑odczytu** (czerwony pasek „🔒 edytuje: X") i mogą przeliczać oraz wysyłać,
ale nie zmieniać projektów/wpisów. Przyciski **„Odśwież dane"** i **„Przejmij edycję"**.
> Dysk sieciowy SMB jest pewniejszy niż OneDrive przy równoczesnej edycji
> (OneDrive synchronizuje z opóźnieniem — blokada jest „best‑effort").

## 6. Aktualizacje

Aplikacja po starcie sprawdza, czy jest nowsza wersja, i pokazuje żółty pasek
**„⬆ Dostępna aktualizacja"**:

- **„⬇ Pobierz i zainstaluj"** — aplikacja sama pobiera nowy plik (sprawdza, czy pobrał
  się w całości), zamyka się, podmienia exe i uruchamia nową wersję.
- **„Otwórz stronę"** — ręczne pobranie z Releases.

Po podmianie SmartScreen może raz ostrzec ponownie (plik niepodpisany).

## 7. Wydanie nowej wersji (dla utrzymującego kod)

```powershell
# 1) podbij numer w version.txt (np. 1.03 -> 1.04) — w repo źródłowym ORAZ publicznym
# 2) zbuduj:
./build.ps1                       # testy + PyInstaller -> dist\WpisGodzinCU.exe
# 3) commit + push źródeł; potem release w repo PUBLICZNYM:
gh release create v1.04 dist\WpisGodzinCU.exe -R TMA-Automation/Wpis-godzin-CU-release --title "v1.04" --notes "..."
```
`version.txt` w repo publicznym MUSI być równe numerowi tagu (`v<wersja>`), inaczej
auto‑aktualizacja nie zadziała. Asset zawsze nazywaj `WpisGodzinCU.exe`.
Po każdej zmianie zachowania zaktualizuj też tę instrukcję (w obu repo).
