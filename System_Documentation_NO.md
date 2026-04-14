# System Dokumentasjon

<table align="center">
  <tr>
    <td><b><a href="System_Documentation_EN.md"><code>English</code></a></b></td>
    <td><a href="System_Documentation_NO.md">Norwegian</a></td>
  </tr>
</table>

## Oppgave

* Utvikle en fotobooking-applikasjon hvor brukere kan booke fotoshoots. En fotoshoot må minst ha en dato, en lokasjon, en beskrivelse og et maks antall personer. Brukere må oppgi navn og telefonnummer etter booking.
* Fotoshoot-teamet har kun kapasitet til å gjennomføre 3 bookinger per dag.

<details>
<Summary> Teknologi </Summary>

* Omega har innebygd sikkerhet, noe som gjør det enkelt å komme i gang.

* Omega har også mange innebygde komponenter, som gjør det mye enklere enn å lage alt selv.

* Omega sitt rammeverk bruker Vue.js, Bootstrap og Microsoft SQL Server, som er gode verktøy for å lage apper og en solid backend.

* Jeg brukte drawSQL til å lage tabellstrukturen min

</details>

<details>

  <summary>Arkitektur</summary>

  <details>
    <summary>Table Setup</summary>

[Table Setup](https://drawsql.app/teams/hocus/diagrams/foto-booking)

  <!-- Bilde beholdes som i original -->

  </details>

  <details>
    <summary> Views </summary>

| View Name                       | Description                                                                                                                                                  | Image |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------- |
| aviw_aHakon_MyBookings          | Dette viewet eksisterer fordi jeg trenger å vise all relevant informasjon for en booking, så jeg joiner PhotoBookingDetails, PhotoBookings og system persons | <img width="774" height="448" alt="image" src="https://github.com/user-attachments/assets/04b4459a-b48c-45ad-93c2-6e0f91d76c57" /> |
| aviw_aHakon_PhotoBookingDetails | Dette viewet brukes for å vise relevant informasjon i booking-kortet. Jeg trenger tittel, beskrivelse og lokasjon fra PhotoBookings, samt detaljer           | <img width="720" height="328" alt="image" src="https://github.com/user-attachments/assets/00ed2d7b-9936-4e42-a272-457d48749c7c" /> |

  </details>

  <details>
  <summary>Prossedyrer</summary>

| Procedure Name                | Description                                                                                                                                                                            | Image |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| astp_aHakon_CreateNewBooking  | Tar inn all nødvendig bookingdata som parametere. Etter insert bruker den SCOPE_IDENTITY() for å hente ID-en til den nye bookingen, som brukes til å koble detaljer til riktig booking | <img width="675" height="545" alt="image" src="https://github.com/user-attachments/assets/85907335-c9d0-4808-971b-ec052ef44b18" /> |
| astp_aHakon_CreateBooking     | Setter inn i atbl_aHakon_PersonsBookings-tabellen. Har også sjekk for å hindre dobbeltbooking og sjekk for om dagen er fullbooket                                                      | <img width="742" height="680" alt="image" src="https://github.com/user-attachments/assets/f2ac2f0e-e1d9-4041-9d99-488f117a1d8b" /> |
| astp_aHakon_GetBookingsByDate | Sjekker om en dag har 3 eller flere bookinger                                                                                                                                          | <img width="605" height="288" alt="image" src="https://github.com/user-attachments/assets/c2fa2f10-38bb-4e93-ad5a-594c8620df7e" /> |
| astp_aHakon_CancelBooking     | Brukes for å kansellere bookinger. Har også sjekk for å hindre at brukere sletter andres bookinger                                                                                     | <img width="505" height="383" alt="image" src="https://github.com/user-attachments/assets/e62eed3e-c1fd-41e4-86e6-9757c65b6b10" /> |
| astp_aHakon_BookingConfirmation | Denne sender ein SMS til brukeren for å bekrefte bookingen deiras, sender Email viss ingen telefonnummer er funnet. | <img width="1338" height="1130" alt="image" src="https://github.com/user-attachments/assets/a224d1d2-3866-4e54-8039-15069eb1e955" />

  </details>

</details>

<details>
<summary> Sikkerhet </summary>

Autentisering skjer via Omega365CTP (Core Technology Platform). Når brukere besøker siden, møter de en innloggingsside hvor de kan velge mellom Microsoft-innlogging eller SQL-login (epost/telefon + passord).

Hvis brukeren velger Microsoft, skjer autentiseringen hos Microsoft, og systemet returnerer en token som er koblet til brukeren.

---

### Tilgangskontroll

I Omega365 konfigureres tilgang gjennom access-tabeller. Brukere blir tildelt roller, og hver rolle bestemmer hvilke moduler brukeren har tilgang til.

Moduler inneholder informasjon om hvilke apper brukeren kan bruke og hvilke tabeller de kan hente data fra.

Det spesifiseres også om brukeren har lov til å redigere (AllowEdit) eller slette (AllowDelete). I tillegg kan roller ha capabilities, som gir mer detaljerte rettigheter.

Alle roller må være knyttet til en OrgUnit.

---

### 2FA

Tofaktorautentisering anbefales sterkt. Alternativer inkluderer:

* SMS og e-post (minst sikre, men bedre enn ingenting)
* TOTP (f.eks. Microsoft Authenticator og Google Authenticator)
* Passkeys (passordfri løsning knyttet til en enhet)

---

### OrgUnits

OrgUnits er bygget opp hierarkisk hvor alle har en parent.

* Brukere arver tilgang nedover i hierarkiet
* "DoNotInherit" kan brukes for å hindre arv
* Brukere ser kun data fra sin aktive OrgUnit og underliggende enheter

---

### Table security

* atbv-views brukes for å kontrollere tilgang via WHERE-clause
* aviw-views brukes for å hente data fra flere tabeller samtidig som sikkerhet beholdes
* I tillegg finnes egne sikkerhetssjekker i triggere

I mitt tilfelle:

* Kun brukere med Photography User eller Photographer-rolle har tilgang
* Kun Photographer kan se og redigere fotoshoots

---

### Triggers

* Alle triggere sjekker sviw_System_MyAccessTables for rettigheter (AllowEdit / AllowDelete)
* Dette er laget med SQL Templates
* Ekstra sjekk hindrer overlappende tider på bookinger

</details>

<details>
  <summary>App Structure</summary>

Se [User Manual](User_Manual_NO.md) for gjennomgang.

---

Totalt har jeg laget 3 apper:

1. Kalender (startside)
2. Booking Dashboard

   * Opprette booking
   * Se egne bookinger
   * Redigere fotoshoots (med riktige rettigheter)
3. Booking-side

---

Flyt:

* Klikk på dato → dato sendes via URL
* Klikk på booking → booking_ID + dato sendes
* Bruker sendes til detaljside hvor de kan booke
* Etter booking → sendes tilbake til dashboard

---

Frontend er laget i Vue.js (TypeScript) med hovedsakelig custom CSS og noe Bootstrap.

</details> 

<details>
<summary> Utfordringer under utvikling </summary>

### Users claiming bookings

* Tidligere hadde jeg ikke PersonsBookings-tabellen
* Brukere "claimet" en booking ved å få Person_ID lagt inn
* Dette fungerte dårlig

#### Løsning

* Opprettet egen tabell (atbl_aHakon_PersonsBookings)
* Hver booking blir en egen rad
* Mye ryddigere løsning

---

### Kunne opprette booking uten detaljer

* Bookinger uten detaljer dukket opp i dashboardet
* Hvis man klikket på dem, viste de ingenting

#### Løsning

* Lagde en modal som krever input før opprettelse
* Nå må alle nødvendige felt fylles ut

</details>

<details> 

<summary> Manglende funksjonalitet </summary>

* Man kan endre dato i URL til f.eks. 1900 og fortsatt booke
* Bookinger blir ikke visuelt markert som fullbooket

</details>

<details>
<summary> Ekstra funksjonalitet jeg ikke rakk </summary>

* Statusfelt:

  * "Pending" når en booking opprettes
  * Admin kan godkjenne eller avslå

</details>
