<!DOCTYPE html>
<html>

  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>Replace with your title</title>
    <meta name="description" content="">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <script src="https://api.mapbox.com/mapbox-gl-js/v1.7.0/mapbox-gl.js"></script>
    <link href="https://api.mapbox.com/mapbox-gl-js/v1.7.0/mapbox-gl.css" rel="stylesheet" />
    <script src='https://cdnjs.cloudflare.com/ajax/libs/tabletop.js/1.5.1/tabletop.min.js'></script>
    <link rel="stylesheet" href="style.css">

  </head>
  <style>
    body {
      background: #404040;
      margin: 0;
      padding: 0;
    }


    #map {
      border-left: 1px solid #fff;
      position: absolute;
      width: 100%;
      top: 0;
      bottom: 0;
    }


    .mapboxgl-popup {
      padding-bottom: 5px;
    }

    .mapboxgl-popup-close-button {
      display: none;
    }

    .mapboxgl-popup-content {
      font: 400 15px/22px 'Source Sans Pro', 'Helvetica Neue', Sans-serif;
      padding: 0;
      width: 250px;
    }

    .mapboxgl-popup-content-wrapper {
      padding: 1%;
    }

    .mapboxgl-popup-content h3 {
      background: rgb(61, 59, 59);
      text-align: center;
      color: #fff;
      margin: 0;
      display: block;
      padding: 15px;
      font-weight: 700;
      margin-top: -5px;
    }

    .mapboxgl-popup-content h4 {
      margin: 0;
      display: block;
      padding: 10px 3px 10px 10px;
      font-weight: 400;
    }

    .mapboxgl-container {
      cursor: pointer;
    }

    .mapboxgl-popup-anchor-top>.mapboxgl-popup-content {
      margin-top: 3px;
    }

    .mapboxgl-popup-anchor-top>.mapboxgl-popup-tip {
      border-bottom-color: rgb(61, 59, 59);
    }

  </style>

  <body>

    <div id="map"></div>
    <script>
      var transformRequest = (url, resourceType) => {
        var isMapboxRequest =
          url.slice(8, 22) === "api.mapbox.com" ||
          url.slice(10, 26) === "tiles.mapbox.com";
        return {
          url: isMapboxRequest
            ? url.replace("?", "?pluginName=sheetMapper&")
            : url
        };
      };

      //YOUR TURN: add your Mapbox access token 
      mapboxgl.accessToken = 'pk.eyJ1IjoiY2Fyb2xhbmFjYXJvbCIsImEiOiJjazk0aWdqM24wNW03M2duM3M0aXhraHYwIn0.MpX8eFM2O4eOkMseDp6ioQ';
      var map = new mapboxgl.Map({
        container: 'map', // container id
        style: "mapbox://styles/mapbox/streets-v11.", //YOUR TURN: choose a style: https://docs.mapbox.com/api/maps/#styles
        center: [-122.411464, 37.7852299], // starting position [lng, lat]
        zoom: 9, // starting zoom
        transformRequest: transformRequest
      });


      map.on("load", function() {
        init();
      });

      // Initialize Tabletop to access your table
      function init() {
        Tabletop.init({
          // YOUR TURN: change 'key' value to point to your spreadsheet
          key: 'https://docs.google.com/spreadsheets/d/1BaR-vMpQzf0XTON-nHCc35Wjt9z5ZPmxejda2WlkWOY/edit?usp=sharing',
          // once Tabletop has loaded the data, it passes it to the 'callback' function, 'addPoints', defined below
          callback: addPoints,
          simpleSheet: true
        });
      }


      // create a function called addPoints that iterates through your table (row by row) to create markers and popups
      function addPoints(data) {

        // iterate through your table to set the marker to lat/long values for each row

        data.forEach(function(row) {

          // create a variable for your popup and populate your popup with information from your table
          var popup = new mapboxgl.Popup()
            .setHTML(`<h3>` + row.Name + `</h3>` + '<h4>' + '<b>' + 'Address: ' + '</b>' + row.Address + '</h4>' + '<h4>' + '<b>' + 'Phone: ' + '</b>' + row.Phone + '</h4>'); // use the table to populate your popup with text


          // create a variable for your markup and add it to the map 
          var marker = new mapboxgl.Marker({
              color: 'purple'
            })
            .setLngLat([row.Longitude, row.Latitude])
            .setPopup(popup)
            .addTo(map); // add the marker to the map


        });
      }

    </script>
  </body>

</html>
