# AI-praksa
# Rešenje Data Science zadataka

Ovaj repozitorijum sadrži rešenja zadataka, uključujući obradu podataka, semantičku pretragu i kreiranje prediktivnog modela.

## Komentar o implementaciji i donošenju odluka (Zadatak 3)

Kao **najzahtevniji**, a ujedno i **najzanimljiviji** deo celog projekta, izdvojio bih treći zadatak koji se bavi predviđanjem nedeljne prodaje (`Weekly_Sales`) na osnovu Walmart skupa podataka.

### Zašto je ovaj deo bio najzahtevniji?
Najveći izazov nije bio u samom obučavanju modela, već u **pripremi podataka** pre nego što oni uopšte stignu do mašinskog učenja. Zahtevni delovi su obuhvatali:
1. **Izvlačenje meseca iz datuma unosa:** Kolona sa datumom je inicijalno bila u tekstualnom formatu, pa je prva odluka bila da je konvertujem u pravi `datetime` format, a zatim pomoću `.dt.month` izvučem mesec kao zaseban parametar koji model može da razume.
2. **Računanje "srednje zarade" po prodavnici:** Bilo je potrebno izračunati istorijski prosek prodaje za svaku pojedinačnu prodavnicu i taj podatak preslikati nazad na svaki red u tabeli.

### Zašto je implementirano baš tako i koje su odluke donete?
* **Odluka o grupisanju i mapiranju:** Za kreiranje kolone `srednja_zarada`, umesto komplikovanih operacija spajanja tabela (`pd.merge`), izabrao sam kombinaciju `.groupby()` i `.map()` metoda:
  ```python
  srednja = df.groupby('Store')['Weekly_Sales'].mean()
  df['srednja_zarada'] = df['Store'].map(srednja)
Ova odluka je doneta jer `.map()` završava ceo posao u samo jednoj liniji koda. Ona ne menja strukturu tabele niti broj redova, što smanjuje šansu za grešku i čini kod maksimalno efikasnim i lakim za čitanje.

* **Odluka o podeli podataka:** Umesto klasične nasumične podele (`train_test_split`), podatke sam podelio prateći zahteve iz teksta zadatka. 

### Zašto je ovaj deo bio najzanimljiviji?
Najzanimljiviji deo mi je bila sama implementacija linearne regresije. Iako je to najosnovniji algoritam, bilo je korisno videti kako daje sasvim korektne rezultate čim se podaci (poput meseca i srednje zarade) dobro pripreme unapred. Takođe, računanje MSE metrike na kraju daje jasnu i realnu sliku o tome koliko model zapravo greši u praksi.
