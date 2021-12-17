### Table of Contents

- [1. Sprint 4 Plan](#1-sprint-4-plan)
- [2. Sprint 4 Achievements](#2-sprint-4-achievements)
- [3. Sprint 4 Challenge](#3-sprint-4-challenge)
- [4. Next Sprint Plan](#4-next-sprint-plan)

<br/>

## 1. Sprint 4 Plan

## Frontend

### Header

- [ ] Sign out function

### Signin/Signup Page

- [x] SignIn Page (\*)
- [x] SignUp Page (\*)

### Create Cover Page

- [x] Combination audio player
- [ ] Record Audio Syncing
- [x] File Upload Function
- [ ] File Upload Syncing

### Cover Page

- [x] Cover info area
- [x] Listen area
- [x] Delete cover
- [x] Edit Cover Info

### User Profile Page

- [x] Basic info area, bio area (\*)
- [x] Cover area (\*)
- [x] Name, bio editing (\*)
- [x] Profile photo editing (\*)

### Mini Player

- [x] Dummy design
- [x] Song playing, Play/Pause function

## Backend

### Initializing

- [x] Make model

### User

- [x] `/user/signup/`(\*)
- [x] `/user/signin/`(\*)
- [x] `/user/signout/`(\*)
- [ ] `/user/info/<id:int>/`

### Instrument

- [x] `/instrument/`

### Cover

- [x] `/cover/<songid:int>/` (\*)
- [x] `/cover/<songid:int>/<instrumentid:int>/` (\*)
- [x] `/cover/info/<id:int>/` (\*)
- [x] `/cover/like/<id:int>/` (\*)

### Combination

- [x] `/combination/<songid:int>/` (\*)
- [x] `/combination/info/<id:int>/` (\*)

### Song

- [ ] `/song/`
- [ ] `/song/main/`
- [ ] `/song/search/?q=<key:str>/`
- [ ] `/song/info/<id:int>/`

(Those marked with \* were not merged to main branch. See Sprint 4 Achievements for details.)

## 2. Sprint 4 Achievements

### 2.1 Frontend

- Cover Page
- Create Cover Page
  were implemented in sprint 4.

- User Profile Page
- Signin/Signup page
  are done in a separate branch, but since backend user views are not implemented yet(see below), it is not merged into the main branch.

- Testing

**Overall coverage metric**

![image](https://user-images.githubusercontent.com/77932817/143673592-777039a6-9a9b-49ef-bfbd-951a1b98dfac.png)

**Lowest coverage**

![image](https://user-images.githubusercontent.com/77932817/143673599-b6e9eac0-602b-48b4-bb19-207851c7f7ce.png)

The waveform function in CoverPage is currently being revised and new library is being searched, so the test code has not been implemented yet.

### 2.2 Backend

- Initialization
- Instrument views
  were implemented in sprint 4.

- User views
  were not implemented due to some challenges in implementation. These will be implemented in Sprint 5.

- Cover views
- Combination views
  are done in a separate branch, but since user views are not implemented yet, it is not merged into the main branch.

- Testing

**Overall coverage metric**

![image](https://user-images.githubusercontent.com/77932817/143673791-7f2b18cc-0c13-4bbc-9d31-c8e81ee3ec84.png)

**Lowest coverage**

![image](https://user-images.githubusercontent.com/77932817/143673817-43721996-38d0-46a7-9258-9c318eac3e6a.png)

These files are basic files that were added while setting the project. Either the test codes for these files will be implemented in sprint 5, or they will be ignored from tests.

### 2.3 Connecting frontend and backend

- Frontend API settings
  Using Redux Saga and Axios, the basic settings to connect frontend and backend were implemented in sprint 4.

- Cover Page
  was connected to the backend using Axios.

### 2.3 GitHub

- README instructions were added.

#### Requirements and Specification

- Project Abstract
  was modified. The title of the project was changed from **Bandcruit** to **MetaBand**, and the meaning of the new title was added into the abstract.

- User Stories
  was modified according to the next sprint plan. The stories to be implemented in Sprint 5 are marked.

#### Design and Planning

- HTTP Data Format
  was added in system architecture. This was originally added in Sprint 3; However, it was not mentioned in the Sprint 3 Backlog. It was acknowledged in the Sprint 4 meeting.

- Implementation plan
  was changed according to the next sprint plan.

## 3. Sprint 4 Challenge

### TrackPlayer

We need a device that can play various covers at the same time, but HTMLMediaElement can only play one audio at a time.  
One possible solution is to combine covers in backend server and then provide to user, but it is not optimal since 1) saving all possible combinations to the server is space-consuming and 1) user may experience long delay due to combining covers in real-time.  
So, we designed a new TrackPlayer that can play many covers at the same time with multiple HTMLAudioElements.  
Test results show that even when 100 covers were played at once, the time difference between each covers were less than 1ms, providing a good quality playback environment.
We designed a state machine with states of `init`, `loading`, `wait`, `play`, `pause`, and `ended` so that basic player functions can work without problems.

### Handling Audio data

It was very difficult to handle audio recorded/uploaded by the user. A library to record and a library to play were separately found and applied using the web audio API.  
Since it has to be synchronized with reference audio after recording, audio editing functions were additionally implemented.  
After recording/uploading, the audio file is changed to array buffer, then changed to audio buffer, and then converted into an mp3 file using the `lamejs` library.  
Since these libraries are not built for React Framework, it is also difficult to test them using jest.

### Django REST Framework

For easier implementation of the backend, Django REST Framework was used. However, since it has many features and most of our team is new to the library, it was hard to decide which methods / code styles to use. After many revisions, the file structure was decided as follows.

```
models.py
serializers.py <-- contains various Django REST Framework Serializers of each model
views/
  song_views.py
  cover_views.py
  ...
tests/
  tools.py <-- setup functions for tests
  test_models.py
  ...
  views/
    test_cover.py
    ...
```

Views are written in Class form, using Django REST Framework's `generics.GenericAPIView` with various mixins.
These decisions had to be made as backend was initialized, to minimize the refactoring work after each teammate writes their backend views. Before the views are implemented, all views were made as a dummy, only returning status 501. Dummy serializers were also made.

## 4. Next Sprint Plan

### 4.1 Backend Implmentation Plan

|   #    | Model    | API                                           | Difficulty | Time | Sprint |  Assignee  |
| :----: | :------- | :-------------------------------------------- | :--------: | :--: | :----: | :--------: |
| `B1.4` | **User** | `/user/info/<id:int>/`                        |   ★☆☆☆☆    |  1   |   5    |    정윤    |
| `B5.1` | **Song** | `/song/`                                      |   ★★☆☆☆    |  2   |   5    |    형욱    |
| `B5.2` |          | `/song/main/` **\*Needs song recommendation** |   ★★★★☆    |  4   |   5    | 형욱, 종선 |
| `B5.3` |          | `/song/search/?q=<key:str>/`                  |   ★★☆☆☆    |  2   |   5    |    형욱    |
| `B5.4` |          | `/song/info/<id:int>/`                        |   ★★☆☆☆    |  2   |   5    |    형욱    |

### 4.2 Frontend Implementation Plan

|    #    | Page/Component        | Feature               | Dependency   | Difficulty | Time | Sprint |  Assignee  |
| :-----: | :-------------------- | :-------------------- | :----------- | :--------: | :--: | :----: | :--------: |
| `F1.3`  |                       | Sign out function     | `B1.3`       |   ★☆☆☆☆    |  2   |   5    |    종선    |
| `F11.5` | **Create Cover Page** | Record syncing        | `F11.4`      |   ★★★★★    |  4   |   5    | 형욱, 태원 |
| `F11.7` |                       | File upload syncing   | `F11.6`      |   ★★★★★    |  3   |   5    | 형욱, 태원 |
| `F12.2` | **User Profile Page** | Cover area            | `F4.1, B3.3` |   ★★☆☆☆    |  1   |   5    |    정윤    |
| `F12.3` |                       | name, bio editing     | `F12.1`      |   ★★☆☆☆    |  1   |   5    |    정윤    |
| `F12.4` |                       | Profile photo editing | `F12.1`      |   ★★★☆☆    |  1   |   5    |    정윤    |

### 4.3 Other

The following should be implemented by all team members together. These will be done by having team scrum meetings and online/offline coding sessions.

- Change DB
- Deploy to EC2 server
- ML Recommendation
- Connect Backend and Frontend
- Alpha test
- Fill dummy data for Demo
