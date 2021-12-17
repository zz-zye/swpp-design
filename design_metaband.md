### Table of Contents

- [1. System Architecture](#1-system-architecture)
  - [1.1. MVC Pattern](#11-mvc-pattern)
  - [1.2. Model](#12-model)
    - [E-R Diagram](#e-r-diagram)
  - [1.3. View](#13-view)
    - [UI Flow](#ui-flow)
    - [Globally used components/containers](#globally-used-componentscontainers)
    - [Pages](#pages)
  - [1.4. Controller](#14-controller)
    - [HTTP Data Format](#http-data-format)
- [2. Design Details](#2-design-details)
  - [2.1. Frontend Design](#21-frontend-design)
    - [Components](#components)
    - [Containers](#containers)
    - [Services](#services)
  - [2.2. Algorithms](#22-algorithms)
    - [Components](#components-1)
    - [Containers](#containers-1)
    - [Services](#services-1)
  - [2.3. Backend Design](#23-backend-design)
    - [API](#api)
- [3. Implementation Plan](#3-implementation-plan)
  - [3.1. Backend Implementation Plan](#31-backend-implementation-plan)
  - [3.2. Frontend Implementation Plan](#32-frontend-implementation-plan)
- [4. Testing Plan](#4-testing-plan)
  - [Unit Tests](#unit-tests)
  - [Functional Tests](#functional-tests)
  - [Acceptance & Integration Tests](#acceptance--integration-tests)
- [Document Revision History](#document-revision-history)

<br/>

---

## 1. System Architecture

### 1.1. MVC Pattern

Below is the Model-View-Controller pattern of MetaBand's system architecture.  
The backend consists of 8 models, and the frontend is largely divided into 7 pages.

<br/><br/>

![MVC](https://user-images.githubusercontent.com/77932817/138987116-e326713a-bf05-47ec-8426-9dcc848ad61e.png)

<br/>

---

### 1.2. Model

#### E-R Diagram

**Rectangles** depict Django model entities. Attributes of the model are shown as a list inside the table with field name and type.  
**PK** indicates primary key, listed at the top, and **FK** indicates foreign key.  
**Lines** depict relations. explanation about the relation is written in the middle of each line.  
The cardinality and ordinality of relations are denoted as numbers (0..\*) on each end of the line.  
Intermediary join tables in many-to-many relationships were left out for better clarity.

<br/>

![model](https://user-images.githubusercontent.com/77932817/139421345-3e221abe-cd83-4938-aa90-bfc69ed9fb50.png)

<br/>

Below is the description for each model.

| Model          | Description                                                                                                                                                             |
| :------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| User           | The user. Has bio(`Description`) and profile image(`Photo`). Can have followers and followings. Can submit instruments they can play. Can like covers and combinations. |
| Song           | Song with youtube reference video containing covers and combinations.                                                                                                   |
| Combination    | Combination for a particular song, made by combining covers. Has views and likes.                                                                                       |
| CombinationLog | Used to update viewcount of combinations.                                                                                                                               |
| Cover          | Cover for a particular song with a particular instrument, made by user. Has title, category, description, and tags added by user. Has views and likes.                  |
| CoverLog       | Used to update viewcount of covers.                                                                                                                                     |
| CoverTag       | Used to depict each tag of covers.                                                                                                                                      |
| Instrument     | Instrument used in covers by users.                                                                                                                                     |

<br/>

---

### 1.3. View

#### UI Flow

<br/>

> [**Figma Link**](https://www.figma.com/file/gNln0yZ7mLkvUX1IxviQQg/UI-Flow?node-id=0%3A1)  
> To see the full UI flow in high definition, please visit the Figma link above.

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

The details of each page and component are explained below.

<br/>

---

#### Globally used components/containers

**Header**

- Present on every page
- Show logo
- Show search bar
  - On searching a keyword, send to **SearchResultPage**
- If not logged in -> Show `signup` & `signin` button
  - Click `signup` -> send to **SignUpPage**
  - Click `signin` -> send to **SignInPage**
- Else -> show `signout` button
  - Click `signout` -> sign out

**PlayerBar**

- Present on every page except **SignUpPage** and **SignInPage**
- Show buttons for playing combinations: play, pause, previous / next
- Show current combination's song title, instrument icons

<br/>

---

#### Pages

**1. MainPage**

- Show recommended song(`Album`) list
  - Click play button of `Album` -> play top combination of the song
  - Click author of item -> send to author's **ProfilePage**
  - Click other parts of `Album` -> send to **SongPage**

**1-1. SignUpPage**

- Get email, password, passwordConfirm input
  - Click confirm button -> sign up user and send to **MainPage**

**1-2. SignInPage**

- Get email, password input
  - Click confirm button -> sign in user and send to **MainPage**

**2. SearchResultPage**

- Show list of songs that match query from most relevant
  - Click play button of item -> play top combination of the song
  - Click other parts of item -> send to **SongPage**
- Show `addSong` button
  - Click `addSong` -> send to **CreateSongPage**

**3. SongPage**

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
  - Click `Record` -> send to **CreateCoverPage** with selected combination

**3-1. CreateSongPage**

- Get title, singer, category, reference link, description input
- Click `confirm` button -> create and send to **SongPage**

**4. CoverPage**

- Cover info area
  - Show song image, title, singer and category
  - Show cover title and author
- Listen area
  - Show `Listen` button
    - Click `Listen` -> play the cover in small player
  - Show small player
  - Show description and tags

**4-1. CreateCoverPage - Step 1: Record**

- Reference area
  - If combination is selected -> show combination audio player
  - Else -> show reference Youtube video player
- Main area
  - Show file upload area
    - User can upload their audio file
  - Show `Record` button
    - Click -> live record the audio in sync with the reference
  - Click `Prev` button -> send to **SongPage**
  - Click `Next` button -> send to **Step 2**

**4-2. CreateCoverPage - Step 2: Preview**

- Show reference player and preview player
- Click `Prev` button -> send to **Step 1**
- Click `Next` button -> send to **Step 3**

**4-3. CreateCoverPage - Step 3: Info**

- Get title, category, instrument, tags and description input
- Click `Prev` button -> send to **Step 2**
- Click `Next` button -> create and send to **CoverPage**

**5. ProfilePage**

- Basic info area
  - Show profile photo, name, followers and following count
  - Owner can edit photo, name
- Cover area
  - Show cover list of user
- Bio area
  - Owner can edit bio

<br/>

---

### 1.4. Controller

<br/>

![controller](https://user-images.githubusercontent.com/77932817/139020294-b4f49473-0691-4250-8369-b0344e7d446e.png)

<br/>

The controller will receive HTTP requests from the view(frontend) by the user, and respond with HTTP responses. The former is depicted by arrows from left to right, and the latter is depicted by arrows from right to left. On each arrow, the API and explanation is written.

#### HTTP data format

**1-1. `user/signup/`**

> Request (`POST`)
>
> ```json
> {
>   "email": "example@snu.ac.kr",
>   "password": "qwer1234"
> }
> ```
>
> Response (Status: `201`)
>
> ```json
> {}
> ```

**1-2. `user/signin/`**

> Request (`POST`)
>
> ```json
> {
>   "email": "example@snu.ac.kr",
>   "password": "qwer1234"
> }
> ```
>
> Response (Status: `204`)
>
> ```json
> {}
> ```

**1-3. `user/signout/`**

> Request (`GET`)
>
> ```json
> {}
> ```
>
> Response (Status: `204`)
>
> ```json
> {}
> ```

**1-4. `user/info/<id:int>/`**

> Request (`GET`)
>
> ```json
> {}
> ```
>
> Response (Status: `200`)
>
> ```json
> {
>   "id": "1",
>   "username": "swpp2020",
>   "email": "swpp2020@snu.ac.kr",
>   "password": "qwer1234",
>   "description": "Description",
>   "photo": "<user photo>",
>   "followings": {},
>   "instruments": {}
> }
> ```
>
> Request (`POST`)
>
> ```json
> {
>   "description": "Edited Description"
> }
> ```
>
> Response (Status: `200`)
>
> ```json
> {
>   "id": "1",
>   "username": "swpp2020",
>   "email": "swpp2020@snu.ac.kr",
>   "password": "qwer1234",
>   "description": "Edited Description",
>   "photo": "<user photo>",
>   "followings": {},
>   "instruments": {}
> }
> ```
>
> Request (`DELETE`)
>
> ```json
> {}
> ```
>
> Response (Status: `204`)
>
> ```json
> {}
> ```

**2. `instrument/`**

> Request (`GET`)
>
> ```json
> {}
> ```
>
> Response (Status: `200`)
>
> ```json
> [
>   {
>     "id": 0,
>     "name": "Vocals"
>   },
>   {
>     "id": 1,
>     "name": "Guitar"
>   }
> ]
> ```
>
> Request (`POST`)
>
> ```json
> {
>   "name": "Piano"
> }
> ```
>
> Response (Status: `201`)
>
> ```json
> {
>   "id": 2,
>   "name": "Piano"
> }
> ```

**3-1. `cover/<songid:int>/`**

> Request (`GET`)
>
> ```json
> {}
> ```
>
> Response (Status: `200`)
>
> ```json
> [
>   {
>     "id": 11425609,
>     "audio": "<audio link 1>",
>     "title": "Funky bass",
>     "description": "Some jazzy bass",
>     "user": {...},
>     "song": {...},
>     "instrument": { "id": 3, "name": "Guitar" },
>     "tags": ["funky"],
>     "views": 75114619,
>     "combination_id": 91162842
>   },
>   {
>     "id": 8145183,
>     "audio": "<audio link 2>",
>     "title": "METAL!!",
>     "description": "thick guitar cover",
>     "user": {...},
>     "song": {...},
>     "instrument": { "id": 1, "name": "Vocal" },
>     "tags": ["cool", "massive"],
>     "views": 12936493,
>     "combination_id": 31474537
>   }
> ]
> ```
>
> Request (`POST`)
>
> ```json
> {
>   "audio": "<audio link>",
>   "title": "guitar cover!",
>   "description": "cool guitars",
>   "instrument": 1,
>   "tags": ["cool", "cute"],
>   "combination_id": 11298229
> }
> ```
>
> Response (Status: `201`)
>
> ```json
> {
>   "id": 11435674,
>   "audio": "<audio link>",
>   "title": "guitar cover!",
>   "description": "cool guitars",
>   "user": {...},
>   "song": {...},
>   "instrument": { "id": 1, "name": "Vocal" },
>   "tags": ["cool", "cute"],
>   "combination_id": 11298229
> }
> ```

**3-2. `cover/<songid:int>/<instrumentid:int>/`**

> Request (`GET`)
>
> ```json
> {}
> ```
>
> Response (Status: `200`)
>
> ```json
> [
>   {
>     "id": 8145183,
>     "audio": "<audio link 2>",
>     "title": "METAL!!",
>     "description": "thick guitar cover",
>     "user": {...},
>     "song": {...},
>     "instrument": { "id": 1, "name": "Vocal" },
>     "tags": ["cool", "massive"],
>     "views": 12936493,
>     "combination_id": 31474537
>   }
> ]
> ```

**3-3. /cover/info/<id:int>/**

> Request (`GET`)
>
> ```json
> {}
> ```
>
> Response (Status: `200`)
>
> ```json
> {
>   "id": 11425609,
>   "audio": "<audio link 1>",
>   "title": "Funky bass",
>   "description": "Some jazzy bass",
>   "user": {...},
>   "song": {...},
>   "instrument": { "id": 3, "name": "Guitar" },
>   "tags": ["funky"],
>   "views": 75114619,
>   "combination_id": 91162842
> }
> ```
>
> Request (`PUT`)
>
> ```json
> {
>   "title": "Cool Bass",
>   "description": "Very very cool",
>   "tags": ["cool", "lit"]
> }
> ```
>
> Response (Status: `200`)
>
> ```json
> {
>   "id": 11425609,
>   "audio": "<audio link 1>",
>   "title": "Cool Bass",
>   "description": "Very very cool",
>   "user": {...},
>   "song": {...},
>   "instrument": { "id": 3, "name": "Guitar" },
>   "tags": ["cool", "lit"],
>   "views": 75114619,
>   "combination_id": 91162842
> }
> ```
>
> Request (`DELETE`)
>
> ```json
> {}
> ```
>
> Response (Status: `204`)
>
> ```json
> {}
> ```

**3-4. `cover/like/<id:int>/`**

> Request (`GET`)
>
> ```json
> {}
> ```
>
> Response (Status: `200`)
>
> ```json
> {
>   "isLiked": false
> }
> ```
>
> Request (`PUT`)
>
> ```json
> {
>   "isLiked": true
> }
> ```
>
> Response (Status: `200`)
>
> ```json
> {
>   "isLiked": true
> }
> ```

**4-1. `combination/<songid:int>/`**

> Request (`GET`)
>
> ```json
> {}
> ```
>
> Response (Status: `200`)
>
> ```json
> [
>   {
>     "id": 12364821,
>     "covers": [...],
>     "likes": 123
>   },
>   {
>     "id": 23465719,
>     "covers": [...],
>     "likes": 39
>   }
> ]
> ```
>
> Request (`POST`)
>
> ```json
> {
>   "covers": [11298123, 12483920]
> }
> ```
>
> Response (Status: `201`)
>
> ```json
> {
>   "id": 23645123,
>   "covers": [{...}, {...}]
> }
> ```

**4-2. `combination/info/<id:int>/`**

> Request (`GET`)
>
> ```json
> {}
> ```
>
> Response (Status: `200`)
>
> ```json
> {
>   "id": 12364821,
>   "covers": [...],
>   "likes": 123
> }
> ```

**4-3. `combination/like/<id:int>/`**

> Request (`GET`)
>
> ```json
> {}
> ```
>
> Response (Status: `200`)
>
> ```json
> {
>   "isLiked": false
> }
> ```
>
> Request (`PUT`)
>
> ```json
> {
>   "isLiked": true
> }
> ```
>
> Response (Status: `200`)
>
> ```json
> {
>   "isLiked": true
> }
> ```

**4-4. `combination/main/`**

> Request (`GET`)
>
> ```json
> {}
> ```
>
> Response (Status: `200`)
>
> ```json
> [
>   {
>     "id": 12364821,
>     "song": {...},
>     "covers": [...],
>     "likes": 123
>   },
>   {
>     "id": 23465719,
>     "song": {...},
>     "covers": [...],
>     "likes": 39
>   }
> ]
> ```

**5-1. `song/`**

> Request (`GET`)
>
> ```json
> {}
> ```
>
> Response (Status: `200`)
>
> ```json
> [
>   {
>     "id": 12375392,
>     "title": "Bohemian Rhapsody",
>     "singer": "Queen",
>     "category": "Rock",
>     "reference": "http://example1.com",
>     "description": "Queen's magnum opus"
>   },
>   {
>     "id": 12375393,
>     "title": "Don't Stop Me Now",
>     "singer": "Queen",
>     "category": "Rock",
>     "reference": "http://example2.com",
>     "description": "Queen's magnum opus"
>   }
> ]
> ```
>
> Request (`POST`)
>
> ```json
> {
>   "title": "Bohemian Rhapsody",
>   "singer": "Queen",
>   "category": "Rock",
>   "reference": "http://example.com",
>   "description": "Queen's magnum opus"
> }
> ```
>
> Response (Status: `201`)
>
> ```json
> {
>   "id": 12375392,
>   "title": "Bohemian Rhapsody",
>   "singer": "Queen",
>   "category": "Rock",
>   "reference": "http://example.com",
>   "description": "Queen's magnum opus"
> }
> ```

~~5-2. `song/main/` _(deprecated)_~~

> Request (`GET`)
>
> ```json
> {}
> ```
>
> Response (Status: `200`)
>
> ```json
> [
>   {
>     "id": 12375392,
>     "title": "Bohemian Rhapsody",
>     "singer": "Queen",
>     "category": "Rock",
>     "reference": "http://example1.com",
>     "description": "Queen's magnum opus"
>   },
>   {
>     "id": 12375393,
>     "title": "Don't Stop Me Now",
>     "singer": "Queen",
>     "category": "Rock",
>     "reference": "http://example2.com",
>     "description": "Queen's magnum opus"
>   }
> ]
> ```

**5-3. `song/search/?q=<key:str>/`**

> Request (`GET`)
>
> ```json
> {}
> ```
>
> Response (Status: `200`)
>
> ```json
> [
>   {
>     "id": 12375392,
>     "title": "Bohemian Rhapsody",
>     "singer": "Queen",
>     "category": "Rock",
>     "reference": "http://example1.com",
>     "description": "Queen's magnum opus"
>   }
> ]
> ```

**5-4. `song/info/<id:int>/`**

> Request (`GET`)
>
> ```json
> {}
> ```
>
> Response (Status: `200`)
>
> ```json
> {
>   "id": 12375392,
>   "title": "Bohemian Rhapsody",
>   "singer": "Queen",
>   "category": "Rock",
>   "reference": "http://example1.com",
>   "description": "Queen's magnum opus"
> }
> ```
>
> Request (`PUT`)
>
> ```json
> {
>   "description": "Edited description"
> }
> ```
>
> Response (Status: `200`)
>
> ```json
> {
>   "id": 12375392,
>   "title": "Bohemian Rhapsody",
>   "singer": "Queen",
>   "category": "Rock",
>   "reference": "http://example1.com",
>   "description": "Edited description"
> }
> ```

<br/>

---

## 2. Design Details

### 2.1. Frontend Design

#### Components

![front_components](https://user-images.githubusercontent.com/5417636/141515758-25f446ed-afc6-4ef3-8d82-de5f86aabdcd.png)

<br/>

#### Containers

![front_container](https://user-images.githubusercontent.com/5417636/141515768-7937e8b9-e7ea-49e5-ab20-f6db5e8451d8.png)

<br/>

#### Services

![front_services](https://user-images.githubusercontent.com/77932817/139018444-237c9f89-7982-4974-81f1-f72b4ec823b1.png)

<br/>

---

### 2.2. Algorithms

#### Components

**1. PlayerBar**

- `onTitleClicked`: Redirect to SongPage of clicked song
- `onPlayClicked`: Resume or pause playing combination.
- `onNextClicked`: Play next combination in playlist.
- `onPrevClicked`: Play previous combination in playlist.
- `onLikeClicked`: Call backend API (`POST: "/combination/like/<id:int>/"`)
- `onUnlikeClicked`: Call backend API (`DELETE: "/combination/like/<id:int>/"`)

**2. SimplePlayer**

- `onPlayButtonClicked`: Resume playing combination.
- `onPauseButtonClicked`: Resume playing combination.
- `onLikeClicked`: Call backend API (`POST: "/combination/like/<id:int>/"`)
- `onUnlikeClicked`: Call backend API (`DELETE: "/combination/like/<id:int>/"`)

<br/>

---

#### Containers

**0. Header**

- `init`: Check user is logged in by calling backend API. Then, show login and logout button if user in not signed in, else show logout button.
- `onSearchClicked`: Redirect to SearchResult page with search keyword as parameter.

**1. MainPage**

- `onAlbumTitleClicked`: Redirect to SongPage of clicked song
- `onAlbumPlayClicked`: Add to PlayBar's playlist.

**1-1. SignUpPage**

- `onSignUpClicked`: Call backend API (`POST: "/user/signup/"`) with input email and password. If signin sucess, redirect to previous page.

**1-2. SignInPage**

- `onSignInClicked`: Call backend API (`POST: "/user/signin/"`) with input email and password. If login sucess, redirect to previous page.

**2. SearchResultPage**

- `onResultLineClicked`: Redirect to SongPage of clicked song
- `onResultPlayClicked`: Add to PlayBar's playlist.
- `onAddSongClicked`: Redirect to CreateSongPage.

**3. SongPage**

- `onRefLinkClicked`: Redirect to ref link url. Maybe youtube link.
- `onCombinationGetClicked`: Get combination covers and put them into coverSelector.
- `onCombinationListenClicked`: Add clicked combination to PlayBar's playlist.
- `onRecordClicked`: Redirect to CreateCoverRecord with selected covers
- `onListenCliced`: Add selected covers as combination to PlayBar's playlist.
- `onCoverSelected`: Add selected cover list.
- `onCoverAddClicked`: Show list of instruments.
- `onCoverGetClicked`: Add selected cover list.
- `onCoverTitleClicked`: Redirect to Coverpage of clicked cover.

**3-1. CreateSongPage**

- `onConfirmClicked`: Check forms and call backend API (`POST: "/song/"`) with inputs.

**4. CoverPage**

- `onListenClicked`: Show new miniPlayer and start to play.
- `onEditClicked`: Only for author. Show EditCover.
- `onDeleteClicked`: Call backend API (`DELETE: "/cover/info/<id:int>/"`), and redirect to MainPage.

**4-1. CreateCoverRecordPage**

- `onFileDropped`: save file as recorded cover.
- `onRecordClicked`: Start to play ref or selected combination, and start record.
- `onCancelClicked`: Redirect to Main.
- `onNextButtonClicked`: Check inputs and move to next step.

**4-2. CreateCoverPreviewPage**

- `onFileDropped`: save file as recorded cover.
- `onRecordClicked`: Start to play ref or selected combination, and start record.
- `onNextClicked`: Check inputs and move to next step.
- `onPrevClicked`: Move to prev step.

**4-3. CreateCoverInfoPage**

- `onNextClicked`: Check inputs, call backend API (`POST: "/cover/<songid:int>/"`), and redirect to CoverPage.
- `onPrevClicked`: Move to prev step.

**5. ProfilePage**

- `onCoveTitleClicked`: Redirect to Coverpage of clicked cover.
- `onNameEditClicked`: Chage name editable, and show nameSubmit button.
- `onNameSubmitClicked`: Submit to backend, and unshow nameSubmit button.
- `onBioEditClicked`: Chage bio editable, and show bioSubmit button.
- `onBioSubmitClicked`: Submit to backend, and unshow bioSubmit button.

<br/>

---

#### Services

**1. UserService**

- `sign_up(email:string, password:string)` : Call Backend signup API, and return the result.
- `sign_in(email:string, password:string)` : Call Backend signin API, and return the result.
- `sign_out()` : Call Backend signout API.
- `get_specific_user(id:int)` : Call Backend API and return the information of requested user.
- `edit_specific_user(id:int)` : Call Backend API and edit specific user information.
- `delete_specific_user(id:int)` : Call Backend API and delete specific user.

**2. SongService**

- `create_new_song()` : Call Backend API and create new SongPage.
- `get_song_list()` : Call Backend API and get all song list.
- `get_song_recommendation()` : Call Backend API and get recommended song list .
- `get_search_song(name : string)` : Call Backend API and get song list related to the searched key.
- `get_specific_song(id:int)` : Call Backend API and get specific song information.
- `edit_specific_song(id:int)` : Call Backend API and edit specific song information.

**3. CoverService**

- `create_new_cover(songid:int)` : Call Backend API and create new CoverPage.
- `get_cover_list(songid:int)` : Call Backend API and get all cover list.
- `get_cover_list_instrument(songid:int, instrumentid:int)` : Call Backend API and return cover list including specific instrument.
- `get_cover_list_info(id:int)` : Call Backend API and get specific cover information.
- `edit_specific_cover_info(id:int)` : Call Backend API and edit specific cover information.
- `delete_specific_cover(id:int)` : Call Backend API and delete specific cover.

**4. Combination Service**

- `get_combination_list(songid:int)` : Call Backend API and return combination list of requested song.
- `create_new_combination(songid:int)` : Call Backend API and create new combination.
- `get_combination_info(id:int)` : Call Backend API and get specific combination information.
- `get_combination_like(id:int)` : Call Backend API and return whether user likes this combination.
- `set_combination_like(id:int)` : Call Backend API and set like flag to 1.
- `unset_combination_like(id:int)` : Call Backend API and set like flag to 0.

**5.Instrument Service**

- `get_instrument_list()` : Call Backend API and get instrument list.
- `create_instrument()` : Call Backend API and add new instrument.

<br/>

---

### 2.3. Backend Design

#### API

| Model           | API                                       | GET                            | POST                          | PUT                            | DELETE                |
| --------------- | ----------------------------------------- | ------------------------------ | ----------------------------- | ------------------------------ | --------------------- |
| **User**        | `/user/signup/`                           | X                              | Sign up(create new user)      | X                              | X                     |
|                 | `/user/signin/`                           | X                              | Sign in                       | X                              | X                     |
|                 | `/user/signout/`                          | Sign out                       | X                             | X                              | X                     |
|                 | `/user/info/<id:int>/`                    | Get specific user              | X                             | Edit specific user info        | Delete specific user  |
| **Instrument**  | `/instrument/`                            | Get instrument list            | (admin) Create new instrument | X                              | X                     |
| **Cover**       | `/cover/<songid:int>/`                    | Get cover list                 | Create new cover              | X                              | X                     |
|                 | `/cover/<songid:int>/<instrumentid:int>/` | Get cover list with instrument | X                             | X                              | X                     |
|                 | `/cover/info/<id:int>/`                   | Get specific cover info        | X                             | Edit specific cover info       | Delete specific cover |
|                 | `/cover/like/<id:int>/`                   | Get if user likes cover        | X                             | User likes/unlikes cover       | X                     |
| **Combination** | `/combination/<songid:int>/`              | Get combination list           | Create new combination        | X                              | X                     |
|                 | `/combination/info/<id:int>/`             | Get specific combination info  | X                             | X                              | X                     |
|                 | `/combination/like/<id:int>/`             | Get if user likes combination  | X                             | User likes/unlikes combination | X                     |
| **Song**        | `/song/`                                  | Get song list                  | Create new song               | X                              | X                     |
|                 | `/song/main/`                             | Get song recomemdation         | X                             | X                              | X                     |
|                 | `/song/search/?q=<key:str>/`              | Get song search list           | X                             | X                              | X                     |
|                 | `/song/info/<id:int>/`                    | Get specific song              | X                             | Edit specific song info        | X                     |

<br/>

---

## 3. Implementation Plan

### 3.1. Backend Implementation Plan

The **Difficulty** is categorized into 1 ~ 5 stars.  
The **Time** needed for implementation is shown in days.  
There are no hard dependencies among backend APIs, but every API should be made after the initializing step `B0.1`.

|   #    | Model            | API                                       | Difficulty | Time | Sprint |  Assignee  |
| :----: | :--------------- | :---------------------------------------- | :--------: | :--: | :----: | :--------: |
| `B0.1` | **Initializing** | make model                                |   ★★☆☆☆    |  2   |   4    |    종선    |
| `B1.1` | **User**         | `/user/signup/`                           |   ★★☆☆☆    |  2   |   4    |    정윤    |
| `B1.2` |                  | `/user/signin/`                           |   ★☆☆☆☆    |  1   |   4    |    정윤    |
| `B1.3` |                  | `/user/signout/`                          |   ★☆☆☆☆    |  1   |   4    |    정윤    |
| `B1.4` |                  | `/user/info/<id:int>/`                    |   ★☆☆☆☆    |  1   |   4    |    정윤    |
| `B2.1` | **Instrument**   | `/instrument/`                            |   ★☆☆☆☆    |  1   |   4    |    태원    |
| `B3.1` | **Cover**        | `/cover/<songid:int>/`                    |   ★★☆☆☆    |  1   |   4    |    태원    |
| `B3.2` |                  | `/cover/<songid:int>/<instrumentid:int>/` |   ★★☆☆☆    |  2   |   4    |    태원    |
| `B3.3` |                  | `/cover/info/<id:int>/`                   |   ★★★☆☆    |  2   |   4    |    종선    |
| `B3.4` |                  | `/cover/like/<id:int>/`                   |   ★☆☆☆☆    |  1   |   4    |    태원    |
| `B4.1` | **Combination**  | `/combination/<songid:int>/`              |   ★★☆☆☆    |  2   |   4    |    태원    |
| `B4.2` |                  | `/combination/info/<id:int>/`             |   ★☆☆☆☆    |  1   |   4    |    태원    |
| `B4.3` |                  | `/combination/like/<id:int>/`             |   ★☆☆☆☆    |  1   |   4    |    태원    |
| `B5.1` | **Song**         | `/song/`                                  |   ★★☆☆☆    |  2   |   4    |    형욱    |
| `B5.2` |                  | `/song/main/` **=>** `/combination/main/` |   ★★★★☆    |  4   |   4    | 형욱, 종선 |
| `B5.3` |                  | `/song/search/?q=<key:str>/`              |   ★★☆☆☆    |  2   |   4    |    형욱    |
| `B5.4` |                  | `/song/info/<id:int>/`                    |   ★★☆☆☆    |  2   |   4    |    형욱    |

<br/>

---

### 3.2. Frontend Implementation Plan

The **Difficulty** and **Time** columns are the same as backend.  
The **Dependency** column shows which features it depends on. Only hard dependencies that are completely necessary are shown. Otherwise, it is possible to implement the feature with dummy models.

|    #     | Page/Component                 | Feature                                     | Dependency               | Difficulty | Time | Sprint |  Assignee  |
| :------: | :----------------------------- | :------------------------------------------ | :----------------------- | :--------: | :--: | :----: | :--------: |
|  `F1.1`  | **Header**                     | Design, sign in / sign up buttons           |                          |   ★☆☆☆☆    |  1   |   3    |    종선    |
|  `F1.3`  |                                | Sign out function                           | `B1.3`                   |   ★☆☆☆☆    |  2   |   4    |    종선    |
|  `F1.3`  |                                | Search bar, simple search algorithm         |                          |   ★★★☆☆    |  2   |   3    |    종선    |
|  `F2.1`  | **Player Bar**                 | Dummy player bar                            |                          |   ★☆☆☆☆    |  1   |   3    |    종선    |
|  `F2.2`  |                                | Song playing, Play/Pause funciton           | `F2.1`                   |   ★★★☆☆    |  2   |   3    |    종선    |
|  `F2.3`  |                                | Previous/Next function                      | `F2.2, B4.1`             |   ★★★☆☆    |  2   |   3    |    종선    |
|  `F3.1`  | **Album** (item of album list) | Dummy design                                |                          |   ★☆☆☆☆    |  1   |   3    |    정윤    |
|  `F3.2`  |                                | Play function                               | `F2.1, F3.1`             |   ★☆☆☆☆    |  1   |   3    |    정윤    |
|  `F3.3`  |                                | Spinning CD design                          | `F3.1`                   |   ★★☆☆☆    |  1   |   3    |    정윤    |
|  `F4.1`  | **Mini Player**                | Dummy design                                |                          |   ★☆☆☆☆    |  1   |   3    |    종선    |
|  `F4.2`  |                                | Song playing, Play/Pause funciton           | `F4.1`                   |   ★★★☆☆    |  2   |   3    |    종선    |
|  `F5.1`  | **Main Page**                  | Album list without recommendation algorithm | `F3.1, B5.1`             |   ★★☆☆☆    |  2   |   3    |    정윤    |
|  `F6.1`  | **Sign Up, Sign In Page**      | Input, dummy button                         |                          |   ★☆☆☆☆    |  1   |   4    |    정윤    |
|  `F6.2`  |                                | Connecting backend, authorization           | `F6.1, B1.1, B1.2`       |   ★★★☆☆    |  2   |   4    |    정윤    |
|  `F7.1`  | **Search Result Page**         | Album list without query matching           | `F3.1, B5.1`             |   ★★☆☆☆    |  1   |   3    |    정윤    |
|  `F7.2`  |                                | Add song button                             |                          |   ★☆☆☆☆    |  1   |   3    |    정윤    |
|  `F8.1`  | **Song Page**                  | Song info area                              | `B5.4`                   |   ★★☆☆☆    |  1   |   3    |    태원    |
|  `F8.2`  |                                | Top combination list                        | `F2.1, B5.1, B4.1, B4.2` |   ★★★☆☆    |  2   |  3,4   |    태원    |
|  `F8.3`  |                                | Top cover list                              | `F4.1, B4.1, B3.1, B3.3` |   ★★★☆☆    |  2   |  3,4   |    태원    |
|  `F8.4`  |                                | Combination making function                 | `F8.2, F8.3`             |   ★★★☆☆    |  2   |  3,4   |    태원    |
|  `F8.5`  |                                | Play / record buttons                       | `F2.1, F8.4`             |   ★☆☆☆☆    |  1   |   4    |    태원    |
|  `F9.1`  | **Create Song Page**           | Input, confirm button                       | `B5.1`                   |   ★★☆☆☆    |  1   |   3    |    태원    |
| `F10.1`  | **Cover Page**                 | Cover info area                             | `B3.1, B3.3`             |   ★★☆☆☆    |  1   |   3    |    종선    |
| `F10.2`  |                                | Listen area                                 | `F4.1`                   |   ★★☆☆☆    |  1   |   3    |    종선    |
| `F10.3`  |                                | Delete cover                                | `F10.1`                  |   ★☆☆☆☆    |  1   |   3    |    종선    |
| `F10.4`  |                                | Edit cover info                             | `F10.1`                  |   ★★★☆☆    |  2   |   4    |    종선    |
| `F11.1`  | **Create Cover Page**          | Reference youtube video player              |                          |   ★★★☆☆    |  1   |   3    |    형욱    |
| `F11.2`  |                                | Combination audio player                    | `F4.1, B4.1, B3.3`       |   ★☆☆☆☆    |  1   |   3    |    형욱    |
| `F11.3`  |                                | Reference area                              | `F11.1, F11.2`           |   ★★☆☆☆    |  1   |   3    |    형욱    |
| `F11.4`  |                                | Record function                             |                          |   ★★★★☆    |  3   |   3    |    형욱    |
| `F11.5`  |                                | Record syncing                              | `F11.4`                  |   ★★★★★    |  4   |   4    | 형욱, 태원 |
| `F11.6`  |                                | File upload function                        |                          |   ★★★☆☆    |  2   |   4    |    형욱    |
| `F11.7`  |                                | File upload syncing                         | `F11.6`                  |   ★★★★★    |  3   |  4, 5  | 형욱, 태원 |
| `F11.8`  |                                | Step 2: Preview page                        | `F11.2`                  |   ★★☆☆☆    |  1   |   3    |    형욱    |
| `F11.9`  |                                | Step 3: Info page                           | `B3.3`                   |   ★☆☆☆☆    |  1   |   3    |    형욱    |
| `F11.10` |                                | Prev / next buttons                         |                          |   ★☆☆☆☆    |  1   |   3    |    형욱    |
| `F12.1`  | **User Profile Page**          | Basic info area, bio area                   | `B1.4`                   |   ★★☆☆☆    |  1   |   4    |    정윤    |
| `F12.2`  |                                | Cover area                                  | `F4.1, B3.3`             |   ★★☆☆☆    |  1   |   4    |    정윤    |
| `F12.3`  |                                | name, bio editing                           | `F12.1`                  |   ★★☆☆☆    |  1   |   4    |    정윤    |
| `F12.4`  |                                | Profile photo editing                       | `F12.1`                  |   ★★★☆☆    |  1   |   4    |    정윤    |

<br/>

### 3.3. Associated Risks / Plan B

The risk of this plan is to implement audio editing feature and machine learning feature.

The implementation of the back-end is set as sprint4. However, if not all of them are implemented so far, important functions will be implemented first. In particular, features related to the Song, Cover, and Combination models will be implemented preferentially in spring 4, and the insufficient parts will be implemented following in spring 5.

The implementation of the front-end first implements features related to recording and playback in print3. In sprint 3, UI implementation is the top priority. And implementing complex logic such as integrating with the server and modifying audio files will be implemented in sprint4.

If the audio editing function is not implemented smoothly on the frontend, we will consider proceeding with the project without the editing function, or creating a new audio file by editing audio on the server.

And the machine learning function, song recommendation feature, is planned in spring 4, but it is also a difficult part, and if the schedule is postponed due to implementations of other functions, implementation may be made in spring 5. If the model cannot be properly tuned due to the schedule, the plan will be revised to try the existing api or model.

<br/>

---

## 4. Testing Plan

### Unit Tests

The subject of unit testing would be every component and module. In each sprint, we would test implemented modules with React: Jest, Enzyme / Django : Python unit test. We expect code coverage to be over 90%.

### Functional Tests

The subject of functional testing would be every API. We will use React: Jest, Enzyme / Django : Python unit test. In sprint 3,4 we would test RESTful API.

### Acceptance & Integration Tests

We would use Cucumber for acceptance testing and Travis CI for integration testing. We will run acceptance test and integration test in sprint 4.

## Document Revision History

- `Rev. 1.0 2021-10-28` - initial version
- `Rev. 1.1 2021-11-13` - modified in Sprint 3
- `Rev. 1.2 2021-11-27` - modified in Sprint 4
- `Rev. 1.21 2021-11-10` - modified in issue #76
