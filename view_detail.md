## View detail

### **Globally Used Components**

**Header**
- Present on every page
- Show logo
- Show search bar
  - On searching a keyword, send to **Search Result Page**
- If not logged in -> Show `Sign up` & `Sign in` button
  - Click `Sign up` -> send to **Sign Up Page**
  - Click `Sign in` -> send to **Sign In Page**

**Player Bar**
- Present on every page except **Sign Up Page** and **Sign In Page**
- Show buttons for playing combinations: play, pause, previous / next
- Show current combination's song title, instrument icons

### **Containers**

**1. Main Page**
- Show recommended song list
  - Click play button of item -> play top combination of the song
  - Click other parts of item -> send to **Song Page**
- **Player Bar**

**1-1. Sign Up Page**
- Get email, password input
  - Click confirm button -> sign up user and send to **Main Page**

**1-2. Sign In Page**
- Get email, password input
  - Click confirm button -> sign in user and send to **Main Page**

**2. Search Result Page**
- Show list of songs that match query from most relevant
  - Click play button of item -> play top combination of the song
  - Click other parts of item -> send to **Song Page**
- Show `Add Song` button
  - Click `Add Song` -> send to **Create Song Page**
- **Player Bar**


**3. Song Page**
- Song info area
  - Show song image, title, singer, genre, and `reference video` button
  - Click `reference video` -> send to reference video link
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
- Get title, singer, genre, reference link, description input
- Click confirm button -> create and send to **Song Page**

**4. Cover Page**
- Cover info area
  - Show song image, title, singer and genre
  - Show cover title and author
- Listen area
  - Show `Listen` button
    - Click `Listen` -> play the cover in small player
  - Show small player
  - Show description and tags

**4-1. Create Cover Page - Step 1**
- Reference area
  - If combination is selected -> show reference combination player
  - Else -> show reference Youtube video player
- Main area
  - Show file upload area
    - User can upload their audio file
  - Show `Record` button
    - Click -> live record the audio in sync with the reference
  - Click confirm button -> send to **Step 2**

**4-2. Create Cover Page - Step 2**
- Get title, genre, instrument, tags and description input
- Click confirm button -> create and send to **Cover Page**

**5. User Profile Page**
- Basic info area
  - Show profile photo, name, followers and following count
  - If the owner is viewing -> show `Edit` button
- Cover area
  - Show cover list of user
- Bio area
