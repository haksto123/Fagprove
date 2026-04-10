# Daglig dokumentasjon

<details>
<summary> 07.04.26 (Tirsdag) </summary>
<ul><li> Har skrevet planelggingen for fagprøven, begynnt litt med rolle styring for å få tilgang til systemet. </li></ul>
</details>

<details>
<summary> 08.04.26 (Onsdag) </summary>
<ul>
<li> 
Gått litt vekk ifra orignale planen. Hovudsiden er nå ein kalender der du klikker inne på ein dato, også blir du sendt til dashbordet der du kan velge kass type booking du vil ha (wedding photos, family photos, osv...)
Blitt ferdig med tilgangsjekkene og har satt opp roller til "User" og "Admin".
Strevde litt med selve kalender siden, fordi eg trur den beste måten var å lage den ifra scratch, men funker fjell nå! </li>
<li> Satt opp "SQL templates" for å lett endre sikkerthets sjekker til triggers og atbv-er eg har lagd </li>
<li> har begynnt på admin siden også der man kan opprette, oppdatere, og slette bookings </li>
  
<li> Planen nå er: Kalender -> Booking side -> booking detaljer </li>
</ul>
</details>

<details>
<summary> 09.04.26 (Torsdag) </summary>
<ul><li> 
  Begynnt på details siden til bookingene. Har lagd ein stored procedure for å oppdatere person_ID på ein booking, men føler at dette er ein dårlig måte å gjørr det på. 
  Lagde ein stored procedure for å telle kor mange bookings er gjort på ein dag sånn at eg kan stoppe ein bruker for å gå videre om det er 3 bookings på ein dag.  </li></ul>
</details>

<details>
<summary> 10.04.26 (Fredag) </summary>
<ul>
  <li> Lagte ein ny tabell i tabell strukten (atbl_aHakon_PersonsBookings) for å inserte inn bookings inn i denne tabellen istedenfor å oppdatere atbl_aHakon_PhotoBookingDetails med person_ID, siden dette blei ein del kluss og funket ikkje så bra. </li>
  <li> Har satt opp My Bookings tabben inne i details appen der man kan se kva slags bookings du har lagd på ein spesifik dato. </li>
  <li> Trur det grunnleggende er på plass nå ish, men mangler ein del UI fixes, også mangler eg ein cancel booking knapp. </li>
</ul>
</details>

<details>
<summary> 11.04.26 (Lørdag) </summary>
<ul><li> tekst </li></ul>
</details>

<details>
<summary> 12.04.26 (Søndag) </summary>
<ul><li> tekst </li></ul>
</details>

<details>
<summary> 13.04.26 (Mandag) </summary>
<ul><li> tekst </li></ul>
</details>

<details>
<summary> 14.04.26 (Tirsdag) </summary>
<ul><li> tekst </li></ul>
</details>
