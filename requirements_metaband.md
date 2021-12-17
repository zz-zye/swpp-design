### Table of Contents

- [1. Project Abstract](#1-project-abstract)
- [2. Customer](#2-customer)
- [3. Competitive Landscape](#3-competitive-landscape)
- [4. User Stories](#4-user-stories)
- [5. User Interface Requirements](#5-user-interface-requirements)
- [Document Revision History](#document-revision-history)

## 1. Project Abstract

> MetaBand is a platform where band enthusiasts can easily create **_band cover contents_** regardless of time and space. A **_cover_** is defined as a performance of an existing song by other vocalists/instrumentalists. In MetaBand, you can **_record & upload_** song covers with your vocals or instruments, and **_listen_** to other people's covers.<br/> MetaBand specializes in connectivity between covers. Among the covers of a particular song,<br/> **1)** Anyone can listen to combinations of any covers simultaneously, and<br/> **2)** Anyone can record their own cover while listening to that combination and upload it.<br/> For example, a `guitarist G` may add guitar lines while listening to `vocalist V`'s vocals and `drummer D`'s drums. Another `vocalist V2` may hear `G`'s guitars and record their own vocals to it. These kinds of interactions make it possible to have limitless amount of **_cover combinations_**, and infinite possibilities for musical collaboration. MetaBand aims to be a place for people of any musical background, where band enthusiasts can easily participate in musical activities and experience connections with other people without any constraints of real-time or off-line meetings.

## 2. Customer

### General

- People who are interested in creating music covers
- People who want to find others to play music with

### Specific

- Beginners / Hobbyist musicians who want more experience of creating music
- Professional vocalist / instrumentalists who want to share their skill and collaborate with others
- Non-musicians who only want to listen to new and creative covers of existing songs

## 3. Competitive Landscape

### Competitors

**[Kompoz](https://www.kompoz.com)**

- Website focused on collaborating on people's compositions
- Available for vocals & all instruments
- Team-based collaboration on projects: One person starts a project and requests collaboration

![image](https://user-images.githubusercontent.com/77932817/136314822-9d2f96e5-cde2-4567-9921-a3e7e90552ca.png)

**[Everysing](https://www.everysing.com/town.sm)**

- App/Website focused on vocal covers
- Vocals only
- People can upload 'Duets' to celebrities or others singing
- Limited connectivity: A 'Duet' itself cannot be used again for other people to sing with

![image](https://user-images.githubusercontent.com/77932817/136315696-2b4c864f-f8ee-4f9e-be34-ed6a0c7d15a7.png)

**[YAMAHA Syncroom](https://syncroom.yamaha.com/)**

- Standalone program for real-time online music sessions
- Available for vocals & all instruments
- Directed towards more skilled players
- Syncing problems exist between players with long distance

### Differentiation

- **Song-based collaboration**
  - People can freely add to any other cover for the same song
  - More opportunities for collaboration compared to team-based system
  - More beginner friendly since people in team-based system tend to look for more experienced players
- **Full connectivity**
  - Any cover can be used as a basis for another cover
  - More opportunities for collaboration
- **Available for vocals & all instruments**
  - Broader customer spectrum
- **No real-time playing**
  - Safe from restrictions of real-time playing such as sync problems
  - More beginner friendly since real-time playing is harder than pre-recording

## 4. User Stories

Stories for future sprints are listed below.   
Backend is not implemented in Sprint 3, so all user stories below can only be fully implemented after Sprint 4.   
Until backend is completed, all data and server responses will be substituted with dummy data.

### **Player bar**

**Player bar plays song (Sprint 3)**

- Actors : `User`
- Preconditions :
  - In **Main page**
  - `User` selects song from the `recommendation list`
- Scenario :
  - When song is selected, `Player bar` appears at bottom of the page.
  - `Player bar` shows title, instrument icons, play control buttons(play, pause, previous/next) and like button.
    - The `previous/next` button will play the previous/next song in the `recommendation list`.
- Exception :
- Acceptance test :
  - `Recommendation list` should be properly displayed.
  - Selected song should be properly played.
  - Play control buttons should work.
  - The like button should work.

**_In Sprint 3, a dummy recommendation list will be used. There will be no recommendation algorithm._**

### **Main Page**

**Search Music (Sprint 3)**

- Actors : `User` or `Guest`
- Preconditions :
  - In **Main page**
  - Actor inputs search keyword in `search bar`
  - Actor clicks `Search` button
- Scenario :
  - List all songs that match keywords by matching order(from most relevant).
  - Show `Add song` button below the list, which loads **Create song page** when clicked.
- Exception :
  - If `Guest` clicks `Add song` button, move to **Sign in page**.
- Acceptance test :
  - The list should show all songs that match keywords.
  - The songs should be ordered by matching order.

**Listen to Music (Sprint 3)**

- Actors : `User` or `Guest`
- Preconditions :
  - In **Main page**
  - List of songs is shown
- Scenario :
  - When the actor clicks `Play` button of song, it is played and shown in the `Player bar`.
  - When the actor clicks title of song, **Song page** is loaded
- Exception :
- Acceptance test :
  - When clicking `Play` button, song should be played and shown well on `Player bar`.
  - When clicking title, **Song page** should be loaded correctly.

### **Create Song Page**

**Create Song Page (Sprint 3)**

- Actors : `User`
- Preconditions :
  - In **Main page**
  - `User` searched for a song that doesn't exist
  - Message informs that no one has previously created that song page
- Scenario :
  - `User` clicks `Create new song` button.
  - **Create song page** is loaded.
  - `User` enters title, singer, genre, Youtube reference link, and description about the song.
  - `User` clicks `Confirm` button.
  - **Song page** is loaded.
- Exception :
  - If either title, singer, genre, link or description field is empty, disable `Confirm` button
- Acceptance Test :
  - `Create new song` button should redirect to **Create song page**.
  - New `Song page` instance should be created
  - `Confirm` button should redirect to **Song page**.

### **Song Page**

**Listen to Combination (Sprint 3)**

- Actors : `User` or `Guest`
- Preconditions :
  - In **Main page**
  - Actor searched for a particular song
  - Some other user already created the **Song page**
- Scenario :
  - **Song page** for that song is loaded.
  - Clicking `Get Combination` button loads the combination into the `custom combination making area`.
  - Clicking `Listen` Button plays the combination in `Player bar`.
- Exception :
  - If `Listen` button is clicked without anything loaded into the `custom combination making area`, do nothing.
- Acceptance Test :
  - Correct **Song page** should be loaded.
  - When clicking `Listen`, the combination should play properly.

**Make Custom Combination (Sprint 3)**

- Actors : `User` or `Guest`
- Preconditions :
  - In **Song page**
- Scenario :
  - 1. Actor clicks `+` button, and list of instruments is shown.
  - 2. Actor selects an instrument(vocals, piano, guitar, etc.), and list of covers is shown.
  - 3. Actor clicks `Get` button of a cover, which loads the cover into `custom combination making area`.
  - Actor repeats 1-3 until a custom combination is made.
- Exception :
  - Same cover cannot be selected twice. (However, same instrument **can** be selected twice.)
- Acceptance Test :
  - Selected covers should be included in the custom combination
  - If the completed combination does not exist, it should be newly added to the combination list

**Functions in Cover List (Sprint 3,4)**

- Actors : `User` or `Guest`
- Preconditions :
  - In **Song page**
  - Actor clicked `+` button
  - Actor selected an instrument
- Scenario :
  - List of covers is shown.
  - Clicking `username` redirects to **User profile page**.
  - Clicking `title` redirects to **Cover detail page**.
  - Clicking each `tag` redirects to search result of that tag in **Main page**.
  - Clicking the area inside the cover item (not `username`, `title` or `tag`) plays the cover.
  - Actor Clicks `Get` button to choose cover.
  - Selected cover's `title` is shown next to the chosen instrument icon.
- Exception :
- Acceptance Test :
  - Every redirection should work well.
  - Selected Cover's `title` should be shown properly.
  - Listening to the selected cover should work properly.

### **Create Cover Page**

**Create new cover in Create Cover Page (Sprint 3, 4)**

- Actors : `User`
- Preconditions :
  - In **Song page**
  - `User` selected covers and clicked `Record` button
- Scenario :
  - **Create cover page (step 1)** is loaded.
  - 1. `User` records live while listening to the selected cover combination or youtube reference.
  - 2. `User` uploads a file containing their recording.
  - `User` clicks `Next` button.
  - **Create cover page (step 2)** is loaded.
  - `User` can preview(play in a mini player) their recording.
  - **Create cover page (step 3)** is loaded.
  - `User` inputs the title, instrument, genre, tags, and description.
  - `User` clicks `Save` button.
  - The cover is saved and **Cover page** is loaded.
- Exception :
  - if `User` does not fill the title or instrument, `Save` button should be disabled.
- Acceptance test :
  - Live listening & recording should be implemented properly.
  - New cover instance should be made properly.
  - Inputs of title, instrument, genre, and tags should be saved.

### **Cover Page**

**Listen to the Cover in Cover Page (Sprint 3)**

- Actors : `User` or `Guest`
- Preconditions :
  - In **Song page**
  - Actor clicks the title of a cover
- Scenario :
  - **Cover page** is loaded.
  - If `Play` button on the right side of the page is clicked, cover is played in `Player bar`.
- Exception :
- Acceptance test :
  - if `Play` button is clicked, the cover should be played.

**Show Cover Information (Sprint 3)**

- Actors : `User` or `Guest`
- Preconditions :
  - In **Song page**
  - Actor clicks the title of a cover
- Scenario :
  - **Cover page** is loaded.
  - The genre, description, tags can be seen.
  - Cover combination which was used for the recording of this cover can also be seen.
- Exception :
- Acceptance test :
  - The cover information should be displayed clearly.

**Edit Cover Information (Sprint 4)**

- Actors : User who is `Author` of this Cover page, `Admin`
- Preconditions :
  - In **Cover page**
  - Actor clicks the `Edit` button.
- Scenario :
  - **Edit cover page** is loaded, which looks like `Create cover page`.
  - After edits, the user can save information with the `Save` button.
- Exception :
  - If necessary inputs are blank, `Save` button should be disabled.
- Acceptance test :
  - After Clicking the `Save` button, the information should be changed correctly.

**Remove Cover Songs (Sprint 4)**

- Actors : User who is `Author` of this Cover page, `Admin`
- Precondition :
  - In **Cover page**
  - Actor clicks `Delete` button
- Scenario :
  - The cover song of this **Cover page** is deleted.
- Exception :
  - If the cover is already deleted, do nothing.
- Acceptance test :
  - The cover page should be removed along with its comment, tag, likes, etc.

### **Profile Page (Sprint 3,4)**

- Cover list
  - list all covers that the `User` made
- Follow
  - show following and follower counts of the `User`
  - show if the `Viewer` is following this profile
- Bio
  - Show bio that the `User` wrote

### **User Registration**

**Sign Up (Sprint 4)**

- Actors : `Guest`
- Preconditions :
  - In **Main page**
  - `Guest` has never signed up before
  - `Guest` clicks `Sign up` button
- Scenario :
  - **Sign up page** is loaded
  - `Guest` enters email and password
  - `Sign up` Button is activated
  - `Guest` clicks `Sign up` Button
  - `Guest`'s email and password is saved to DB
- Exception :
  - Does not enter email or password -> `Sign up` Button disabled
  - Email already exists -> should redirect to **Sign in page**
- Acceptance Test :
  - Email and password should be saved for the new user

**Sign In (Sprint 4)**

- Actors : `Guest`
- Preconditions :
  - In **Main page**
  - `Guest` has already signed up before
  - `Guest` clicks `Sign in` button
- Scenario :
  - **Sign in page** is loaded
  - `Guest` enters email and password
  - `Sign in` Button is activated
  - `Guest` clicks `Sign in` Button
  - `Guest` is signed in, changing the status to `User`
- Exception :
  - Does not enter email or password -> `Sign in` Button disabled
  - Email not found in DB -> redirect to **Sign up page**
  - Wrong password -> alert
- Acceptance Test :
  - `Guest` status should change to `User`(logged-in)

## 5. User Interface Requirements

> #### [Figma Link](https://www.figma.com/file/gNln0yZ7mLkvUX1IxviQQg/UI-Flow?node-id=0%3A1)
>
> To see the full UI in high definition, please visit the Figma link above.

**Main, SignIn, SignUp Page**
![UI 1](https://user-images.githubusercontent.com/77932817/139008457-9cfa0d03-ee35-4529-b564-22e08442d434.png)

<br/>

**Song, CoverPage**
![UI 2](https://user-images.githubusercontent.com/77932817/139008511-6b12f202-7ea0-411d-8646-ae831aa82387.png)

<br/>

**CreateCoverPage**
![UI 3](https://user-images.githubusercontent.com/77932817/139009023-221a2b4c-3fad-4b01-96a6-dfc5305e3a56.png)

<br/>

**SearchResult, CreateSong, Profile Page**
![UI 4](https://user-images.githubusercontent.com/77932817/139008566-ead7560f-9a90-4935-b27a-32936b623734.png)

<br/>

## Document Revision History

- `Rev. 1.0 2021-10-16` - initial version
- `Rev. 1.1 2021-10-30` - modified in Sprint 2
