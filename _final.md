### Table of Contents

- [1. System Architecture](#1-system-architecture)
- [2. Design Details](#2-design-details)
- [3. Implementation Plan](#3-implementation-plan)
- [4. Testing Plan](#4-testing-plan)
- [Document Revision History](#document-revision-history)

## 1. System Architecture
### **MVC**
<system_architecture.drawio>

### **Model**
#### **E-R Diagram**
<model.drawio>

#### **Model Methods**

(method 설명)

### **View**
#### **UI Flow**

[figma](https://www.figma.com/file/wxrYdzTTUHOKm9W5qSGWmC/design?node-id=63%3A3)

#### **Page Functions**

> **Globally Used Components**

**Header**
- Present on every page
- Show logo
- Show search bar
  - On searching a keyword, send to **Search Result Page**
- If not logged in -> Show `signup` & `signin` button
  - Click `signup` -> send to **Sign Up Page**
  - Click `signin` -> send to **Sign In Page**
- Else -> show `signout` button
  - Click `signout` -> sign out

**Player Bar**
- Present on every page except **Sign Up Page** and **Sign In Page**
- Show buttons for playing combinations: play, pause, previous / next
- Show current combination's song title, instrument icons

> **Containers**

**1. Main Page**
- Show recommended song(`Album`) list
  - Click play button of `Album` -> play top combination of the song
  - Click other parts of `Album` -> send to **Song Page**
- **Player Bar**

**1-1. Sign Up Page**
- Get email, password, passwordConfirm input
  - Click confirm button -> sign up user and send to **Main Page**

**1-2. Sign In Page**
- Get email, password input
  - Click confirm button -> sign in user and send to **Main Page**

**2. Search Result Page**
- Show list of songs that match query from most relevant
  - Click play button of item -> play top combination of the song
  - Click author of item -> send to author's **User Profile Page**
  - Click other parts of item -> send to **Song Page**
- Show `addSong` button
  - Click `addSong` -> send to **Create Song Page**
- **Player Bar**


**3. Song Page**
- Song info area
  - Show song image, title, singer, category, and `refLink` button
  - Click `refLink` -> send to reference video link
- Top combinations area
  - Show list of top combinations in a table
  - Show each combination's rank, covers, plays, likes, and `Get` button
    - Click `Get` button of item -> add the combination to current combination
    - Click other parts of item -> play the combination
- Current combination area
  - Show `Add`, `Record`, `Play` button
  - Click `Add` -> show `Instrument list` dropdown
  - select instrument from `Instrument list` -> show `Cover list` of that instrument
  - `Cover list` area
    - Show list of top covers of particular instrument
    - Show each cover's rank, author, title, tags, plays, likes, and `Get` button
    - Click `Get` of item -> add the cover to current combination
    - Click other parts of list item -> play the cover in small player
  - Click `Play` -> play the combination
  - Click `Record` -> send to **Create Cover Page** with selected combination

**3-1. Create Song Page**
- Get title, singer, category, reference link, description input
- Click `confirm` button -> create and send to **Song Page**

**4. Cover Page**
- Cover info area
  - Show song image, title, singer and category
  - Show cover title and author
- Listen area
  - Show `Listen` button
    - Click `Listen` -> play the cover in small player
  - Show small player
  - Show description and tags

**4-1. Create Cover Page - Step 1: Record**
- Reference area
  - If combination is selected -> show combination audio player
  - Else -> show reference Youtube video player
- Main area
  - Show file upload area
    - User can upload their audio file
  - Show `Record` button
    - Click -> live record the audio in sync with the reference
  - Click `Prev` button -> send to **Song Page**
  - Click `Next` button -> send to **Step 2**
  

**4-2. Create Cover Page - Step 2: Preview**
- Show reference player and preview player
- Click `Prev` button -> send to **Step 1**
- Click `Next` button -> send to **Step 3**

**4-3. Create Cover Page - Step 3: Info**
- Get title, category, instrument, tags and description input
- Click `Prev` button -> send to **Step 2**
- Click `Next` button -> create and send to **Cover Page**

**5. User Profile Page**
- Basic info area
  - Show profile photo, name, followers and following count
  - If the owner is viewing -> show `Edit` button
- Cover area
  - Show cover list of user
- Bio area


## 2. Design Details
### **Frontend Design**
#### **Components**
<front_components.drawio>

#### **Algorithms**
1. Header
- init: Check user is logged in by calling backend api. Then, show login and logout button if user in not signed in, else show logout button.
- onSearchClicked: Redirect to SearchResult page with search keyword as parameter.

2. Main
- onAlbumTitleClicked: Redirect to SongPage of clicked song

3. PlayBar
- onTitleClicked: Redirect to SongPage of clicked song
- onPlayClicked: Resume of pause playing combination.
- onNextClicked: Play next combination in playlist.
- onPrevClicked: Play previous combination in playlist.
- onLikeClicked: Call backend api (POST: "/combination/like/<id:int>/")
- onUnlikeClicked: Call backend api (DELETE: "/combination/like/<id:int>/")

4. SignIn
- onSignInClicked: Call backend api (POST: "/user/signin/") with input email and password. If login sucess, redirect to previous page.

4. SignUp
- onSignUpClicked: Call backend api (POST: "/user/signup/") with input email and password. If signin sucess, redirect to previous page.

5. SearchResult
- onResultLineClicked: Redirect to SongPage of clicked song
- onResultAuthorClicked: Redirect to ProfilePage of clicked user


### **Backend Design**
#### **API**

## 3. Implementation Plan


## 4. Testing Plan
### **Unit Tests**

### **Functional Tests**

### **Integration Tests**

## Document Revision History
- `Rev. 1.0 2021-XX-XX` - initial version
