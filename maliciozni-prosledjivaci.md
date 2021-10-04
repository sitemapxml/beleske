# Dodat je maliciozni prosleđivač (forwarder)


## Email obaveštenje
 - email obaveštenje o kreiranju prosleđivača je stiglo u folder **cPanel** ► **Security** na admin adresu.

Ukoliko navedena email adresa za prosleđivanje izgleda sumnjivo, postupak za dalju proveru je sledeći: 

 - pronaći hosting nalog za navedenu email adresu putem **WHM-a**
 - u odeljku **Products** ► **Services** proveriti na kom se serveru nalaze podaci korisnika.
 - pronaći IP adresu sa koje je izmenjen prosleđivač. povezati se iz terminala sa odgovarajućim serverom i proveriti **access log**

## Provera **access_log** fajla
Putanja do fajla je sledeća: <br>`/usr/local/cpanel/logs/access_log` 
 
Poželjno je da se access log prikaže sa cat a zatim preusmeri na `grep` prema email adresi naloga sa koga je pristupljeno cPanel-u i na kraju još jedan `grep` prema `addfwd` stranici sa koje je izvršena promena. Cela komanda izgleda ovako:
```
sudo cat /usr/local/cpanel/logs/access_log | grep korisnik%40example.com | grep addfwd
```
> NAPOMENA: Znak **@** u email adresi mora da bude kodiran sa **%40** zato što je tako prikazan i u access_log fajlu. Ceo access_log  je najbolje prikazati sa less. Za pretraživanje celog log fajla unazad može se koristiti upitnik (?)

Ukoliko je u log fajlu pronađen traženi string, biće prikazano nešto slično kao u primeru ispod:

```
185.246.208.134 - korisnik%40example.com [08/18/2021:13:23:26 -0000] "GET /cpsess4233906373/webmail/paper_lantern/mail/addfwd.html HTTP/1.1" 200 0 "https://host34.dwhost.net:2096/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.131 Safari/537.36" "s" "-" 2096
185.246.208.134 - korisnik%40example.com [08/18/2021:13:23:31 -0000] "POST /cpsess4233906373/webmail/paper_lantern/mail/doaddfwd.html HTTP/1.1" 200 0 "https://host34.dwhost.net:2096/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.131 Safari/537.36" "s" "-" 2096
```

## Provera geografskog porekla IP adrese
Uraditi `whois` proveru IP adrese navedene u `access_log` fajlu.

Ukoliko se utvrdi da je geografsko poreklo IP adrese neka udaljena zemlja, najverovatnije se radi o zloupotrebi. U tom slučaju potrebno je postupiti prema sledećem nizu koraka:

## Uklanjanje zlonamernog prosleđivača i/ili filtera
 - ulogovati se u kompromitovani cPanel nalog
 - otvoriti stranicu `Forwarders` i napraviti snimak ekrana
 - obrisati zlonamerne prosleđivače
 - nakon toga vratiti se na početnu stranicu cPanel-a i otvoriti stranicu `Email Filters`
 - za svaki od email filtera otvoriti stranicu `Manage` i proveriti da li ima zlonamernih filtera. Ukloniti ukoliko su isti nađeni.

## Resetovanje lozinke
Lozinka se **resetuje** samo ako kompromitovana email adresa **nije glavna** u nalogu.
Ukoliko je kompromitovana email adresa istovremeno **glavna** email adresa u nalogu, lozinku **ne treba** resetovati.

Postupak za resetovanje lozinke:
Lozinka email naloga se resetuje preko **dwh** skripte.
potrebno je pokrenuti SSH sesiju sa odgovarajućim serverom i uneti sledeću komandu: 
```
sudo dwh -be email_korisnika

# ili, drugi način
sudo dwh --block --email email_korisnika
```

## Kreiranje tiketa

 - naslov tiketa:
 `Kompromitovan email nalog ( korisnik / user@example.com )`
 - Kreirani tiket koristi templejt `Kompromitovan email nalog (Forwarder)` koji se može pronaći traženjem pomoću ključne reči **Forwarder**
 - Urediti detalje u tekstu u skladu sa slučajem.
 - dodati linkove do fotografija (zlonamernih prosleđivača i filtera)
<br>

U tiketu je potrebno odabrati sledeće opcije:
 1. **Department** ► **Abuse**
 2. **Status** ► **Awaiting Client Response**
 3. **Priority** ► **Normal**
 4. **Assigned** ► Ostaviti kao što jeste
 5. **In Desk** ► **srpski**

Takođe, potrebno je u polje **Server** uneti **naziv** odgovarajućeg servera.

Postaviti komentar klikom na dugme **Add to ticket**
Nakon što se pojavi poruka sa odabirom za naknadne radnje, odabrati opciju **View ticket**.

## Nakon postavljanja tiketa

### Dodavanje komentara za administratore
Nakon postavljanja tiketa, potrebno je nalepiti odgovarajući deo iz **access_log** fajla kao komentar.

### Postavljanje datuma isteka (**Due Date**)
 1. Ukoliko je lozinka **resetovana**, može se postaviti veći vremenski period, okvirno **48 sati**. 
 2. Ukoliko lozinka **nije resetovana** (ostavljeno je korisniku da samostalno resetuje lozinku), datum isteka se stavlja narednog dana u pre-podnevnim časovima. 
:warning: Potrebno je imati u vidu da u ovom slučaju napadač može ponovo da kreira zlonamerne prosleđivače i filtere, pre nego što je korisnik imao vremena da reaguje. Iz tog razloga je poželjno proveriti još jednom prisustvo neželjenih promena u nalogu pre otvaranja tiketa.

