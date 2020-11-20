# Xamarin.Forms.BaiduMaps

BaiduMap 地图和定位在Xamarin.Android/Xamarin.Forms 中的应用（5.3.2/6.4/7.0/7.1）

**SDK 5.3.2/6.4/7.0/7.1 通用匹配绑定规则：**

```xml
<metadata>
	<attr path="/api/package[@name='com.baidu.mapapi']/class[@name='VersionInfo']/field[@name='VERSION_INFO']"
		  name="name">VersionInformation</attr>

	<attr path="/api/package[@name='com.baidu.location']/class[@name='Address']/field[@name='address']"
		  name="name">Addresses</attr>

	<attr path="/api/package[@name='com.baidu.mapapi.map']/interface[@name='BaiduMap.OnMapStatusChangeListener']/method[@name='onMapStatusChangeStart' and count(parameter)=2 and parameter[1][@type='com.baidu.mapapi.map.MapStatus'] and parameter[2][@type='int']]"
		  name="managedName">OnMapStatusChangeStart2</attr>

	<attr path="/api/package[@name='com.baidu.mapapi.search.poi']/interface[@name='OnGetPoiSearchResultListener']/method[@name='onGetPoiDetailResult' and parameter[1][@type='com.baidu.mapapi.search.poi.PoiDetailSearchResult']]"
		  name="managedName">OnGetPoiDetailResult2</attr>

	<attr path="/api/package[@name='com.baidu.platform.comapi.map']/interface[@name='MapViewListener']/method[@name='onClickedItem' and count(parameter)=4 and parameter[1][@type='int'] and parameter[2][@type='int'] and parameter[3][@type='com.baidu.platform.comapi.basestruct.GeoPoint'] and parameter[4][@type='long']]"
		  name="managedName">OnClickedItem2</attr>

	<attr path="/api/package[@name='com.baidu.platform.comapi.map']/interface[@name='NaviMapViewListener']/method[@name='onMapRenderModeChange' and count(parameter)=1 and parameter[1][@type='int']]"
		  name="managedName">OnMapRenderModeChange2</attr>

	<attr path="/api/package[@name='com.baidu.platform.comapi.map']/interface[@name='NaviMapViewListener']/method[@name='onMapAnimationFinish' and count(parameter)=0]"
		  name="managedName">OnMapAnimationFinish2</attr>

	<attr path="/api/package[@name='com.baidu.platform.comapi.map']/class[@name='ItemizedOverlay']/method[@name='compare' and count(parameter)=2 and parameter[1][@type='java.lang.Integer'] and parameter[2][@type='java.lang.Integer']]"
		  name="managedType">Java.Lang.Integer</attr>

	<add-node path="/api/package/class[implements[@name='java.lang.Comparable']]">
		<method name="compareTo"
				return="int"
				abstract="false"
				native="false"
				synchronized="false"
				static="false"
				final="false"
				deprecated="not deprecated"
				visibility="public">
			<parameter name="o"
					   type="java.lang.Object" />
		</method>
	</add-node>

	<add-node path="/api/package/class[implements[@name='java.util.Comparator']]">
		<method name="compare"
				return="int"
				abstract="false"
				native="false"
				synchronized="false"
				static="false"
				final="false"
				deprecated="not deprecated"
				visibility="public">
			<parameter name="o1"
					   type="java.lang.Object" />
			<parameter name="o2"
					   type="java.lang.Object" />
		</method>
	</add-node>

</metadata>

```
**如果你使用单独的定位服务，可以使用如下方式：
将定位SDK的SERVICE设置成为前台服务,使用Notification， 提高定位进程存活率**

