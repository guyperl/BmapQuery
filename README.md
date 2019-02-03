BmapQuery.js  v0.3
==========

BmapQuery is a Microsoft BingMaps V8 functions. to be used inside web pages.
component to be used as part of web applications and websites.


## Installation

Installing BmapQuery is an easy task. Just follow these simple steps:

 1. **Download** the latest version from the Github website:
    https://github.com/yamazakidaisuke/BmapQuery.
    ( Example Site: https://mapapi.org/indexb.php )
    You should have already completed this step, but be sure you have the very latest version.

2. BmapQuery is by default installed in the `js` folder. You can
place the files in whichever you want though. Operation check with index.html.


## Getting Started

1. <script src="...?Callback=GetMap&..." is specified in the URL."GetMap" runs first.

2. Please get BingMapsKey from "BingMaps Dev Center" site.
   [Get BingMapsKey >> BingMaps Dev Center](https://www.bingmapsportal.com/)

3. Replace [ *** YOUR MY KEY *** ] in the example code with BingMapsKey.

**[html: index.html]**

    <!-- 1.Load BingMapsControl api [callback=GetMap] -->
    <script src='https://www.bing.com/api/maps/mapcontrol?callback=GetMap&key=[***Your My Key***]' async defer></script>

    <!-- 2.Load BmapQuery -->
    <script src="js/BmapQuery.js"></script>

    <!-- 3.BmapQuery Start -->
    <script>
    //init
    function GetMap(){
        //Start
        const map = new Bmap("#myMap); //Preparation
        //Map
        map.startMap(47.6149, -122.1941, "load", 10); //Run on one line
        //Pin
        let pin = map.pin(47.6149, -122.1941, "#ff0000"); //Run on one line
        //infobox
        map.infobox(47.6149, -122.1941, "Title", "Description"); //Run on one line
    }


## Examples
**Sample files for each function are prepared in the "example" folder.**


## Directory Structure

It's opinionated about how you organize your repositories.


    ├── index.html               (Link to example)
    ├── js/
    │   └── BmapQuery.js         (BingMaps Library)
    ├── img/
    │   └── poi_custom.png
    ├─── example/                (All example files)
    │   ├── map_start.html
    │   ├── map_event.html
    │   ├── map_changeView.html
    │   ├── pushpin.html
    │   ├── ...
    │   ...
    │
    ├── README.md
    └── LICENSE


## Checking Your Installation

To test your installation, just call the following page at your website:

[[BmapQuery.js] (https://mapapi.org/indexb.php)]

![default](https://user-images.githubusercontent.com/1481062/51815689-1d4ef300-2306-11e9-9c03-a6e0654025a6.JPG)



## Documentation

**[script]**

    //*Sample
    function GetMap() {
        //----------------------------------------------------
        // Instance...
        //----------------------------------------------------
        let map = new Bmap("#myMap");

        //----------------------------------------------------
        // Display Map
        // startMap(lat, lon, "MapType", Zoom[1~20]);
        //----------------------------------------------------
        map.startMap(47.6149, -122.1941, "load", 16);//MapType[load, aerial,canvasDark,canvasLight,birdseye,grayscale,streetside]

        //----------------------------------------------------
        // Map:Events
        // onMap("event", callback);
        // [event:click,dblclick,rightclick,mousedown,mouseout,mouseover,mouseup,mousewheel,maptypechanged,viewchangestart,viewchange,viewchangeend]
        //----------------------------------------------------
        map.onMap("click",function(){
            alert("MapEvent!");
        });

        //----------------------------------------------------
        // Pushpin
        // pin(lat, lon, "color", [drag:true|false], [click:true|false], [hover:true|false], [visible:true|false]);
        //----------------------------------------------------
        let pin1 = map.pin(47.6149, -122.1941, "#ff0000");

        //----------------------------------------------------
        // Pushpin:Text
        // pinText(lat, lon, "title", "subtitle", "text");
        //----------------------------------------------------
        let pin2 = map.pinText(47.6160, -122.1950, "title","subtitle","A");

        //----------------------------------------------------
        // Pushpin:Icon
        // pinIcon(lat, lon, icon, scale, anchor_x, anchor_y);
        //----------------------------------------------------
        let pin3 = map.pinIcon(47.6130, -122.1945, "img/poi_custom.png", 1.0, 0, 0);

        //----------------------------------------------------
        // pushpin:Events
        // onPin(pushpin, "event", callback);
        // [event: click,mousedown,mouseout,mouseover,mouseup]
        //----------------------------------------------------
        map.onPin(pin1, "click", function(){
            alert("PinEvent1");
        });

        //----------------------------------------------------
        // Infobox
        // infobox(lat, lon, "title", "description");
        //----------------------------------------------------
        map.infobox(47.6149, -122.1941, "1 step", "Start");

        //----------------------------------------------------
        // Infobox:html
        // infoboxHtml(lat, lon, html);
        //----------------------------------------------------
        map.infoboxHtml(47.6160, -122.1950, '<div style="background:red;">Hello,world</div>');

        //----------------------------------------------------
        // MapChangeView(after 2 seconds.)
        // changeMap(lat, lon, "MapType", Zoom[1~20]);
        //----------------------------------------------------
        setTimeout(function(){
            map.changeMap(47.6150, -122.1950, "load", 17);
        },2000);

        //----------------------------------------------------
        // Geocode(2 patterns & after 4 seconds.)
        // getGeocode("searchQuery",callback);
        //----------------------------------------------------
        setTimeout(function () {
            //A. Address
            map.getGeocode("Seattle", function(data){
                alert("A. getGeocode");
                document.querySelector("#geocode").innerHTML=data;
            });
            //B. Click Event
            map.onGeocode("click", function(data){
                alert(data.location);
            });
        },4000);

        //------------------------------------------------------------------------
        //Get Reverse Geocode
        //2 patterns
        //after 6,8 seconds.
        //------------------------------------------------------------------------
        //A.Set location data for BingMaps
        setTimeout(function () {
            const location = map.setLocation(47.6130, -122.1945);
            map.reverseGeocode(location, function(data){
                alert("A. Reverse Geocode");
                document.querySelector("#geocode").innerHTML=data;
            });
        },6000);

        //B.Get MapCenter location
        setTimeout(function () {
            const mapCenter = map.getCenter();
            map.reverseGeocode(mapCenter, function(data){
                alert("B. Reverse Geocode");
                document.querySelector("#geocode").innerHTML=data;
            });
        },8000);

        //----------------------------------------------------
        //Directions:Search.
        // !! For confirmation, set the parameters for each country !!
        // +  [ English => https://www.bing.com/...&setLang=en&setMkt=en-US ]
        // +  [ Japan   => https://www.bing.com/...&setLang=ja&setMkt=ja-JP ]
        //------------------------------------------------------------------------
        document.getElementById("search").onclick = function () {
            const from  = document.getElementById("from").value;  //StartPoint
            const to    = document.getElementById("to").value;    //EndPoint
            const mode  = document.getElementById("mode").value;  //RouteMode[walking,driving]
            const array = ["Bellevue", "Yarrow Point"];           //Waypoints...
            map.direction("#direction", mode, from , to, array);  //Direction Methed
        };

        //-----------------------------------------------------
        // AutoSuggest
        // !! Only viewing user's region can be displayed !!
        //-----------------------------------------------------
        // HTML:Add !!
        // <h1>AutoSuggest(Enter city in text box)</h1>
        // <div id='searchBoxContainer'>
        //     <input type='text' id='searchBox'><button id="clear">Clear</button>
        // </div>
        //-----------------------------------------------------
        map.selectedSuggestion("#searchBox","#searchBoxContainer");

        //----------------------------------------------------
        // Traffic
        // map.traffic();
        //----------------------------------------------------
        map.traffic();

        //----------------------------------------------------
        // get Boundary
        // map.getBoundary("type");
        //----------------------------------------------------
        // [ "type" ]
        // *CountryRegion:Country or region.
        // *AdminDivision1:First administrative level within the country/region level, such as a state or a province.
        // *AdminDivision2:Second administrative level within the country/region level, such as a county.
        // *PopulatedPlace:A concentrated area of human settlement, such as a city, town or village.
        // *Neighborhood:A section of a populated place that is typically well-known, but often with indistinct boundaries.
        // *Postcode1:The smallest post code category, such as a zip code.
        // *Postcode2:The next largest post code category after Postcode1 that is created by aggregating Postcode1 areas.
        // *Postcode3:The next largest post code category after Postcode2 that is created by aggregating Postcode2 areas.
        // *Postcode4:The next largest post code category after Postcode3 that is created by aggregating Postcode3 areas.
        // Note: Not all entity types are available in all areas.
        //---------------------------------------------------
        map.getBoundary("PopulatedPlace");

        //----------------------------------------------------
        // Get multiple boundaries
        //  map.getMultiBoundary(["Postcode"...]);
        //----------------------------------------------------
        const zipCodes = ['98004', '98005', '98007', '98008', '98039'];
        map.getMultiBoundary(zipCodes);

    }

## Author

Copyright (c) 2018-2019, BingMapsGO! - DaisukeYamazaki. All rights reserved.
https://mapapi.org - See LICENSE.md for license information.


