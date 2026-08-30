# Kanał aktualizacji SpacerNext

Kanał programu jest domyślnie połączony z `https://github.com/Risepolak/SpacerNext.git`, gałąź `master`.

Najprościej uruchomić samodzielny graficzny publikator `tools\SpacerNextUpdatePublisher.exe`. Pozwala zalogować właściwe konto, podać wersję i listę zmian, a następnie pokazuje przebieg budowania oraz wysyłania. Plik CMD jest tylko opcjonalnym skrótem uruchamiającym ten sam EXE; PowerShell nie jest wymagany do obsługi okna publikatora.

1. Zaloguj Git do GitHub (np. Git Credential Manager) i upewnij się, że `origin` wskazuje na repozytorium SpacerNext.

```powershell
git credential-manager github list
git credential-manager github login
git remote set-url origin https://Risepolak@github.com/Risepolak/SpacerNext.git
```

W tym komputerze zapisane jest także konto `Xardas17`. Nazwa `Risepolak@` w adresie powoduje, że publikator wybierze właściwe konto będące właścicielem repozytorium.
2. Zwiększ `AssemblyVersion` i `AssemblyFileVersion` w `WorldEditorCore/SpaceEditor/Properties/AssemblyInfo.cs`.
3. Utwórz pakiet i manifest:

```powershell
.\tools\Publish-SpacerNextUpdate.ps1 -Version 0.1.5.0 -Notes "Opis zmian"
```

4. Sprawdź wygenerowany ZIP i manifest, zatwierdź je i wyślij na skonfigurowaną gałąź. Można też wykonać publikację automatycznie:

```powershell
.\tools\Publish-SpacerNextUpdate.ps1 -Version 0.1.5.0 -Notes "Opis zmian" -Branch master -Push
```

Skrypt buduje program w katalogu tymczasowym, dlatego można przygotować wydanie nawet wtedy, gdy roboczy SpacerNext jest uruchomiony. Do repozytorium dodaje wyłącznie manifest i ZIP kanału aktualizacji.

Pakiet celowo nie zawiera `SpaceEditor.xml`, `SpacerNext.Update.xml`, logów ani plików PDB. Dzięki temu aktualizacja nie usuwa ustawień i układu okien użytkownika. Program użytkownika nie wymaga instalacji Git: manifest i ZIP są pobierane bezpośrednio z GitHub przez HTTPS. SpacerNext weryfikuje SHA-256 i pokazuje rzeczywisty postęp pobranych bajtów w prawym górnym rogu paska tytułu. Instalacja wymaga zapisania aktywnego ZEN/SPN; można ją bezpiecznie odroczyć.