```csharp
   [Activity(Label = "App Name",
        Icon = "@mipmap/ic_launcher",
        Theme = "@style/MainTheme",
        LaunchMode = LaunchMode.SingleTop,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        private static LocationClient mLocationClient = null;
        private MyLocationListener myLocationListener = new MyLocationListener();
        private Notification mNotification;

        protected override void OnCreate(Bundle bundle)
        {

            base.OnCreate(bundle);

            TabLayoutResource = Resource.Layout.Tabbar;
            ToolbarResource = Resource.Layout.Toolbar;

            //动态请求用户允许使用定位权限
            if (ContextCompat.CheckSelfPermission(this, Manifest.Permission.AccessFineLocation) != Permission.Granted)
            {
                RequestPermissions(new string[] {
                    Manifest.Permission.AccessNetworkState,
                    Manifest.Permission.Internet,
                    Manifest.Permission.WriteExternalStorage,
                    Manifest.Permission.ReadExternalStorage,
                    Manifest.Permission.AccessCoarseLocation,
                    Manifest.Permission.AccessFineLocation,
                    Manifest.Permission.AccessWifiState,
                    Manifest.Permission.WriteSettings }, 1);
            }
            else
            {
                InitLocation();
                InitNotification();
            }

            Xamarin.Forms.Forms.Init(this, bundle);
            LoadApplication(new App(new AndroidInitializer()));
        }

        #region BaiduSDK


        /// <summary>
        /// 初始化配置
        /// </summary>
        private void InitLocation()
        {
            // 创建定位客户端
            mLocationClient = new LocationClient(this);
            myLocationListener = new MyLocationListener();
            // 注册定位监听
            mLocationClient.RegisterLocationListener(myLocationListener);
            LocationClientOption mOption = new LocationClientOption();
            //高精度
            mOption.SetLocationMode(LocationClientOption.LocationMode.HightAccuracy);
            //附近地址
            mOption.SetIsNeedAddress(true);
            // 可选，默认0，即仅定位一次，设置发起连续定位请求的间隔需要大于等于1000ms才是有效的
            mOption.ScanSpan = 1000;
            // 可选，默认gcj02，设置返回的定位结果坐标系，如果配合百度地图使用，建议设置为bd09ll;
            mOption.CoorType = "bd09ll";
            // 可选，默认false，设置是否开启Gps定位
            mOption.OpenGps = true;
            // 设置定位参数
            mLocationClient.LocOption = mOption;
        }
        /// <summary>
        /// 初始化前台服务
        /// </summary>
        private void InitNotification()
        {
            //设置后台定位
            //android8.0及以上使用NotificationUtils
            if (Build.VERSION.SdkInt >= BuildVersionCodes.O)
            {
                var notificationUtils = new NotificationUtils(this);
                var builder = notificationUtils.GetAndroidChannelNotification("MyAPP", "正在后台定位");
                mNotification = builder.Build();
            }
            else
            {
                //获取一个Notification构造器
                Notification.Builder builder = new Notification.Builder(ApplicationContext, NotificationUtils.ANDROID_CHANNEL_ID);
                Intent nfIntent = new Intent(this, typeof(MainActivity));

                var currenttimemillis = (long)(DateTime.UtcNow - new DateTime(1970, 1, 1, 0, 0, 0, DateTimeKind.Utc)).TotalMilliseconds;

                builder.SetContentIntent(PendingIntent.GetActivity(this, 0, nfIntent, 0))
                    .SetContentTitle("定位服务")
                    .SetSmallIcon(Android.Resource.Mipmap.SymDefAppIcon)
                    .SetContentText("正在后台定位")
                    .SetWhen(currenttimemillis);

                //获取构建好的Notification
                mNotification = builder.Build();
            }
        }
        /// <summary>
        /// 定义位置侦听
        /// </summary>
        private class MyLocationListener : BDAbstractLocationListener
        {
            public async override void OnReceiveLocation(BDLocation lct)
            {
                if (lct == null)
                    return;
                try
                {
                    // 此处设置开发者获取到的方向信息，顺时针0-360
                    var locData = new MyLocationData.Builder()
                        .Accuracy(lct.Radius)
                        .Direction(lct.Direction).Latitude(lct.Latitude)
                        .Longitude(lct.Longitude)
                        .Build();

                    var msg = $"OnReceiveLocation:{lct.Latitude} Latitude:{lct.Longitude} Address:{lct.Country}{lct.Province}{lct.City}{lct.Address?.Street}";
                    Log.Info("获取位置：", msg);
					
					//在这里可以实现你的本地存储
					//TODO...

                }
                catch (Java.Lang.Exception ex)
                {
                    Android.Util.Log.Error("Exception", ex.Message);
                }
            }
        }

        #endregion


        protected override void OnStart()
        {
            base.OnStart();
            //app 从后台唤醒，进入前台
            activityActive++;
            isBackground = false;
            Android.Util.Log.Info("ACTIVITY", "程序从后台唤醒");
            // 将定位SDK的SERVICE设置成为前台服务, 提高定位进程存活率
            mLocationClient?.EnableLocInForeground(1, mNotification);
            mLocationClient?.Start();
        }

        protected override void OnStop()
        {
            activityActive--;
            if (activityActive == 0)
            {
                //app 进入后台
                isBackground = true;
                //程序进入后台
                Android.Util.Log.Info("ACTIVITY", "程序进入后台");
                //关闭后台定位（true：通知栏消失；false：通知栏可手动划除）
                mLocationClient?.DisableLocInForeground(true);
                mLocationClient?.Stop();
            }
            base.OnStop();
        }

        protected override void OnDestroy()
        {
            base.OnDestroy();

            // 关闭前台定位服务
            mLocationClient?.DisableLocInForeground(true);

            //取消之前注册的 BDAbstractLocationListener 定位监听函数
            if (myLocationListener != null)
                mLocationClient?.UnRegisterLocationListener(myLocationListener);

            //停止Baidu定位服务
            mLocationClient?.Stop();
        }

        protected override void OnResume()
        {
            base.OnResume();
        }

        protected override void OnNewIntent(Intent intent)
        {
            base.OnNewIntent(intent);
        }

        protected override void OnActivityResult(int requestCode, Result resultCode, Intent intent)
        {
            base.OnActivityResult(requestCode, resultCode, intent);
        }

        public override void OnWindowFocusChanged(bool hasFocus)
        {
            base.OnWindowFocusChanged(hasFocus);
            base.Window.SetBackgroundDrawable(null);
        }

        public override void OnRequestPermissionsResult(int requestCode, string[] permissions, [GeneratedEnum] Permission[] grantResults)
        {
            base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
            Platform.OnRequestPermissionsResult(requestCode, permissions, grantResults);
        }
    }
```
**
在Xamarin.Forms 中，如果你使用基本地图视图模式，并且在加载地图时开始定位服务，你可以自定义一个MapView，定义一个IBaiduLocationService 接口用来创建位置服务，如下：**
```csharp
public class Map : View
    {
        public Map()
        {
            VerticalOptions = HorizontalOptions = LayoutOptions.FillAndExpand;
        }

        /// <summary>
        /// 地图坐标类型
        /// </summary>
        public static readonly BindableProperty MapTypeProperty = BindableProperty.Create(
            propertyName: nameof(MapType),
            returnType: typeof(MapType),
            declaringType: typeof(Map),
            defaultValue: MapType.Standard
        );
        public MapType MapType
        {
            get { return (MapType)GetValue(MapTypeProperty); }
            set { SetValue(MapTypeProperty, value); }
        }

        /// <summary>
        /// 使用跟踪模式
        /// </summary>
        public static readonly BindableProperty UserTrackingModeProperty = BindableProperty.Create(
            propertyName: nameof(UserTrackingMode),
            returnType: typeof(UserTrackingMode),
            declaringType: typeof(Map),
            defaultValue: UserTrackingMode.None
        );
        public UserTrackingMode UserTrackingMode
        {
            get { return (UserTrackingMode)GetValue(UserTrackingModeProperty); }
            set { SetValue(UserTrackingModeProperty, value); }
        }

        /// <summary>
        /// 显示用户位置
        /// </summary>
        public static readonly BindableProperty ShowUserLocationProperty = BindableProperty.Create(
            propertyName: nameof(ShowUserLocation),
            returnType: typeof(bool),
            declaringType: typeof(Map),
            defaultValue: true
        );
        public bool ShowUserLocation
        {
            get { return (bool)GetValue(ShowUserLocationProperty); }
            set { SetValue(ShowUserLocationProperty, value); }
        }

        /// <summary>
        /// 显示指南针
        /// </summary>
        public static readonly BindableProperty ShowCompassProperty = BindableProperty.Create(
            propertyName: nameof(ShowCompass),
            returnType: typeof(bool),
            declaringType: typeof(Map),
            defaultValue: true
        );
        public bool ShowCompass
        {
            get { return (bool)GetValue(ShowCompassProperty); }
            set { SetValue(ShowCompassProperty, value); }
        }

        /// <summary>
        /// 指南针位置
        /// </summary>
        public static readonly BindableProperty CompassPositionProperty = BindableProperty.Create(
            propertyName: nameof(CompassPosition),
            returnType: typeof(Point),
            declaringType: typeof(Map),
            defaultValue: new Point(40, 40)
        );
        public Point CompassPosition
        {
            get { return (Point)GetValue(CompassPositionProperty); }
            set { SetValue(CompassPositionProperty, value); }
        }

       /// <summary>
       /// 缩放比例
       /// </summary>
        public static readonly BindableProperty ZoomLevelProperty = BindableProperty.Create(
            propertyName: nameof(ZoomLevel),
            returnType: typeof(float),
            declaringType: typeof(Map),
            defaultValue: 11f
        );
        public float ZoomLevel
        {
            get { return (float)GetValue(ZoomLevelProperty); }
            set { SetValue(ZoomLevelProperty, value); }
        }

        /// <summary>
        /// 最小缩放比例
        /// </summary>
        public static readonly BindableProperty MinZoomLevelProperty = BindableProperty.Create(
            propertyName: nameof(MinZoomLevel),
            returnType: typeof(float),
            declaringType: typeof(Map),
            defaultValue: 3f
        );
        public float MinZoomLevel
        {
            get { return (float)GetValue(MinZoomLevelProperty); }
            set { SetValue(MinZoomLevelProperty, value); }
        }

        /// <summary>
        /// 最大缩放比例
        /// </summary>
        public static readonly BindableProperty MaxZoomLevelProperty = BindableProperty.Create(
            propertyName: nameof(MaxZoomLevel),
            returnType: typeof(float),
            declaringType: typeof(Map),
            defaultValue: 22f
        );
        public float MaxZoomLevel
        {
            get { return (float)GetValue(MaxZoomLevelProperty); }
            set { SetValue(MaxZoomLevelProperty, value); }
        }

        /// <summary>
        /// 中心位置
        /// </summary>
        public static readonly BindableProperty CenterProperty = BindableProperty.Create(
            propertyName: nameof(Center),
            returnType: typeof(Coordinate),
            declaringType: typeof(Map),
            defaultValue: new Coordinate(28.693, 115.958)
        );
        public Coordinate Center
        {
            get { return (Coordinate)GetValue(CenterProperty); }
            set { SetValue(CenterProperty, value); }
        }

        /// <summary>
        /// 显示缩放工具条
        /// </summary>
        public static readonly BindableProperty ShowScaleBarProperty = BindableProperty.Create(
            propertyName: nameof(ShowScaleBar),
            returnType: typeof(bool),
            declaringType: typeof(Map),
            defaultValue: true
        );
        public bool ShowScaleBar
        {
            get { return (bool)GetValue(ShowScaleBarProperty); }
            set { SetValue(ShowScaleBarProperty, value); }
        }

        /// <summary>
        /// 清除覆盖物
        /// </summary>
        public static readonly BindableProperty ClearOverlayProperty = BindableProperty.Create(
           propertyName: nameof(ClearOverlay),
           returnType: typeof(bool),
           declaringType: typeof(Map),
           defaultValue: false
       );
        public bool ClearOverlay
        {
            get { return (bool)GetValue(ClearOverlayProperty); }
            set { SetValue(ClearOverlayProperty, value); }
        }


        /// <summary>
        /// 显示缩放工具栏
        /// </summary>
        public static readonly BindableProperty ShowZoomControlProperty = BindableProperty.Create(
            propertyName: nameof(ShowZoomControl),
            returnType: typeof(bool),
            declaringType: typeof(Map),
            defaultValue: true
        );
        public bool ShowZoomControl
        {
            get { return (bool)GetValue(ShowZoomControlProperty); }
            set { SetValue(ShowZoomControlProperty, value); }
        }


        public IBaiduLocationService LocationService { get; set; }
        public IProjection Projection { get; set; }

        public IList<Pin> Pins => pins;
        private readonly ObservableCollection<Pin> pins = new ObservableCollection<Pin>();

        public IList<Polyline> Polylines => polylines;
        private readonly ObservableCollection<Polyline> polylines = new ObservableCollection<Polyline>();

        public IList<Polygon> Polygons => polygons;
        private readonly ObservableCollection<Polygon> polygons = new ObservableCollection<Polygon>();

        public IList<Circle> Circles => circles;
        private readonly ObservableCollection<Circle> circles = new ObservableCollection<Circle>();

        public event EventHandler<MapBlankClickedEventArgs> BlankClicked;
        public void SendBlankClicked(Coordinate pos)
        {
            BlankClicked?.Invoke(this, new MapBlankClickedEventArgs(pos));
        }

        public event EventHandler<MapPoiClickedEventArgs> PoiClicked;
        public void SendPoiClicked(Poi poi)
        {
            PoiClicked?.Invoke(this, new MapPoiClickedEventArgs(poi));
        }

        public event EventHandler<MapDoubleClickedEventArgs> DoubleClicked;
        public void SendDoubleClicked(Coordinate pos)
        {
            DoubleClicked?.Invoke(this, new MapDoubleClickedEventArgs(pos));
        }

        public event EventHandler<MapLongClickedEventArgs> LongClicked;
        public void SendLongClicked(Coordinate pos)
        {
            LongClicked?.Invoke(this, new MapLongClickedEventArgs(pos));
        }

        public event EventHandler<EventArgs> Loaded;
        public void SendLoaded()
        {
            Loaded?.Invoke(this, EventArgs.Empty);
        }

        public event EventHandler<EventArgs> StatusChanged;
        public void SendStatusChanged()
        {
            StatusChanged?.Invoke(this, EventArgs.Empty);
        }

    }
```

