 最近项目中用到了openlayer，版本用的是2.13的，以前没接触过，想了解一下整个的工作机制。以openlayer中的xyz-esri.html例子为例，跟了一下源码调试了整个过程，把主要过程记录下来。从map的初始化到各层的创建，最后到图片的创建和加载。

> var layerExtent = new OpenLayers.Bounds( -13758743.4295939,  5591455.28887228, -13531302.3472101 , 5757360.4178881);
            map = new OpenLayers.Map( 'map', {'restrictedExtent': layerExtent} );
            layer = new OpenLayers.Layer.XYZ( "ESRI",
                    "http://server.arcgisonline.com/ArcGIS/rest/services/World_Topo_Map/MapServer/tile/${z}/${y}/${x}",
                    {sphericalMercator: true} );
            map.addLayer(layer);
            map.zoomToExtent(map.restrictedExtent);


整个地图是由div加图片构成的，最外层是我们在body中定义的div,openlayer的对象都是从initialize方法开始执行的，所以Map初始化时，会向我们定义的div中添加div-viewport,之后是div-container,之后是XYZ图层，图层里面是tiles，即构建地图图片的元素，也就是image，图片被加载到XYZ图层不是Map初始化方法时构建的，是在zoomToExtend方法中，构建的。

执行map.zoomToExtent方法，其中执行setCenter方法，setCenter方法中执行了moveTo方法，就会执行Layer的方法，即this.baseLayer.moveTo方法，该方法执行的是Grid的moveTo方法，在此方法中执行initGriddedTiles，目的是计算当前map下能够绘制几行几列的Tiles（Image）,最后会在此方法的最后循环tiles的数组，开始执行Tile.draw方法，在此方法中触发beforedraw方法，执行的是TileManager的queueTileDraw，将这些tile一一加入缓存队列中去。
   在setCenter方法中继续执行，触发zoomEnd方法，执行TileManager的zoomEnd方法，执行TileManager中的updateTimeout方法，200ms之后立即执行drawTilesQueue方法，该方法执行tiles缓存队列中的tile,每次处理2个，触发Image的draw方法，在其中执行renderTile->InitImage->manageCache->getTile->getImage()->生成Image对象append到XYZ的div中，triggerTile的loadStart（Grid的addTileMonitorHooks）,增加图片加载的监听事件，执行loadEnd，TileManager->addToCache->setImgSrc->this.positionTile,至此layer中的image生成完毕。
      第一次是200ms后，开始执行drawTilesQueue，之后每隔16ms执行一次，直到tiles缓存队列中处理完毕。

