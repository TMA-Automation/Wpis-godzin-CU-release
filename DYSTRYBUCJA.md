# Instrukcja dystrybucji — Wpis godzin do ClickUp

Aplikacja to **pojedynczy plik `WpisGodzinCU.exe`** (PyInstaller, z wbudowanym Pythonem).
Użytkownik **nie instaluje niczego** i **nie potrzebuje uprawnień administratora** — kopiuje
plik i klika.

## 1. Skąd wziąć plik

Najnowszy plik jest w **Releases** repozytorium:

➡️ **https://github.com/TMA-Automation/Wpis-godzin-CU-release/releases/latest**
→ sekcja **Assets** → pobierz **`WpisGodzinCU.exe`**.

## 2. Gdzie umieścić plik

Masz dwie opcje:

### A) Lokalnie u każdego (najprostsze)
Skopiuj `WpisGodzinCU.exe` na komputer użytkownika, np. do
`C:\Users\<user>\TMA\WpisGodzinCU\WpisGodzinCU.exe`, i zrób skrót na pulpit.
Każdy ma własny plik; aktualizacje: podmień exe (lub użyj auto‑aktualizacji, p. niżej).

### B) Wspólny folder (OneDrive / dysk sieciowy)
Połóż `WpisGodzinCU.exe` w udostępnionym folderze, np.
`...\OneDrive\TMA\Aplikacje\WpisGodzinCU\`. Każdy uruchamia ten sam plik.
> Uwaga: przy uruchamianiu z OneDrive plik najpierw się synchronizuje — pierwsze
> uruchomienie u nowej osoby może chwilę potrwać.

## 3. Pierwsze uruchomienie (każdy użytkownik)

1. Uruchom `WpisGodzinCU.exe`.
2. Kliknij koło ⚙ (Ustawienia):
   - **Token API ClickUp** — każdy wkleja **swój** token
     (ClickUp → Settings → Apps → API Token).
   - **Wykryj** → wybierz **Team ID**.
   - (Zespołowo) **Wspólny plik danych** — wskaż ten sam dla wszystkich (p. niżej).
3. Na ekranie głównym wskaż **plik/folder z godzinami** (domyślnie folder KADRY).

## 4. Wspólne dane w zespole (opcjonalnie)

Aby wszyscy mieli **te same projekty, wpisy i szablony** (a token i ścieżki zostały
prywatne):

1. Wybierz jedną lokalizację na **wspólny plik**, np.
   `...\OneDrive\TMA\WpisGodzinCU\config.json` (albo na dysku sieciowym `\\serwer\...`).
2. Każdy w ⚙ Ustawienia → **„Wspólny plik danych"** wskazuje **dokładnie ten sam** plik.
3. Od teraz projekty/wpisy/szablony są wspólne.

**Równoczesna praca:** kto pierwszy otworzy program — ten edytuje; kolejne osoby wchodzą
w **tryb tylko‑do‑odczytu** (czerwony pasek „🔒 edytuje: X") i mogą przeliczać oraz wysyłać,
ale nie zmieniać projektów/wpisów. Przyciski **„Odśwież dane"** i **„Przejmij edycję"**.
> Dysk sieciowy SMB jest pewniejszy niż OneDrive przy równoczesnej edycji
> (OneDrive synchronizuje z opóźnieniem — blokada jest „best‑effort").

## 5. Aktualizacje

Aplikacja po starcie sprawdza, czy w repo jest nowsza wersja (`version.txt`), i pokazuje
żółty pasek **„⬆ Dostępna aktualizacja"** z przyciskiem do pobrania nowego `.exe` z Releases.
Wystarczy pobrać i podmienić plik.

## 6. Pierwsze uruchomienie a Windows SmartScreen

Plik nie jest podpisany cyfrowo, więc Windows może pokazać **„Windows ochronił Twój
komputer"**. Kliknij **„Więcej informacji" → „Uruchom mimo to"**. To jednorazowe.
(Aby to wyeliminować, potrzebny jest certyfikat code‑signing.)

## 7. Wydanie nowej wersji (dla utrzymującego kod)

```powershell
# 1) podbij numer w version.txt (np. 1.01 -> 1.02)
# 2) zbuduj:
./build.ps1                       # testy + PyInstaller -> dist\WpisGodzinCU.exe
# 3) commit + tag + release z assetem:
git commit -am "Wersja 1.02"; git push
gh release create v1.02 dist\WpisGodzinCU.exe -R TMA-Automation/Wpis-godzin-CU --title "v1.02" --notes "..."
```
`version.txt` w repo MUSI być równe numerowi tagu (`v<wersja>`), inaczej auto‑aktualizacja
nie zadziała. Asset zawsze nazywaj `WpisGodzinCU.exe`.
