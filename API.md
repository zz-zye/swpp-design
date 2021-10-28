|Model|API|GET|POST|PUT|DELETE|
|---|---|---|---|---|---|
|**User**|`/user/signup/`|X|Sign up(create new user)|X|X|
||`/user/signin/`|X|Sign in|X|X|
||`/user/signout/`|Sign out|X|X|X|
||`/user/info/<id:int>/`|Get specific user|X|Edit specific user info|Delete specific user|
|**Instrument**|`/instrument/`|Get instrument list|(admin) Create new instrument|X|X|
|**Cover**|`/cover/<song_id:int>/`|Get cover list|Create new cover|X|X|
||`/cover/<song_id:int>/<instrument_id:int>/`|Get cover list with instrument|X|X|X|
||`/cover/info/<id:int>/`|Get specific cover info|X|Edit specific cover info|Delete specific cover|
|**Combination**|`/combination/<song_id:int>/`|Get combination list|Create new combination|X|X|
||`/combination/info/<id:int>/`|Get specific combination info|X|X|X|
||`/combination/like/<id:int>/`|Get If user likes combination|User likes combination|X|User unlikes combination|
|**Song**|`/song/`|Get song list|Create new song|X|X|
||`/song/main/`|Get song recomemdation|X|X|X|
||`/song/search/?q=<key:str>/`|Get song search list|X|X|X|
||`/song/info/<id:int>/`|Get specific song|X|Edit specific song info|X|