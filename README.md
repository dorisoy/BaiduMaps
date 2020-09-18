# Xamarin.Forms.BaiduMaps

Application of baidu map in xamarin, a simply packaging of baidu maps sdk（5.3.2/6.4/7.0）

```xml
<metadata>
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

  <attr path="/api/package[@name='com.baidu.platform.comapi.wnplatform.walkmap']/class[@name='WNaviBaiduMap']/method[@name='getId' and count(parameter)=0]" name="managedName">IdVal</attr>

  <remove-node path="/api/package[@name='com.google.protobuf.micro']/class[@name='MessageMicro']/method[@name='mergeFrom' and count(parameter)=1 and parameter[1][@type='com.google.protobuf.micro.CodedInputStreamMicro']]" />

  <attr path="/api/package[@name='com.baidu.platform.comapi.map']/interface[@name='MapViewListener']/method[@name='onClickedItem' and count(parameter)=4 and parameter[1][@type='int'] and parameter[2][@type='int'] and parameter[3][@type='com.baidu.platform.comapi.basestruct.GeoPoint'] and parameter[4][@type='long']]"
name="managedName">OnClickedItem2</attr>

  <attr path="/api/package[@name='com.baidu.platform.comapi.map']/interface[@name='NaviMapViewListener']/method[@name='onMapRenderModeChange' and count(parameter)=1 and parameter[1][@type='int']]"
name="managedName">OnMapRenderModeChange2</attr>

  <attr path="/api/package[@name='com.baidu.platform.comapi.map']/interface[@name='NaviMapViewListener']/method[@name='onMapAnimationFinish' and count(parameter)=0]"
name="managedName">OnMapAnimationFinish2</attr>


  <attr path="/api/package[@name='com.baidu.platform.comapi.map']/class[@name='ItemizedOverlay']/method[@name='compare' and count(parameter)=2 and parameter[1][@type='java.lang.Integer'] and parameter[2][@type='java.lang.Integer']]" name="managedType">Java.Lang.Integer</attr>

  <add-node path="/api/package/class[implements[@name='java.lang.Comparable']]">
    <method name="compareTo" return="int" abstract="false" native="false" synchronized="false" static="false" final="false" deprecated="not deprecated" visibility="public">
      <parameter name="o" type="java.lang.Object" />
    </method>
  </add-node>

  <add-node path="/api/package/class[implements[@name='java.util.Comparator']]">
    <method name="compare" return="int" abstract="false" native="false" synchronized="false" static="false" final="false" deprecated="not deprecated" visibility="public">
      <parameter name="o1" type="java.lang.Object" />
      <parameter name="o2" type="java.lang.Object" />
    </method>
  </add-node>
</metadata>

```

### Screenshot

![](https://github.com/dorisoy/Xamarin.Forms.BaiduMaps/blob/master/Screenshot_20190718_193103_com.dcms.client.jpg?raw=true)
