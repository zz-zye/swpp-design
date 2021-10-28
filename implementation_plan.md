
---

### Backend Implementation Plan

The **Difficulty** is categorized into 1 ~ 5 stars.   
The **Time** needed for implementation is shown in days.   
There are no hard dependencies among backend APIs, but every API should be made after the initializing step `B0.1`.

|#|Model|API|Difficulty|Time(days)|Sprint|Assignee|
|:-:|:-|:-|:-:|:-:|:-:|:-:|
|`B0.1`|**Initializing**|make model|★★☆☆☆|2|2|종선|
|`B1.1`|**User**|`/user/signup/`|★★☆☆☆|2|3|형욱|
|`B1.2`||`/user/signin/`|★☆☆☆☆|1|3|형욱|
|`B1.3`||`/user/signout/`|★☆☆☆☆|1|3|형욱|
|`B1.4`||`/user/info/<id:int>/`|★☆☆☆☆|1|3|형욱|
|`B2.1`|**Instrument**|`/instrument/`|★☆☆☆☆|1|3|태원|
|`B3.1`|**Cover**|`/cover/<songid:int>/`|★★☆☆☆|1|3|태원|
|`B3.2`||`/cover/<songid:int>/<instrumentid:int>/`|★★☆☆☆|2|3|태원|
|`B3.3`||`/cover/info/<id:int>/`|★★★☆☆|2|3|태원|
|`B3.4`||`/cover/like/<id:int>/`|★☆☆☆☆|1|4|태원|
|`B4.1`|**Combination**|`/combination/<songid:int>/`|★★☆☆☆|2|3|정윤|
|`B4.2`||`/combination/info/<id:int>/`|★☆☆☆☆|1|3|정윤|
|`B4.3`||`/combination/like/<id:int>/`|★☆☆☆☆|1|4|정윤|
|`B5.1`|**Song**|`/song/`|★★☆☆☆|2|3|정윤|
|`B5.2`||`/song/main/` ***Needs song recommendation**|★★★★☆|4|3, 4|정윤|
|`B5.3`||`/song/search/?q=<key:str>/`|★★☆☆☆|2|3|정윤|
|`B5.4`||`/song/info/<id:int>/`|★★☆☆☆|2|3|정윤|

---

### Frontend Implementation Plan

The **Difficulty** and **Time** columns are the same as backend.   
The **Dependency** column shows which features it depends on. Only hard dependencies that are completely necessary are shown. Otherwise, it is possible to implement the feature with dummy models.

|#|Page/Component|Feature|Dependency|Difficulty|Time(days)|Sprint|Assignee|
|:-:|:-|:-|:-|:-:|:-:|:-:|:-:|
|`F1.1`|**Header**|Design, sign in / sign up buttons||★☆☆☆☆|1|3|종선|
|`F1.3`||Sign out function|`B1.3`|★☆☆☆☆|2|3|종선|
|`F1.3`||Search bar, simple search algorithm||★★★☆☆|2|3|종선|
|`F2.1`|**Player Bar**|Dummy player bar||★☆☆☆☆|1|3|종선|
|`F2.2`||Song playing, Play/Pause funciton|`F2.1`|★★★☆☆|2|3|종선|
|`F2.3`||Previous/Next function|`F2.2, B4.1`|★★★☆☆|2|3|종선|
|`F3.1`|**Album** (item of album list)|Dummy design||★☆☆☆☆|1|3|태원|
|`F3.2`||Play function|`F2.1, F3.1`|★☆☆☆☆|1|3|태원|
|`F3.3`||Spinning CD design|`F3.1`|★★☆☆☆|1|3|태원|
|`F4.1`|**Mini Player**|Dummy design||★☆☆☆☆|1|3|종선|
|`F4.2`||Song playing, Play/Pause funciton|`F4.1`|★★★☆☆|2|3|종선|
|`F5.1`|**Main Page**|Album list without recommendation algorithm|`F3.1, B5.1`|★★☆☆☆|2|3|태원|
|`F6.1`|**Sign Up, Sign In Page**|Input, dummy button||★☆☆☆☆|1|3|정윤|
|`F6.2`||Connecting backend, authorization|`F6.1, B1.1, B1.2`|★★★☆☆|2|3|정윤|
|`F7.1`|**Search Result Page**|Album list without query matching|`F3.1, B5.1`|★★☆☆☆|1|3|정윤|
|`F7.2`||Add song button||★☆☆☆☆|1|3|정윤|
|`F8.1`|**Song Page**|Song info area|`B5.4`|★★☆☆☆|1|3|태원|
|`F8.2`||Top combination list|`F2.1, B5.1, B4.1, B4.2`|★★★☆☆|2|3,4|태원|
|`F8.3`||Top cover list|`F4.1, B4.1, B3.1, B3.3`|★★★☆☆|2|3,4|태원|
|`F8.4`||Combination making function|`F8.2, F8.3`|★★★☆☆|2|3,4|태원|
|`F8.5`||Play / record buttons|`F2.1, F8.4`|★☆☆☆☆|1|4|태원|
|`F9.1`|**Create Song Page**|Input, confirm button|`B5.1`|★★☆☆☆|1|3|태원|
|`F10.1`|**Cover Page**|Cover info area|`B3.1, B3.3`|★★☆☆☆|1|3|종선|
|`F10.2`||Listen area|`F4.1`|★★☆☆☆|1|3|종선|
|`F10.3`||Delete cover|`F10.1`|★☆☆☆☆|1|3|종선|
|`F10.4`||Edit cover info|`F10.1`|★★★☆☆|2|4|종선|
|`F11.1`|**Create Cover Page**|Reference youtube video player||★★★☆☆|1|3|형욱|
|`F11.2`||Combination audio player|`F4.1, B4.1, B3.3`|★☆☆☆☆|1|3|형욱|
|`F11.3`||Reference area|`F11.1, F11.2`|★★☆☆☆|1|3|형욱|
|`F11.4`||Record function||★★★★☆|3|3|형욱|
|`F11.5`||Record syncing|`F11.4`|★★★★★|4|4|형욱|
|`F11.6`||File upload function||★★★☆☆|2|4|형욱|
|`F11.7`||File upload syncing|`F11.6`|★★★★★|3|4|형욱|
|`F11.8`||Step 2: Preview page|`F11.2`|★★☆☆☆|1|3|형욱|
|`F11.9`||Step 3: Info page|`B3.3`|★☆☆☆☆|1|3|형욱|
|`F11.10`||Prev / next buttons||★☆☆☆☆|1|3|형욱|
|`F12.1`|**User Profile Page**|Basic info area, bio area|`B1.4`|★★☆☆☆|1|3,4|정윤|
|`F12.2`||Cover area|`F4.1, B3.3`|★★☆☆☆|1|3,4|정윤|
|`F12.3`||name, bio editing|`F12.1`|★★☆☆☆|1|4|정윤|
|`F12.4`||Profile photo editing|`F12.1`|★★★☆☆|1|4|정윤|