**在你的Android项目中 定义一个MapRenderer，继承自 ViewRenderer<Map, MapView>
在 OnElementChanged(ElementChangedEventArgs<Map> e) 中实现你的的接口 IBaiduLocationService**

例如：

```csharp
protected override void OnElementChanged(ElementChangedEventArgs<Map> e)
{
	base.OnElementChanged(e);

	if (null == Control)
	{
		SetNativeControl(new MapView(Context));
	}

	if (null != e.OldElement)
	{
		var oldMap = e.OldElement;
		((BaiduLocationServiceImpl)oldMap.LocationService).Unregister();
		MapView oldMapView = Control;
		oldMapView.OnDestroy();
	}

	if (null != e.NewElement)
	{
		Map.LocationService = new BaiduLocationServiceImpl(NativeMap, Context);
	}
}
```

**在你的Xaml视图中启动或者停止服务：**

```csharp
protected override void OnAppearing()
{
	try
	{
		base.OnAppearing();
		//启动服务
		map.LocationService?.Start();
	}
	catch (Exception ex)
	{
		Log.Write(ex);
	}
}
protected override void OnDisappearing()
{
	try
	{
		base.OnDisappearing();
		//停止服务
		map.LocationService?.Stop();
	}
	catch (Exception ex)
	{
		Log.Write(ex);
	}
}
```

