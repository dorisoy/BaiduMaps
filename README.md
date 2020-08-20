# Xamarin.Forms.BaiduMaps

Application of baidu map in xamarin, a simply packaging of baidu maps sdk（5.3.2）

```xml
<metadata>
  <!--
  This sample removes the class: android.support.v4.content.AsyncTaskLoader.LoadTask:
  <remove-node path="/api/package[@name='android.support.v4.content']/class[@name='AsyncTaskLoader.LoadTask']" />
  
  This sample removes the method: android.support.v4.content.CursorLoader.loadInBackground:
  <remove-node path="/api/package[@name='android.support.v4.content']/class[@name='CursorLoader']/method[@name='loadInBackground']" />
  -->
  <attr path="/api/package[@name='com.baidu.location']/class[@name='Address']/field[@name='address']" name="name">AddressInfo</attr>
  <attr path="/api/package[@name='com.baidu.mapapi']/class[@name='VersionInfo']/field[@name='VERSION_INFO']" name="name">VersionInformation</attr>
  <attr path="/api/package[@name='com.baidu.platform.comapi.map']/class[@name='B']/field[@name='b']" name="name">BField</attr>
  <attr path="/api/package[@name='com.baidu.platform.comapi.map']/class[@name='E']/field[@name='e']" name="name">EField</attr>
  <attr path="/api/package[@name='com.baidu.mapapi.search.sug']/class[@name='SuggestionSearch']" name="extends">Java.Lang.Object</attr>
  <attr path="/api/package[@name='com.baidu.mapapi.search.poi']/class[@name='PoiSearch']" name="extends">Java.Lang.Object</attr>
  <attr path="/api/package[@name='com.baidu.mapapi.search.busline']/class[@name='BusLineSearch']" name="extends">Java.Lang.Object</attr>
  <attr path="/api/package[@name='com.baidu.mapapi.search.geocode']/class[@name='GeoCoder']" name="extends">Java.Lang.Object</attr>
  <attr path="/api/package[@name='com.baidu.mapapi.search.route']/class[@name='RoutePlanSearch']" name="extends">Java.Lang.Object</attr>
  <attr path="/api/package[@name='com.baidu.mapapi.search.share']/class[@name='ShareUrlSearch']" name="extends">Java.Lang.Object</attr>
  <attr path="/api/package[@name='com.baidu.location']/class[@name='Address']/field[@name='address']" name="name">Addresses</attr>
  
  <attr path="/api/package[@name='com.baidu.mapapi.map']/interface[@name='BaiduMap.OnMapStatusChangeListener']/method[@name='onMapStatusChangeStart' and count(parameter)=2 and parameter[1][@type='com.baidu.mapapi.map.MapStatus'] and parameter[2][@type='int']]"
        name="managedName">OnMapStatusChangeStart2</attr>

  <attr path="/api/package[@name='com.baidu.mapapi.search.poi']/interface[@name='OnGetPoiSearchResultListener']/method[@name='onGetPoiDetailResult' and parameter[1][@type='com.baidu.mapapi.search.poi.PoiDetailSearchResult']]"
        name="managedName">OnGetPoiDetailResult2</attr>
  
</metadata>
```

### Screenshot

![](https://github.com/dorisoy/Xamarin.Forms.BaiduMaps/blob/master/Screenshot_20190718_193103_com.dcms.client.jpg?raw=true)
