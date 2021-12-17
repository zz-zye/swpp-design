### Table of Contents

- [1. Sprint 5 Plan](#1-sprint-5-plan)
- [2. Sprint 5 Achievements](#2-sprint-5-achievements)
- [3. Sprint 5 Challenge](#3-sprint-5-challenge)
- [4. Future Plans](#4-future-plans)

<br/>

## 1. Sprint 5 Plan

## Frontend & Connecting

### Header

- [x] Sign out function

### Main Page

- [x] Connecting to backend
- [x] Design details

### Search Result Page

- [x] Connecting to backend
- [x] Design details

### Song Page

- [x] Connecting to backend

### Create Cover Page

- [x] Record Audio Syncing
- [x] File Upload Syncing

### Profile Page

- [x] Connecting to backend
- [x] Profile image crop
- [x] Design details

## Backend

### User

- [x] `/user/info/<id:int>/`

### Combination

- [x] `/combination/main/` (moved from /song/main/)

### Song

- [x] `/song/`
- [x] `/song/main/` (moved to /combination/main/)
- [x] `/song/search/?q=<key:str>/`
- [x] `/song/info/<id:int>/`

## Other

- [x] Change DB
- [x] Deploy to EC2 server
- [ ] ML Recommendation
- [x] Connect Backend and Frontend
- [x] Alpha test
- [x] Fill dummy data for Demo

## 2. Sprint 5 Achievements

### 2.1 Frontend

Header, MainPage, SearchResultPage, SongPage, CreateCoverPage & ProfilePage were connected to backend.

Design details were implemented, such as the CD images in MainPage rotating when the corresponding band is playing.

A new & better library was implemented in CreateCoverPage that provides cut & merging service.

Profile image editing functions were implemented in ProfilePage.

**Overall coverage metric**

TODO

**Lowest coverage**

TODO

### 2.2 Backend

User views & Song views were implemented in sprint 5.

The `/song/main/` url which was originally used to recommended songs in MainPage was changed to `/combination/main/` which gives recommends as list of combinations. This is due to the fact that MainPage needs to be able to easily play the best bands(combinations) to the user. This change in API was applied in the Design & Planning page.

- Testing

**Overall coverage metric**

TODO

**Lowest coverage**

TODO

#### Design and Planning

- HTTP Data Format
  was changed to apply the changes made in API, including the `/song/main/` url's change into `/combination/main/`.

## 3. Sprint 5 Challenge

## 4. Future Plans

### 4.1 Frontend

Like and View-counting functions could not be implemented until Sprint 5. These will be implemented until the poster session and added to the report.

In the long term, the look & feel of each 'MetaBand' may be changed. Having each player's 'character' profiles appear together as if they're an actual band may suit the name more.

### 4.2 Backend

(TODO) ... could not be implemented until Sprint 5. This will be implemented until the poster session and added to the report.

In the long term, Search-related libraries or Machine Learning could be implemented into the search function.
