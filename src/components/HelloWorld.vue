<template>
<div>
  <div id="map" class="map">
    
  </div>
  <form class="form-inline">
      <label>Geometry type &nbsp;</label>
      <select id="type">
        <option value="Point">Point</option>
        <option value="LineString">LineString</option>
        <option value="Polygon">Polygon</option>
        <option value="Circle">Circle</option>
        <option value="Rect">Rect</option>
      </select>
    </form>
</div>
  
</template>

<script>

export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  mounted(){

var raster = new ol.layer.Tile({
        source: new ol.source.OSM()
      });

      var source = new ol.source.Vector();
      var vector = new ol.layer.Vector({
        source: source,
        style: new ol.style.Style({
          fill: new ol.style.Fill({
            color: 'rgba(255, 255, 255, 0.2)'
          }),
          stroke: new ol.style.Stroke({
            color: '#ffcc33',
            width: 2
          }),
          image: new ol.style.Circle({
            radius: 7,
            fill: new ol.style.Fill({
              color: '#ffcc33'
            })
          })
        })
      });

      var map = new ol.Map({
        layers: [raster, vector],
        target: 'map',
        view: new ol.View({
          center: [-11000000, 4600000],
          zoom: 4
        })
      });

      var modify = new ol.interaction.Modify({source: source});
      map.addInteraction(modify);

      var draw, snap; // global so we can remove them later
      var typeSelect = document.getElementById('type');

      function addInteractions() {
        if(typeSelect.value==='Rect'){
          draw = new ol.interaction.Draw({
              source: source,
              type: 'LineString',
              style: new ol.style.Style({
                  fill: new ol.style.Fill({
                      color: 'rgba(255, 255, 255, 0.2)'
                  }),
                  stroke: new ol.style.Stroke({
                      color: '#ffcc33',
                      width: 2
                  }),
                  image: new ol.style.Circle({
                      radius: 7,
                      fill: new ol.style.Fill({
                          color: '#ffcc33'
                      })
                  })
              }),
              maxPoints: 2,
              geometryFunction: function(coordinates, geometry){
                  if(!geometry){
                      geometry = new ol.geom.Polygon(null);
                  }
                  var start = coordinates[0];
                  var end = coordinates[1];
                  geometry.setCoordinates([
                      [start, [start[0], end[1]], end, [end[0], start[1]], start]
                  ]);
                  return geometry;
              }
          })
        }else{
          draw = new ol.interaction.Draw({
            source: source,
            type: typeSelect.value
          });
        }
        
        map.addInteraction(draw);
        snap = new ol.interaction.Snap({source: source});
        map.addInteraction(snap);

      }

      /**
       * Handle change event.
       */
      typeSelect.onchange = function() {
        map.removeInteraction(draw);
        map.removeInteraction(snap);
        addInteractions();
      };

      addInteractions();
      
      var mapClick = function(evt){

        var feature = map.forEachFeatureAtPixel(evt.pixel,
            function (feature) {
               return feature;
               
            }
        );
        // console.log(feature);

        source.getFeatures().forEach(function(ele){
          // console.log(ele);

        var features = source.getFeatures();  
          if(features.length>0){  
             for(var j=0;j<features.length;j++){  
                if(features[j].getGeometry().getType()=="LineString"){  
                   var Line = features[j];  
                   var Points = Line.getGeometry().getCoordinates();  
                    console.log('--------------------------------');
                   console.log(Points);
                  //  for (var k = 0; k < Points.length; k++) {  
                  //       var p = Point.getGeometry().getLastCoordinate();  
                  //       if (Points[k][0] == p[0] && Points[k][1] == p[1]) {  
                  //           console.log(Line)
                  //           return Line;  
                  //       }  
                  //   }  
                }  
             } 
          }
        })
        




      }


      map.on('click', mapClick);
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.map{
  width: 100%;
  height: 100vh;
  background: #efefef;
  position: absolute;
  top: 0;
  z-index: -1;
}
</style>
