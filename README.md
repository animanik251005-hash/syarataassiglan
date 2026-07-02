<!doctype html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="initial-scale=1,user-scalable=no,maximum-scale=1,width=device-width">
        <meta name="mobile-web-app-capable" content="yes">
        <meta name="apple-mobile-web-app-capable" content="yes">
        <link rel="stylesheet" href="css/leaflet.css">
        <link rel="stylesheet" href="css/L.Control.Layers.Tree.css">
        <link rel="stylesheet" href="css/qgis2web.css">
        <link rel="stylesheet" href="css/fontawesome-all.min.css">
        <link rel="stylesheet" href="css/leaflet.photon.css">
        <style>
        html, body, #map {
            width: 100%;
            height: 100%;
            padding: 0;
            margin: 0;
        }
        </style>
        <title></title>
    </head>
    <body>
        <div id="map">
        </div>
        <script src="js/qgis2web_expressions.js"></script>
        <script src="js/leaflet.js"></script>
        <script src="js/L.Control.Layers.Tree.min.js"></script>
        <script src="js/leaflet.rotatedMarker.js"></script>
        <script src="js/leaflet.pattern.js"></script>
        <script src="js/leaflet-hash.js"></script>
        <script src="js/Autolinker.min.js"></script>
        <script src="js/rbush.min.js"></script>
        <script src="js/labelgun.min.js"></script>
        <script src="js/labels.js"></script>
        <script src="js/leaflet.photon.js"></script>
        <script src="data/SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMTiang_Listrikshp_1.js"></script>
        <script src="data/SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMSosial_dan_Kesejahteraanshp_2.js"></script>
        <script src="data/SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMRekreasishp_3.js"></script>
        <script src="data/SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLayanan_Transportasishp_4.js"></script>
        <script src="data/SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLampu_Jalanshp_5.js"></script>
        <script src="data/SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMJalanshp_6.js"></script>
        <script src="data/SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Pendidikanshp_7.js"></script>
        <script src="data/SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Kesehatanshp_8.js"></script>
        <script src="data/SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Ibadahshp_9.js"></script>
        <script src="data/SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMBalai_Pertemuanshp_10.js"></script>
        <script>
        var map = L.map('map', {
            zoomControl:false, maxZoom:28, minZoom:1
        }).fitBounds([[1.0960551644218464,103.92388579295186],[1.1013584040077544,103.93757622212468]]);
        var hash = new L.Hash(map);
        map.attributionControl.setPrefix('<a href="https://github.com/qgis2web/qgis2web" target="_blank">qgis2web</a> &middot; <a href="https://leafletjs.com" title="A JS library for interactive maps">Leaflet</a> &middot; <a href="https://qgis.org">QGIS</a>');
        var autolinker = new Autolinker({truncate: {length: 30, location: 'smart'}});
        // remove popup's row if "visible-with-data"
        function removeEmptyRowsFromPopupContent(content, feature) {
         var tempDiv = document.createElement('div');
         tempDiv.innerHTML = content;
         var rows = tempDiv.querySelectorAll('tr');
         for (var i = 0; i < rows.length; i++) {
             var td = rows[i].querySelector('td.visible-with-data');
             var key = td ? td.id : '';
             if (td && td.classList.contains('visible-with-data') && feature.properties[key] == null) {
                 rows[i].parentNode.removeChild(rows[i]);
             }
         }
         return tempDiv.innerHTML;
        }
        // modify popup if contains media
        function addClassToPopupIfMedia(content, popup) {
            var tempDiv = document.createElement('div');
            tempDiv.innerHTML = content;
            var imgTd = tempDiv.querySelector('td img');
            if (imgTd) {
                var src = imgTd.getAttribute('src');
                if (/\.(jpg|jpeg|png|gif|bmp|webp|avif)$/i.test(src)) {
                    popup._contentNode.classList.add('media');
                    setTimeout(function() {
                        popup.update();
                    }, 10);
                } else if (/\.(mp3|wav|ogg|aac)$/i.test(src)) {
                    var audio = document.createElement('audio');
                    audio.controls = true;
                    audio.src = src;
                    imgTd.parentNode.replaceChild(audio, imgTd);
                    popup._contentNode.classList.add('media');
                    setTimeout(function() {
                        popup.setContent(tempDiv.innerHTML);
                        popup.update();
                    }, 10);
                } else if (/\.(mp4|webm|ogg|mov)$/i.test(src)) {
                    var video = document.createElement('video');
                    video.controls = true;
                    video.src = src;
                    video.style.width = "400px";
                    video.style.height = "300px";
                    video.style.maxHeight = "60vh";
                    video.style.maxWidth = "60vw";
                    imgTd.parentNode.replaceChild(video, imgTd);
                    popup._contentNode.classList.add('media');
                    // Aggiorna il popup quando il video carica i metadati
                    video.addEventListener('loadedmetadata', function() {
                        popup.update();
                    });
                    setTimeout(function() {
                        popup.setContent(tempDiv.innerHTML);
                        popup.update();
                    }, 10);
                } else {
                    popup._contentNode.classList.remove('media');
                }
            } else {
                popup._contentNode.classList.remove('media');
            }
        }
        var zoomControl = L.control.zoom({
            position: 'topleft'
        }).addTo(map);
        var bounds_group = new L.featureGroup([]);
        function setBounds() {
        }
        map.createPane('pane_GoogleSatellite_0');
        map.getPane('pane_GoogleSatellite_0').style.zIndex = 400;
        var layer_GoogleSatellite_0 = L.tileLayer('https://mt1.google.com/vt/lyrs=s&x={x}&y={y}&z={z}', {
            pane: 'pane_GoogleSatellite_0',
            opacity: 1.0,
            attribution: '<a href="https://www.google.at/permissions/geoguidelines/attr-guide.html">Map data ©2015 Google</a>',
            minZoom: 1,
            maxZoom: 28,
            minNativeZoom: 0,
            maxNativeZoom: 20
        });
        layer_GoogleSatellite_0;
        map.addLayer(layer_GoogleSatellite_0);
        function pop_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMTiang_Listrikshp_1(feature, layer) {
            var popupContent = '<table>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Id'] !== null ? autolinker.link(String(feature.properties['Id']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Nama_Fasum'] !== null ? autolinker.link(String(feature.properties['Nama_Fasum']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['X'] !== null ? autolinker.link(String(feature.properties['X']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Y'] !== null ? autolinker.link(String(feature.properties['Y']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                </table>';
            var content = removeEmptyRowsFromPopupContent(popupContent, feature);
			layer.on('popupopen', function(e) {
				addClassToPopupIfMedia(content, e.popup);
			});
			layer.bindPopup(content, { maxHeight: 400 });
        }

        function style_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMTiang_Listrikshp_1_0() {
            return {
                pane: 'pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMTiang_Listrikshp_1',
                radius: 4.0,
                opacity: 1,
                color: 'rgba(35,35,35,1.0)',
                dashArray: '',
                lineCap: 'butt',
                lineJoin: 'miter',
                weight: 1,
                fill: true,
                fillOpacity: 1,
                fillColor: 'rgba(243,166,178,1.0)',
                interactive: true,
            }
        }
        map.createPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMTiang_Listrikshp_1');
        map.getPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMTiang_Listrikshp_1').style.zIndex = 401;
        map.getPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMTiang_Listrikshp_1').style['mix-blend-mode'] = 'normal';
        var layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMTiang_Listrikshp_1 = new L.geoJson(json_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMTiang_Listrikshp_1, {
            attribution: '',
            interactive: true,
            dataVar: 'json_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMTiang_Listrikshp_1',
            layerName: 'layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMTiang_Listrikshp_1',
            pane: 'pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMTiang_Listrikshp_1',
            onEachFeature: pop_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMTiang_Listrikshp_1,
            pointToLayer: function (feature, latlng) {
                var context = {
                    feature: feature,
                    variables: {}
                };
                return L.circleMarker(latlng, style_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMTiang_Listrikshp_1_0(feature));
            },
        });
        bounds_group.addLayer(layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMTiang_Listrikshp_1);
        map.addLayer(layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMTiang_Listrikshp_1);
        function pop_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMSosial_dan_Kesejahteraanshp_2(feature, layer) {
            var popupContent = '<table>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Id'] !== null ? autolinker.link(String(feature.properties['Id']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Nama_Fasos'] !== null ? autolinker.link(String(feature.properties['Nama_Fasos']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['X'] !== null ? autolinker.link(String(feature.properties['X']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Y'] !== null ? autolinker.link(String(feature.properties['Y']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                </table>';
            var content = removeEmptyRowsFromPopupContent(popupContent, feature);
			layer.on('popupopen', function(e) {
				addClassToPopupIfMedia(content, e.popup);
			});
			layer.bindPopup(content, { maxHeight: 400 });
        }

        function style_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMSosial_dan_Kesejahteraanshp_2_0() {
            return {
                pane: 'pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMSosial_dan_Kesejahteraanshp_2',
                opacity: 1,
                color: 'rgba(35,35,35,1.0)',
                dashArray: '',
                lineCap: 'butt',
                lineJoin: 'miter',
                weight: 1.0, 
                fill: true,
                fillOpacity: 1,
                fillColor: 'rgba(213,180,60,1.0)',
                interactive: true,
            }
        }
        map.createPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMSosial_dan_Kesejahteraanshp_2');
        map.getPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMSosial_dan_Kesejahteraanshp_2').style.zIndex = 402;
        map.getPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMSosial_dan_Kesejahteraanshp_2').style['mix-blend-mode'] = 'normal';
        var layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMSosial_dan_Kesejahteraanshp_2 = new L.geoJson(json_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMSosial_dan_Kesejahteraanshp_2, {
            attribution: '',
            interactive: true,
            dataVar: 'json_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMSosial_dan_Kesejahteraanshp_2',
            layerName: 'layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMSosial_dan_Kesejahteraanshp_2',
            pane: 'pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMSosial_dan_Kesejahteraanshp_2',
            onEachFeature: pop_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMSosial_dan_Kesejahteraanshp_2,
            style: style_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMSosial_dan_Kesejahteraanshp_2_0,
        });
        bounds_group.addLayer(layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMSosial_dan_Kesejahteraanshp_2);
        map.addLayer(layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMSosial_dan_Kesejahteraanshp_2);
        function pop_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMRekreasishp_3(feature, layer) {
            var popupContent = '<table>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Id'] !== null ? autolinker.link(String(feature.properties['Id']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Nama_Fasos'] !== null ? autolinker.link(String(feature.properties['Nama_Fasos']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['X'] !== null ? autolinker.link(String(feature.properties['X']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Y'] !== null ? autolinker.link(String(feature.properties['Y']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                </table>';
            var content = removeEmptyRowsFromPopupContent(popupContent, feature);
			layer.on('popupopen', function(e) {
				addClassToPopupIfMedia(content, e.popup);
			});
			layer.bindPopup(content, { maxHeight: 400 });
        }

        function style_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMRekreasishp_3_0() {
            return {
                pane: 'pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMRekreasishp_3',
                opacity: 1,
                color: 'rgba(35,35,35,1.0)',
                dashArray: '',
                lineCap: 'butt',
                lineJoin: 'miter',
                weight: 1.0, 
                fill: true,
                fillOpacity: 1,
                fillColor: 'rgba(125,139,143,1.0)',
                interactive: true,
            }
        }
        map.createPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMRekreasishp_3');
        map.getPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMRekreasishp_3').style.zIndex = 403;
        map.getPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMRekreasishp_3').style['mix-blend-mode'] = 'normal';
        var layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMRekreasishp_3 = new L.geoJson(json_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMRekreasishp_3, {
            attribution: '',
            interactive: true,
            dataVar: 'json_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMRekreasishp_3',
            layerName: 'layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMRekreasishp_3',
            pane: 'pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMRekreasishp_3',
            onEachFeature: pop_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMRekreasishp_3,
            style: style_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMRekreasishp_3_0,
        });
        bounds_group.addLayer(layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMRekreasishp_3);
        map.addLayer(layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMRekreasishp_3);
        function pop_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLayanan_Transportasishp_4(feature, layer) {
            var popupContent = '<table>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Id'] !== null ? autolinker.link(String(feature.properties['Id']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Nama_Fasos'] !== null ? autolinker.link(String(feature.properties['Nama_Fasos']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['X'] !== null ? autolinker.link(String(feature.properties['X']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Y'] !== null ? autolinker.link(String(feature.properties['Y']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                </table>';
            var content = removeEmptyRowsFromPopupContent(popupContent, feature);
			layer.on('popupopen', function(e) {
				addClassToPopupIfMedia(content, e.popup);
			});
			layer.bindPopup(content, { maxHeight: 400 });
        }

        function style_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLayanan_Transportasishp_4_0() {
            return {
                pane: 'pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLayanan_Transportasishp_4',
                opacity: 1,
                color: 'rgba(35,35,35,1.0)',
                dashArray: '',
                lineCap: 'butt',
                lineJoin: 'miter',
                weight: 1.0, 
                fill: true,
                fillOpacity: 1,
                fillColor: 'rgba(196,60,57,1.0)',
                interactive: true,
            }
        }
        map.createPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLayanan_Transportasishp_4');
        map.getPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLayanan_Transportasishp_4').style.zIndex = 404;
        map.getPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLayanan_Transportasishp_4').style['mix-blend-mode'] = 'normal';
        var layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLayanan_Transportasishp_4 = new L.geoJson(json_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLayanan_Transportasishp_4, {
            attribution: '',
            interactive: true,
            dataVar: 'json_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLayanan_Transportasishp_4',
            layerName: 'layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLayanan_Transportasishp_4',
            pane: 'pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLayanan_Transportasishp_4',
            onEachFeature: pop_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLayanan_Transportasishp_4,
            style: style_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLayanan_Transportasishp_4_0,
        });
        bounds_group.addLayer(layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLayanan_Transportasishp_4);
        map.addLayer(layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLayanan_Transportasishp_4);
        function pop_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLampu_Jalanshp_5(feature, layer) {
            var popupContent = '<table>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Id'] !== null ? autolinker.link(String(feature.properties['Id']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Nama_fasum'] !== null ? autolinker.link(String(feature.properties['Nama_fasum']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['X'] !== null ? autolinker.link(String(feature.properties['X']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Y'] !== null ? autolinker.link(String(feature.properties['Y']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                </table>';
            var content = removeEmptyRowsFromPopupContent(popupContent, feature);
			layer.on('popupopen', function(e) {
				addClassToPopupIfMedia(content, e.popup);
			});
			layer.bindPopup(content, { maxHeight: 400 });
        }

        function style_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLampu_Jalanshp_5_0() {
            return {
                pane: 'pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLampu_Jalanshp_5',
                radius: 4.0,
                opacity: 1,
                color: 'rgba(35,35,35,1.0)',
                dashArray: '',
                lineCap: 'butt',
                lineJoin: 'miter',
                weight: 1,
                fill: true,
                fillOpacity: 1,
                fillColor: 'rgba(190,207,80,1.0)',
                interactive: true,
            }
        }
        map.createPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLampu_Jalanshp_5');
        map.getPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLampu_Jalanshp_5').style.zIndex = 405;
        map.getPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLampu_Jalanshp_5').style['mix-blend-mode'] = 'normal';
        var layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLampu_Jalanshp_5 = new L.geoJson(json_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLampu_Jalanshp_5, {
            attribution: '',
            interactive: true,
            dataVar: 'json_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLampu_Jalanshp_5',
            layerName: 'layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLampu_Jalanshp_5',
            pane: 'pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLampu_Jalanshp_5',
            onEachFeature: pop_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLampu_Jalanshp_5,
            pointToLayer: function (feature, latlng) {
                var context = {
                    feature: feature,
                    variables: {}
                };
                return L.circleMarker(latlng, style_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLampu_Jalanshp_5_0(feature));
            },
        });
        bounds_group.addLayer(layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLampu_Jalanshp_5);
        map.addLayer(layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLampu_Jalanshp_5);
        function pop_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMJalanshp_6(feature, layer) {
            var popupContent = '<table>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['full_id'] !== null ? autolinker.link(String(feature.properties['full_id']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['osm_id'] !== null ? autolinker.link(String(feature.properties['osm_id']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['osm_type'] !== null ? autolinker.link(String(feature.properties['osm_type']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['highway'] !== null ? autolinker.link(String(feature.properties['highway']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['traffic_si'] !== null ? autolinker.link(String(feature.properties['traffic_si']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['traffic__1'] !== null ? autolinker.link(String(feature.properties['traffic__1']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['crossing_i'] !== null ? autolinker.link(String(feature.properties['crossing_i']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['button_ope'] !== null ? autolinker.link(String(feature.properties['button_ope']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['barrier'] !== null ? autolinker.link(String(feature.properties['barrier']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['proposed'] !== null ? autolinker.link(String(feature.properties['proposed']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['informal'] !== null ? autolinker.link(String(feature.properties['informal']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['ramp'] !== null ? autolinker.link(String(feature.properties['ramp']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['level'] !== null ? autolinker.link(String(feature.properties['level']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['oneway_con'] !== null ? autolinker.link(String(feature.properties['oneway_con']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['sidewalk_r'] !== null ? autolinker.link(String(feature.properties['sidewalk_r']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['sidewalk_l'] !== null ? autolinker.link(String(feature.properties['sidewalk_l']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['ford'] !== null ? autolinker.link(String(feature.properties['ford']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['crossing_m'] !== null ? autolinker.link(String(feature.properties['crossing_m']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['crossing'] !== null ? autolinker.link(String(feature.properties['crossing']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['lit'] !== null ? autolinker.link(String(feature.properties['lit']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['email'] !== null ? autolinker.link(String(feature.properties['email']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['bike_ride'] !== null ? autolinker.link(String(feature.properties['bike_ride']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['name_id'] !== null ? autolinker.link(String(feature.properties['name_id']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['operator_t'] !== null ? autolinker.link(String(feature.properties['operator_t']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['operator'] !== null ? autolinker.link(String(feature.properties['operator']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['capacity'] !== null ? autolinker.link(String(feature.properties['capacity']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['descriptio'] !== null ? autolinker.link(String(feature.properties['descriptio']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['aeroway'] !== null ? autolinker.link(String(feature.properties['aeroway']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['embankment'] !== null ? autolinker.link(String(feature.properties['embankment']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['flood_pron'] !== null ? autolinker.link(String(feature.properties['flood_pron']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['width'] !== null ? autolinker.link(String(feature.properties['width']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['maxspeed_a'] !== null ? autolinker.link(String(feature.properties['maxspeed_a']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['maxheight'] !== null ? autolinker.link(String(feature.properties['maxheight']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['constructi'] !== null ? autolinker.link(String(feature.properties['constructi']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['footway'] !== null ? autolinker.link(String(feature.properties['footway']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['public_tra'] !== null ? autolinker.link(String(feature.properties['public_tra']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['oneway_mot'] !== null ? autolinker.link(String(feature.properties['oneway_mot']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['man_made'] !== null ? autolinker.link(String(feature.properties['man_made']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['incline'] !== null ? autolinker.link(String(feature.properties['incline']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['cycleway'] !== null ? autolinker.link(String(feature.properties['cycleway']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['postal_cod'] !== null ? autolinker.link(String(feature.properties['postal_cod']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['key'] !== null ? autolinker.link(String(feature.properties['key']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['addr_count'] !== null ? autolinker.link(String(feature.properties['addr_count']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['addr_city'] !== null ? autolinker.link(String(feature.properties['addr_city']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['maxweight_'] !== null ? autolinker.link(String(feature.properties['maxweight_']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['garmin_typ'] !== null ? autolinker.link(String(feature.properties['garmin_typ']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['motor_vehi'] !== null ? autolinker.link(String(feature.properties['motor_vehi']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['roundabout'] !== null ? autolinker.link(String(feature.properties['roundabout']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['wikidata'] !== null ? autolinker.link(String(feature.properties['wikidata']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['tunnel'] !== null ? autolinker.link(String(feature.properties['tunnel']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['name_en'] !== null ? autolinker.link(String(feature.properties['name_en']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['import'] !== null ? autolinker.link(String(feature.properties['import']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['tracktype'] !== null ? autolinker.link(String(feature.properties['tracktype']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['name_ja'] !== null ? autolinker.link(String(feature.properties['name_ja']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['parking_la'] !== null ? autolinker.link(String(feature.properties['parking_la']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['bridge_str'] !== null ? autolinker.link(String(feature.properties['bridge_str']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['covered'] !== null ? autolinker.link(String(feature.properties['covered']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['check_date'] !== null ? autolinker.link(String(feature.properties['check_date']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['junction'] !== null ? autolinker.link(String(feature.properties['junction']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['service'] !== null ? autolinker.link(String(feature.properties['service']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['sidewalk'] !== null ? autolinker.link(String(feature.properties['sidewalk']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['bridge_nam'] !== null ? autolinker.link(String(feature.properties['bridge_nam']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['horse'] !== null ? autolinker.link(String(feature.properties['horse']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['alt_name'] !== null ? autolinker.link(String(feature.properties['alt_name']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['lane_marki'] !== null ? autolinker.link(String(feature.properties['lane_marki']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['moped'] !== null ? autolinker.link(String(feature.properties['moped']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['mofa'] !== null ? autolinker.link(String(feature.properties['mofa']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['ref'] !== null ? autolinker.link(String(feature.properties['ref']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['lanes'] !== null ? autolinker.link(String(feature.properties['lanes']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['layer'] !== null ? autolinker.link(String(feature.properties['layer']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['bridge'] !== null ? autolinker.link(String(feature.properties['bridge']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['smoothness'] !== null ? autolinker.link(String(feature.properties['smoothness']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['surface'] !== null ? autolinker.link(String(feature.properties['surface']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['maxspeed'] !== null ? autolinker.link(String(feature.properties['maxspeed']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['motorcycle'] !== null ? autolinker.link(String(feature.properties['motorcycle']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['motor_ve_1'] !== null ? autolinker.link(String(feature.properties['motor_ve_1']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['destinatio'] !== null ? autolinker.link(String(feature.properties['destinatio']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['taxi'] !== null ? autolinker.link(String(feature.properties['taxi']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['psv'] !== null ? autolinker.link(String(feature.properties['psv']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['motorcar'] !== null ? autolinker.link(String(feature.properties['motorcar']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['hgv'] !== null ? autolinker.link(String(feature.properties['hgv']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['goods'] !== null ? autolinker.link(String(feature.properties['goods']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['foot'] !== null ? autolinker.link(String(feature.properties['foot']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['emergency'] !== null ? autolinker.link(String(feature.properties['emergency']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['bicycle'] !== null ? autolinker.link(String(feature.properties['bicycle']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['catmp_Road'] !== null ? autolinker.link(String(feature.properties['catmp_Road']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['sport'] !== null ? autolinker.link(String(feature.properties['sport']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['access'] !== null ? autolinker.link(String(feature.properties['access']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['oneway'] !== null ? autolinker.link(String(feature.properties['oneway']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['name'] !== null ? autolinker.link(String(feature.properties['name']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                </table>';
            var content = removeEmptyRowsFromPopupContent(popupContent, feature);
			layer.on('popupopen', function(e) {
				addClassToPopupIfMedia(content, e.popup);
			});
			layer.bindPopup(content, { maxHeight: 400 });
        }

        function style_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMJalanshp_6_0() {
            return {
                pane: 'pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMJalanshp_6',
                opacity: 1,
                color: 'rgba(183,72,75,1.0)',
                dashArray: '',
                lineCap: 'square',
                lineJoin: 'bevel',
                weight: 1.0,
                fillOpacity: 0,
                interactive: true,
            }
        }
        map.createPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMJalanshp_6');
        map.getPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMJalanshp_6').style.zIndex = 406;
        map.getPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMJalanshp_6').style['mix-blend-mode'] = 'normal';
        var layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMJalanshp_6 = new L.geoJson(json_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMJalanshp_6, {
            attribution: '',
            interactive: true,
            dataVar: 'json_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMJalanshp_6',
            layerName: 'layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMJalanshp_6',
            pane: 'pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMJalanshp_6',
            onEachFeature: pop_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMJalanshp_6,
            style: style_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMJalanshp_6_0,
        });
        bounds_group.addLayer(layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMJalanshp_6);
        map.addLayer(layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMJalanshp_6);
        function pop_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Pendidikanshp_7(feature, layer) {
            var popupContent = '<table>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Id'] !== null ? autolinker.link(String(feature.properties['Id']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Nama_Sekol'] !== null ? autolinker.link(String(feature.properties['Nama_Sekol']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['X'] !== null ? autolinker.link(String(feature.properties['X']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Y'] !== null ? autolinker.link(String(feature.properties['Y']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                </table>';
            var content = removeEmptyRowsFromPopupContent(popupContent, feature);
			layer.on('popupopen', function(e) {
				addClassToPopupIfMedia(content, e.popup);
			});
			layer.bindPopup(content, { maxHeight: 400 });
        }

        function style_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Pendidikanshp_7_0() {
            return {
                pane: 'pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Pendidikanshp_7',
                opacity: 1,
                color: 'rgba(35,35,35,1.0)',
                dashArray: '',
                lineCap: 'butt',
                lineJoin: 'miter',
                weight: 1.0, 
                fill: true,
                fillOpacity: 1,
                fillColor: 'rgba(141,90,153,1.0)',
                interactive: true,
            }
        }
        map.createPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Pendidikanshp_7');
        map.getPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Pendidikanshp_7').style.zIndex = 407;
        map.getPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Pendidikanshp_7').style['mix-blend-mode'] = 'normal';
        var layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Pendidikanshp_7 = new L.geoJson(json_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Pendidikanshp_7, {
            attribution: '',
            interactive: true,
            dataVar: 'json_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Pendidikanshp_7',
            layerName: 'layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Pendidikanshp_7',
            pane: 'pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Pendidikanshp_7',
            onEachFeature: pop_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Pendidikanshp_7,
            style: style_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Pendidikanshp_7_0,
        });
        bounds_group.addLayer(layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Pendidikanshp_7);
        map.addLayer(layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Pendidikanshp_7);
        function pop_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Kesehatanshp_8(feature, layer) {
            var popupContent = '<table>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Id'] !== null ? autolinker.link(String(feature.properties['Id']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Nama_Fasos'] !== null ? autolinker.link(String(feature.properties['Nama_Fasos']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['X'] !== null ? autolinker.link(String(feature.properties['X']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Y'] !== null ? autolinker.link(String(feature.properties['Y']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                </table>';
            var content = removeEmptyRowsFromPopupContent(popupContent, feature);
			layer.on('popupopen', function(e) {
				addClassToPopupIfMedia(content, e.popup);
			});
			layer.bindPopup(content, { maxHeight: 400 });
        }

        function style_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Kesehatanshp_8_0() {
            return {
                pane: 'pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Kesehatanshp_8',
                opacity: 1,
                color: 'rgba(35,35,35,1.0)',
                dashArray: '',
                lineCap: 'butt',
                lineJoin: 'miter',
                weight: 1.0, 
                fill: true,
                fillOpacity: 1,
                fillColor: 'rgba(213,180,60,1.0)',
                interactive: true,
            }
        }
        map.createPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Kesehatanshp_8');
        map.getPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Kesehatanshp_8').style.zIndex = 408;
        map.getPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Kesehatanshp_8').style['mix-blend-mode'] = 'normal';
        var layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Kesehatanshp_8 = new L.geoJson(json_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Kesehatanshp_8, {
            attribution: '',
            interactive: true,
            dataVar: 'json_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Kesehatanshp_8',
            layerName: 'layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Kesehatanshp_8',
            pane: 'pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Kesehatanshp_8',
            onEachFeature: pop_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Kesehatanshp_8,
            style: style_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Kesehatanshp_8_0,
        });
        bounds_group.addLayer(layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Kesehatanshp_8);
        map.addLayer(layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Kesehatanshp_8);
        function pop_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Ibadahshp_9(feature, layer) {
            var popupContent = '<table>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Id'] !== null ? autolinker.link(String(feature.properties['Id']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Nama_Fasos'] !== null ? autolinker.link(String(feature.properties['Nama_Fasos']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['X'] !== null ? autolinker.link(String(feature.properties['X']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Y'] !== null ? autolinker.link(String(feature.properties['Y']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                </table>';
            var content = removeEmptyRowsFromPopupContent(popupContent, feature);
			layer.on('popupopen', function(e) {
				addClassToPopupIfMedia(content, e.popup);
			});
			layer.bindPopup(content, { maxHeight: 400 });
        }

        function style_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Ibadahshp_9_0() {
            return {
                pane: 'pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Ibadahshp_9',
                opacity: 1,
                color: 'rgba(35,35,35,1.0)',
                dashArray: '',
                lineCap: 'butt',
                lineJoin: 'miter',
                weight: 1.0, 
                fill: true,
                fillOpacity: 1,
                fillColor: 'rgba(125,139,143,1.0)',
                interactive: true,
            }
        }
        map.createPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Ibadahshp_9');
        map.getPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Ibadahshp_9').style.zIndex = 409;
        map.getPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Ibadahshp_9').style['mix-blend-mode'] = 'normal';
        var layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Ibadahshp_9 = new L.geoJson(json_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Ibadahshp_9, {
            attribution: '',
            interactive: true,
            dataVar: 'json_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Ibadahshp_9',
            layerName: 'layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Ibadahshp_9',
            pane: 'pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Ibadahshp_9',
            onEachFeature: pop_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Ibadahshp_9,
            style: style_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Ibadahshp_9_0,
        });
        bounds_group.addLayer(layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Ibadahshp_9);
        map.addLayer(layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Ibadahshp_9);
        function pop_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMBalai_Pertemuanshp_10(feature, layer) {
            var popupContent = '<table>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Id'] !== null ? autolinker.link(String(feature.properties['Id']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Nama_Fasos'] !== null ? autolinker.link(String(feature.properties['Nama_Fasos']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['X'] !== null ? autolinker.link(String(feature.properties['X']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                    <tr>\
                        <td colspan="2">' + (feature.properties['Y'] !== null ? autolinker.link(String(feature.properties['Y']).replace(/'/g, '\'').toLocaleString()) : '') + '</td>\
                    </tr>\
                </table>';
            var content = removeEmptyRowsFromPopupContent(popupContent, feature);
			layer.on('popupopen', function(e) {
				addClassToPopupIfMedia(content, e.popup);
			});
			layer.bindPopup(content, { maxHeight: 400 });
        }

        function style_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMBalai_Pertemuanshp_10_0() {
            return {
                pane: 'pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMBalai_Pertemuanshp_10',
                opacity: 1,
                color: 'rgba(35,35,35,1.0)',
                dashArray: '',
                lineCap: 'butt',
                lineJoin: 'miter',
                weight: 1.0, 
                fill: true,
                fillOpacity: 1,
                fillColor: 'rgba(196,60,57,1.0)',
                interactive: true,
            }
        }
        map.createPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMBalai_Pertemuanshp_10');
        map.getPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMBalai_Pertemuanshp_10').style.zIndex = 410;
        map.getPane('pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMBalai_Pertemuanshp_10').style['mix-blend-mode'] = 'normal';
        var layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMBalai_Pertemuanshp_10 = new L.geoJson(json_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMBalai_Pertemuanshp_10, {
            attribution: '',
            interactive: true,
            dataVar: 'json_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMBalai_Pertemuanshp_10',
            layerName: 'layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMBalai_Pertemuanshp_10',
            pane: 'pane_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMBalai_Pertemuanshp_10',
            onEachFeature: pop_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMBalai_Pertemuanshp_10,
            style: style_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMBalai_Pertemuanshp_10_0,
        });
        bounds_group.addLayer(layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMBalai_Pertemuanshp_10);
        map.addLayer(layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMBalai_Pertemuanshp_10);
        setBounds();
        var i = 0;
        layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMTiang_Listrikshp_1.eachLayer(function(layer) {
            var context = {
                feature: layer.feature,
                variables: {}
            };
            layer.bindTooltip((layer.feature.properties['Nama_Fasum'] !== null?String('<div style="color: #323232; font-size: 10pt; font-family: \'Open Sans\', sans-serif;">' + layer.feature.properties['Nama_Fasum']) + '</div>':''), {permanent: true, offset: [-0, -16], className: 'css_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMTiang_Listrikshp_1'});
            labels.push(layer);
            totalMarkers += 1;
              layer.added = true;
              addLabel(layer, i);
              i++;
        });
        var i = 0;
        layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMSosial_dan_Kesejahteraanshp_2.eachLayer(function(layer) {
            var context = {
                feature: layer.feature,
                variables: {}
            };
            layer.bindTooltip((layer.feature.properties['Nama_Fasos'] !== null?String('<div style="color: #323232; font-size: 10pt; font-family: \'Open Sans\', sans-serif;">' + layer.feature.properties['Nama_Fasos']) + '</div>':''), {permanent: true, offset: [-0, -16], className: 'css_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMSosial_dan_Kesejahteraanshp_2'});
            labels.push(layer);
            totalMarkers += 1;
              layer.added = true;
              addLabel(layer, i);
              i++;
        });
        var i = 0;
        layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMRekreasishp_3.eachLayer(function(layer) {
            var context = {
                feature: layer.feature,
                variables: {}
            };
            layer.bindTooltip((layer.feature.properties['Nama_Fasos'] !== null?String('<div style="color: #323232; font-size: 10pt; font-family: \'Open Sans\', sans-serif;">' + layer.feature.properties['Nama_Fasos']) + '</div>':''), {permanent: true, offset: [-0, -16], className: 'css_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMRekreasishp_3'});
            labels.push(layer);
            totalMarkers += 1;
              layer.added = true;
              addLabel(layer, i);
              i++;
        });
        var i = 0;
        layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLayanan_Transportasishp_4.eachLayer(function(layer) {
            var context = {
                feature: layer.feature,
                variables: {}
            };
            layer.bindTooltip((layer.feature.properties['Nama_Fasos'] !== null?String('<div style="color: #323232; font-size: 10pt; font-family: \'Open Sans\', sans-serif;">' + layer.feature.properties['Nama_Fasos']) + '</div>':''), {permanent: true, offset: [-0, -16], className: 'css_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLayanan_Transportasishp_4'});
            labels.push(layer);
            totalMarkers += 1;
              layer.added = true;
              addLabel(layer, i);
              i++;
        });
        var i = 0;
        layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLampu_Jalanshp_5.eachLayer(function(layer) {
            var context = {
                feature: layer.feature,
                variables: {}
            };
            layer.bindTooltip((layer.feature.properties['Nama_fasum'] !== null?String('<div style="color: #323232; font-size: 10pt; font-family: \'Open Sans\', sans-serif;">' + layer.feature.properties['Nama_fasum']) + '</div>':''), {permanent: true, offset: [-0, -16], className: 'css_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLampu_Jalanshp_5'});
            labels.push(layer);
            totalMarkers += 1;
              layer.added = true;
              addLabel(layer, i);
              i++;
        });
        var i = 0;
        layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMJalanshp_6.eachLayer(function(layer) {
            var context = {
                feature: layer.feature,
                variables: {}
            };
            layer.bindTooltip((layer.feature.properties['name_id'] !== null?String('<div style="color: #323232; font-size: 10pt; font-family: \'Open Sans\', sans-serif;">' + layer.feature.properties['name_id']) + '</div>':''), {permanent: true, offset: [-0, -16], className: 'css_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMJalanshp_6'});
            labels.push(layer);
            totalMarkers += 1;
              layer.added = true;
              addLabel(layer, i);
              i++;
        });
        var i = 0;
        layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Pendidikanshp_7.eachLayer(function(layer) {
            var context = {
                feature: layer.feature,
                variables: {}
            };
            layer.bindTooltip((layer.feature.properties['Nama_Sekol'] !== null?String('<div style="color: #323232; font-size: 10pt; font-family: \'Open Sans\', sans-serif;">' + layer.feature.properties['Nama_Sekol']) + '</div>':''), {permanent: true, offset: [-0, -16], className: 'css_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Pendidikanshp_7'});
            labels.push(layer);
            totalMarkers += 1;
              layer.added = true;
              addLabel(layer, i);
              i++;
        });
        var i = 0;
        layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Kesehatanshp_8.eachLayer(function(layer) {
            var context = {
                feature: layer.feature,
                variables: {}
            };
            layer.bindTooltip((layer.feature.properties['Nama_Fasos'] !== null?String('<div style="color: #323232; font-size: 10pt; font-family: \'Open Sans\', sans-serif;">' + layer.feature.properties['Nama_Fasos']) + '</div>':''), {permanent: true, offset: [-0, -16], className: 'css_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Kesehatanshp_8'});
            labels.push(layer);
            totalMarkers += 1;
              layer.added = true;
              addLabel(layer, i);
              i++;
        });
        var i = 0;
        layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Ibadahshp_9.eachLayer(function(layer) {
            var context = {
                feature: layer.feature,
                variables: {}
            };
            layer.bindTooltip((layer.feature.properties['Nama_Fasos'] !== null?String('<div style="color: #323232; font-size: 10pt; font-family: \'Open Sans\', sans-serif;">' + layer.feature.properties['Nama_Fasos']) + '</div>':''), {permanent: true, offset: [-0, -16], className: 'css_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Ibadahshp_9'});
            labels.push(layer);
            totalMarkers += 1;
              layer.added = true;
              addLabel(layer, i);
              i++;
        });
        var i = 0;
        layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMBalai_Pertemuanshp_10.eachLayer(function(layer) {
            var context = {
                feature: layer.feature,
                variables: {}
            };
            layer.bindTooltip((layer.feature.properties['Nama_Fasos'] !== null?String('<div style="color: #323232; font-size: 10pt; font-family: \'Open Sans\', sans-serif;">' + layer.feature.properties['Nama_Fasos']) + '</div>':''), {permanent: true, offset: [-0, -16], className: 'css_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMBalai_Pertemuanshp_10'});
            labels.push(layer);
            totalMarkers += 1;
              layer.added = true;
              addLabel(layer, i);
              i++;
        });
        resetLabels([layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMTiang_Listrikshp_1,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMSosial_dan_Kesejahteraanshp_2,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMRekreasishp_3,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLayanan_Transportasishp_4,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLampu_Jalanshp_5,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMJalanshp_6,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Pendidikanshp_7,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Kesehatanshp_8,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Ibadahshp_9,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMBalai_Pertemuanshp_10]);
        map.on("zoomend", function(){
            resetLabels([layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMTiang_Listrikshp_1,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMSosial_dan_Kesejahteraanshp_2,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMRekreasishp_3,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLayanan_Transportasishp_4,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLampu_Jalanshp_5,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMJalanshp_6,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Pendidikanshp_7,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Kesehatanshp_8,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Ibadahshp_9,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMBalai_Pertemuanshp_10]);
        });
        map.on("layeradd", function(){
            resetLabels([layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMTiang_Listrikshp_1,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMSosial_dan_Kesejahteraanshp_2,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMRekreasishp_3,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLayanan_Transportasishp_4,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLampu_Jalanshp_5,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMJalanshp_6,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Pendidikanshp_7,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Kesehatanshp_8,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Ibadahshp_9,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMBalai_Pertemuanshp_10]);
        });
        map.on("layerremove", function(){
            resetLabels([layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMTiang_Listrikshp_1,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMSosial_dan_Kesejahteraanshp_2,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMRekreasishp_3,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLayanan_Transportasishp_4,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMLampu_Jalanshp_5,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMJalanshp_6,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Pendidikanshp_7,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Kesehatanshp_8,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMFasilitas_Ibadahshp_9,layer_SIGPBLFASOSFASUMkel12SIGPBLFASOSFASUMBalai_Pertemuanshp_10]);
        });
        </script>        
    </body>
</html>

