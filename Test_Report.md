# Test that all the functionality works as expected

<details>
<summary> Calendear app </summary>

| Functionality | Result | Description | Pictures |
|---------------|--------|-------------|----------|
| Check that you cant go back further than the current date | ✅ | works |<img width="2539" height="469" alt="image" src="https://github.com/user-attachments/assets/ed2fd3bb-0e35-43c0-bc7c-88fa558f99af" />|
| Days that are fully booked show up as red and you cant click on them | ✅ | works | <img width="2504" height="437" alt="image" src="https://github.com/user-attachments/assets/91c40806-167e-4cef-8041-aab965f192c2" />|
| Not able to go to a booking without selecting a date first | ✅ | works | |

</details>

<details>
<summary> photo-bookings </summary>
  
| Functionality | Result | Description | Pictures |
|---------------|--------|-------------|----------|
| when booking is full buttons are disabled | ✅ | works | <img width="863" height="206" alt="image" src="https://github.com/user-attachments/assets/9f21c0ed-0c0f-4a2c-93f1-aa936007670d" />|
| Going past UI | ❌ | You can spam through the disabled button and manage to get to the next page, you still cant book something since restrictions stop a user. |  |
| Creating booking works, and shows up in bookings tab | ✅ | works | <img width="843" height="203" alt="image" src="https://github.com/user-attachments/assets/4f8aac8d-11fb-406f-a4a2-8d48c9d613f5" />|
| Creating booking works, and shows up in bookings tab | ✅ | works | <img width="843" height="203" alt="image" src="https://github.com/user-attachments/assets/4f8aac8d-11fb-406f-a4a2-8d48c9d613f5" />|
| Not able to create bookings without details | ✅ | works |  |
| When creating a booking it shows up under My Bookings tab | ✅ | works | <img width="498" height="358" alt="image" src="https://github.com/user-attachments/assets/ddf48a6e-6fba-45ea-a703-431715e2b972" />|
| You get a message/email when booking | ✅❌ | works, sometimes sends a lot more data than necessary but works most of the time | <img width="320" height="350" alt="image" src="https://github.com/user-attachments/assets/ec4950cd-2bf2-4303-ae90-e272b539eccc" />|
| Cancelling booking | ✅ | works | <img width="349" height="130" alt="image" src="https://github.com/user-attachments/assets/e9f74f4b-e6a9-4c3d-b7d9-52556a53008a" />|
| Not able to overlap time | ✅ | works | <img width="1758" height="307" alt="image" src="https://github.com/user-attachments/assets/bd5626fe-08b8-46fe-a853-afadbf24d0e5" />|
| Not able to see other users bookings | ✅ | works | |
</details>