**
你也可以把SDK进程捆绑在主进程中，去掉 android:process=":remote"，
然后把 LocationClientOption 的 ScanSpan 设置为0，默认0，即仅定位一次，然后自定义一个Service 
在服务里定义频率执行间隔定位，当然，你可以使用WorkManager，使用PeriodicWorkRequest 定义一个Worker间隔定位， 例如：**

```xml
<service android:name="com.baidu.location.f"
				 android:enabled="true"
				 android:foregroundServiceType="location" />
```
```csharp
[Service]
public class LocationService : Service
{
        CancellationTokenSource _cts;
        public override IBinder OnBind(Intent intent)
        {
            return null;
        }

        public override StartCommandResult OnStartCommand(Intent intent, StartCommandFlags flags, int startId)
        {
            _cts = new CancellationTokenSource();

            Task.Run(async () => 
            {
                Location locShared = null;
                try
                {
                    locShared = new Location();
                    await locShared.Run(_cts.Token);
                }
                catch (Android.OS.OperationCanceledException)
                {
                }
                finally
                {
                    if (_cts.IsCancellationRequested)
                    {
                        var message = new StopServiceMessage();
                        Device.BeginInvokeOnMainThread(() => MessagingCenter.Send(message, "ServiceStopped")
                        );
                    }
                }
            }, _cts.Token);

            return StartCommandResult.Sticky;
        }

        public override void OnDestroy()
        {
            if (_cts != null)
            {
                _cts.Token.ThrowIfCancellationRequested();
                _cts.Cancel();
            }
            base.OnDestroy();
        }
    }

    public class Location
    {
        readonly bool stopping = false;
        public Location()
        {
            try
            {
                //
            }
            catch (Java.Lang.Exception ex)
            {
                Android.Util.Log.Error("上报", ex.Message);
            }
        }

        public async Task Run(CancellationToken token)
        {
            await Task.Run(async () => 
            {
                while (!stopping)
                {
                    token.ThrowIfCancellationRequested();
                    try
                    {
                        await Task.Delay(2000);
                        System.Diagnostics.Debug.Print($"间隔2秒GPS定位一次.....");

                        var request = new GeolocationRequest(GeolocationAccuracy.Best);
                        var location = await Geolocation.GetLocationAsync(request);
                        if (location != null)
                        {
                            GlobalSettings.Latitude = location.Latitude;
                            GlobalSettings.Longitude = location.Longitude;
                            System.Diagnostics.Debug.Print($"{location.Latitude} {location.Longitude}");
                        }
                    }
                    catch (Exception)
                    {
                        Device.BeginInvokeOnMainThread(() =>
                        {
                            var errormessage = new LocationErrorMessage();
                            MessagingCenter.Send(errormessage, "LocationError");
                        });
                    }
                }
            }, token);
        }
    }
```

### Screenshot

![](https://github.com/dorisoy/Xamarin.Forms.BaiduMaps/blob/master/Screenshot_20190718_193103_com.dcms.client.jpg?raw=true)
