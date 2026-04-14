# System Documentation

<table align="center">
  <tr>
    <td><b><a href="System_Documentation_EN.md"><code>English</code></a></b></td>
    <td><a href="System_Documentation_NO.md">Norwegian</a></td>
  </tr>
</table>

## Task

- Develop a photo booking application where users can book photoshoots. A photoshoot should minimum have a date, a location, a description, and a max amount of persons. Users should leave their name and phone number after the booking.
- The photoshoot team only has capacity to shoot 3 bookings a day.

<details>
<Summary> Technology </Summary>

- Omega has built in security, so it is easy to get started.
- Also Omega has a lot of built in components which makes it a lot easier than rather having to make them myself.
- Omega's framework uses, vue.js, Bootstrap, and Microsoft SQL server, which are very good tools for creating apps, and a good backend.

- I used drawSQL to create my table structure
</details>

<details>
  
  <summary>Architecture</summary>
  <details>
    <summary>Table Setup</summary>
  [Table Setup](https://drawsql.app/teams/hocus/diagrams/foto-booking)
    
  <img width="1158" height="683" alt="image" src="https://github.com/user-attachments/assets/b9f220e9-58ee-4d5e-b4b9-5680a419e828" />
  </details>


  <details>
    <summary> Views </summary>

| View Name | Description | Pictures |
|-----------------------|------|-------|
| aviw_aHakon_MyBookings  | This view exists because I need to show all relevant information for the booking, so I join in PhotoBookingDetails, PhotoBookings, and system persons   |  <img width="774" height="448" alt="image" src="https://github.com/user-attachments/assets/04b4459a-b48c-45ad-93c2-6e0f91d76c57" />
| aviw_aHakon_PhotoBookingDetails  | This view exists because I need to show relevant information for the booking card, I need title, description, and location from PhotoBookings, and I need to show the details | <img width="720" height="328" alt="image" src="https://github.com/user-attachments/assets/00ed2d7b-9936-4e42-a272-457d48749c7c" />
  </details>

  <details>
  <summary>Procedures</summary>

| Procedure Name | Description | Pictures |
|----------------|-------------|----------|
| astp_aHakon_CreateNewBooking | This procedure takes in all required booking data as parameters. After inserting the booking, it uses SCOPE_IDENTITY() to retrieve the ID of the newly created record, which is then used to link the booking details to the correct booking. | <img width="675" height="545" alt="image" src="https://github.com/user-attachments/assets/85907335-c9d0-4808-971b-ec052ef44b18" />
| astp_aHakon_CreateBooking | This procedure Inserts into the atbl_aHakon_PersonsBookings table, it also has a  check to make sure a person cant book something twice, and has a check if the day is fully booked. | <img width="742" height="680" alt="image" src="https://github.com/user-attachments/assets/f2ac2f0e-e1d9-4041-9d99-488f117a1d8b" />
| astp_aHakon_GetBookingsByDate | This procedure is a check to see if a day is has more or equal to 3 bookings in a day. | <img width="605" height="288" alt="image" src="https://github.com/user-attachments/assets/c2fa2f10-38bb-4e93-ad5a-594c8620df7e" />
| astp_aHakon_CancelBooking | This last procedure ive made is for canceling bookings a person has made. It also has a check to make sure you cant delete another persons booking | <img width="505" height="383" alt="image" src="https://github.com/user-attachments/assets/e62eed3e-c1fd-41e4-86e6-9757c65b6b10" />
| astp_aHakon_BookingConfirmation | This procedure sends an SMS with a confirmation of the booking, aswell sends an email if no phonenumber is attached to the person thats booking | <img width="1338" height="1130" alt="image" src="https://github.com/user-attachments/assets/a224d1d2-3866-4e54-8039-15069eb1e955" />

  </details>
</details>



<details>
<summary> Security </summary>
The website authentication happens through Omega365CTP (Core Technology Platform). When users visit the site they immediately get met with a login page. Here you can choose between logging in with microsoft, or use SQL-Login by writing their Mail/Phone number and password.
If the user chooses microsoft, the authentication happens internally at microsoft and it returns a token thats connected to the user in the system.



In Omega365, access control is configured through access tables. Users are assigned roles, and each role defines which modules the user has access to. 
The modules contain information about which applications the user can use and which tables they are allowed to retrieve data from.
It is also specified whether the user has permission to edit data (AllowEdit) or delete data (AllowDelete). In addition, roles can include capabilities, which are more granular permissions that grant users specific types of access.
Every role that is assigned must be linked to an OrgUnit.

# 2FA
In addition to this, two-factor authentication (2FA) is highly recommended. There are several available options, such as SMS, Email, Time-based One-Time Password (TOTP), and Passkeys. 
SMS and Email are generally considered the least secure methods, but they are still better than having no 2FA at all.

TOTP is based on one-time codes generated in authentication apps like Microsoft Authenticator and Google Authenticator after you have registered the service. 
Passkeys are a passwordless alternative where your login is directly linked to a device you own, such as your phone, and is used to verify your identity.

# OrgUnits
The orgunits are built around a hierarchy where all orgunits will have a parent orgunit. These orgunits can be different types, for example Company, Contract, Project etc. 
To get access to data a user needs to have role in an orgunit.
Inheritance grants access to underlying OrgUnits (orgunit descendants). By default, users inherit access downwards the OrgUnit hierarchy. 
This means that if you have a role assigned in a higher-level OrgUnit, you automatically have the same role in all OrgUnits beneath it.

Orgunits can also have "DoNotInherit" enabled in the orgunit settings. For these OrgUnits, having a role assigned in a higher level of the hierarchy is not enough. 
Only users with a role assigned directly to the DoNotInherit OrgUnit will be granted access.

# Context
Users also have a orgunit context they can choose to be in. Users will only see data depending on which orgunit they stand in and data in the underlying orgunits. 

### OrgUnit hierarchy example:

<img width="991" height="710" alt="image" src="https://github.com/user-attachments/assets/1d0cdb80-6984-4924-a407-ebb9241cf1ae" />

# Table security
For making sure that not everyone has access to every table, there exists views called atbv's that automatically get created when you make a new table.
These atbv's contain a whereclause that you can use to control who should have access to your table.
Then you can create an aviw that collects the data from the atbv so you can keep the security check. (you can also just use the atbv, but if you need columns from other tables you need to create an aviw)

There should also be a custom security check in the triggers for extra security.

- In my case I have created security checks in both the triggers and the atbv's. So anyone without the Photography user or the Photographer roles will not have access to this app, and tables.
- Aswell as anyone without Photographer role will not be able to see the edit photoshoots tab, since only admins should be able to see this tab.

# Triggers

- All my trigger have a check on sviw_System_MyAccessTables where it checks if AllowEdit = 1 (or AllowDelete = 1 for delete triggers)

  <img width="671" height="223" alt="image" src="https://github.com/user-attachments/assets/c7162241-9aa1-4281-ad64-473ca2819d00" />
- These checks were created using something called SQL Templates. So instead of manually writing in every check in all triggers, I run this code, and have these tokens at the top that make sure that these checks are only applied for triggers where the name space is aHakon, and it makes sure that the table has the column ID in it.
- <img width="721" height="353" alt="image" src="https://github.com/user-attachments/assets/7026275e-077c-4549-93e3-9e0d3ac5fb6e" />

- In the atbl_aHakon_PhotoBookingDetails_ITrig I also have an extra check to make sure you can't create bookings with times that overlap with eachother
- <img width="510" height="250" alt="image" src="https://github.com/user-attachments/assets/7ca757dc-ee41-4af6-8d35-3e8a7fdbee07" />

</details>

<details>
  <summary>App Structure</summary>

See the [User Manual](https://github.com/haksto123/provefagprove/blob/main/User_Manual.md) for a quick walkthrough.

---------------------------------------------------------------------
The total amount of apps I have created is 3. One for the Calendar (The Page you start on). One for the Booking Dashboard where you can enter a booking, see the bookings you have booked, and this is where you can edit photoshoots if you have the required permissions. 
And I have one app for where you can book.
When you click on a date in the calendar, the date you chose will be sent with in the URL. This is used for filtering each day as its own. 
Then when you click on a booking the booking_ID will be sent with in the URL aswell as the date, both of these used for filtering. 
You will then be sent to the booking details page where you can book a photoshoot, after booking you get automatically sent back to the booking dashboard where you can see your booking has been created.

I have made all the frontend using vue.js (typescript) and styled with mostly custom CSS, but I also use bootstrap.

</details> 

<details>
<summary> Challenges during development </summary>
  
### Users claiming bookings   
- In my old schema I did not have the Persons Bookings table. My original idea was that an admin creates a booking, then when a user books I just insert the Person_ID into that booking. As if a user "claims" that booking
- This was very bothersome, and did not work that well

#### Solution
- I came up with the solution that I needed another table (atbl_aHakon_PersonsBookings), so rather than updating the person_ID when someone books, I just insert all the necessary data as a new row in this table.
- This worked much better, and made everything in the app cleaner.

### Able to make a booking without details
- You were able to make a booking without any details. this was a problem becuase it would show up in the booking dashboard, and if someone clicked on it nothing would show up.

### Solution
- I fixed this by creating a button that opens a modal where a person has to write in all the necessary details.
- Now you cant create a booking without the details.
  
</details>

<details> 

<summary> Missed functionality </summary>

- If you change the date in the URL from 2026 to 1900, you can make a booking in the 1900's... This I didnt have time to fix.
- When you create a booking it doesnt get greyed out or anything so it looks like its still avliable. Users still cant book it since the security checks stops them, but it would be nice functionality to have.

</details>

<details>
<summary> extra functionality I didnt have time for </summary>
  
- Thought about adding a Status field. So when a person books the status is set as "Pending" until an admin approves or declines the booking.
</details>
