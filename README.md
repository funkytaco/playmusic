Node-JS Google Play Music API
====

Written by Jamon Terrell <git@jamonterrell.com>

This project is not endorsed by of affiliated with Google in any way.

The library requires Google credentials, but does not require an All Access subscription on that account. It also requires a mobile device registered against that account. Ensure you have signed into the app on a phone and that you have played any amount of music. This will authorise the device that the library will masquerade as.

How to Use
----

Initialization
```
var PlayMusic = require('playmusic');
var pm = new PlayMusic();
pm.init({email: "email@address.com", password: "password"}, function() {
  // place code here
})
```

Retrieve list of all tracks in your library (uploaded tracks _and_ tracks added to library from All Access)
```
    pm.getAllTracks(function(library) {
        var song = library.data.items.pop();
        console.log(song);
        pm.getStreamUrl(song.id, function(streamUrl) {
            console.log(streamUrl);
        });
    });
```

Search for a song
```
    pm.search("bastille lost fire", 5, function(data) { // max 5 results
        var song = data.entries.sort(function(a, b) { // sort by match score
            return a.score < b.score;
        }).shift(); // take first song
        console.log(song);
        pm.getStreamUrl(song.track.nid, function(streamUrl) {
            console.log(streamUrl);
        });
    }, function(message, body, err, httpResponse) {
        console.log(message);
    });
```

Retrieve Playlists
```
    // gets all playlists
    pm.getPlayLists(function(data) {
        console.log(data.data.items);
    });

    // gets all playlists, and all entries on each
    pm.getPlayListEntries(function(data) {
        console.log(data.data.items);
    });
```

Retrieve the Stream URL for a song by track.storeId (All Access songs only!!!)
```
    pm.getStreamUrl("Thvfmp2be3c7kbp6ny4arxckz54", console.log);
```

Retrieve the Stream URL for a song by track.id (uploaded songs only!!!)
```
    pm.getStreamUrl("84df1e4e-6b76-3147-9a78-a44becc28dc5", console.log);
```

Retrieve information about an album or artist (All Access Only!!!)
```
    // getArtist - artistId, albumList, topTrackCount, relatedArtistCount[, success, error]
    pm.getArtist('Ak6zkmv2zbbsaxl63cgsnx5ttcm', true, 2, 2);

    // getAlbum - albumId, includeTracks[, success, error]
    pm.getAlbum('Bfn67zo6q3ekh35eaorkq5untmi', true);
```

Get Google Play Music Settings

```
    pm.getSettings();
```

Console Mode!
===

```
npm install playmusic
node

var pm = new (require('playmusic'));
pm.init({email: "email", password: "password"}, function() {});

// wait for a second or two to make sure you're logged in.

pm.getPlayLists();
pm.getPlayListEntries();

// getArtist - artistId, albumList, topTrackCount, relatedArtistCount[, success, error]
pm.getArtist('Ak6zkmv2zbbsaxl63cgsnx5ttcm', true, 2, 2);

// getStreamUrl for All Access - sj#track -> storeId
pm.getStreamUrl('Tsbbwp6r2wpwxb55noc6b26kwq4');

// getStreamUrl for uploaded - sj#track -> id
pm.getStreamUrl('84df1e4e-6b76-3147-9a78-a44becc28dc5');


// Find all uploaded tracks (tracks returned by getAllTracks without a storeId), get a streamUrl for one of them
var allTracks;
pm.getAllTracks(function(data) { allTracks = data.data.items; });
var searchResults = allTracks.filter(function(track) { return typeof track.storeId === "undefined"; });
console.log(searchResults[0]);
pm.getStreamUrl(searchResults[0].id);
```

Future
----
* create stations / get songs from stations/genres
* provide better examples
* add automated integration testing so semaphore build results are useful
* improve authentication
* externalize cookie handling
* explore providing a higher level api that allows easy following of relationships between objects
* Suggestions?  submit an issue!

License
----
This Source Code is subject to the terms of the Mozilla Public
License, v. 2.0. If a copy of the MPL was not distributed with this
file, You can obtain one at http://mozilla.org/MPL/2.0/.

Attribution
----

Special thanks go out to the following individuals and projects:
* [Google Play Music resolver for Tomahawk](https://github.com/tomahawk-player/tomahawk-resolvers/blob/master/gmusic/content/contents/code/gmusic.js) - information on the gmusic api
* [gmusicapi project](https://github.com/simon-weber/Unofficial-Google-Music-API/blob/develop/gmusicapi/protocol/mobileclient.py)  by Simon Weber and others - information on the gmusic api, also special thanks to their IRC support of my many questions
* [wyozi](https://github.com/wyozi) - playlist management support
* [Jonathan Cremin](https://github.com/kudos) and [match-audio](https://match.audio/) - contributions including support for new api changes and documentation for authorization, bug fixes for error handling
* [Jan Smolders](https://github.com/jansmolders86) and [MediacenterJS](http://mediacenterjs.com/) - testing, troubleshooting and other feedback
* [Puneeth Uttla](https://github.com/kiwiluver75) - authentication workaround
