# 关于Android设备唯一标识符号

### 前言

由于在开发中需要开发游客模式，在用户没有登录的情况下必须确保设备的唯一性，于是惯性思维想到的肯定是使用DevicesId 来作为设备的唯一标识，用以代替用户登录以后的唯一标识符。

但是由于国内复杂的rom定制情况，以及用户权限禁止的情况。DevicesId 在使用中并不能百分百的货到到。所以本篇文章就是描述一下，我在开发中如何处理设备唯一标识符的。

###  一、一些常用的获取设备唯一标识符的方法

* IMEI
* Mac 地址
* ANDROID_ID
* Serial Number, SN(设备序列号)
* UniquePsuedoID

#### 1.1 关于IMEI

> IMEI 国际移动设备身份码  目前GSM/WCDMA/LTE手机终端需要使用IMEI号码，在单卡工程中一个手机号对应一个IMEI号，双卡手机则会对应两个IMEI号，一张是手机卡对应一个。

#####1.1.1 关于获取IMEI过程

需要的权限

```Xml
<uses-permission android:name="android.permission.READ_PHONE_STATE"/>
```

获取IMEI 调用的方法

```java
TelephonyManager telephonyManager = (TelephonyManager)context.getSystemService(context.TELEPHONY_SERVICE);  
String imei = telephonyManager.getDeviceId();
```

##### 1.1.2 使用IMEI 存在的弊端 

由以上可以看出使用IMEI来作为Android的设备唯一标识符存在一定的弊端， 如果用户禁用掉相关权限，那么对于以上获取参数的代码。则会直接报错，不会得到我们想要的内容。

#### 1.2Mac地址

> Mac 指的就是我们设备网卡的唯一设别码，该码全球唯一，一般称为物理地址，硬件地址用来定义设备的位置

##### 1.2.1 获取设备的Mac地址

需要的权限

```Xml
<!--访问WIFI的权限-->
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/> 
```

获取mac地址的方法

获取mac地址有一点需要注意的就是android 6.0版本后，以下注释方法不再适用，不管任何手机都会返回"02:00:00:00:00:00"这个默认的mac地址，这是googel官方为了加强权限管理而禁用了getSYstemService(Context.WIFI_SERVICE)方法来获得mac地址。

```java
        /*
        获取mac地址有一点需要注意的就是android 6.0版本后，以下注释方法不再适用，
 不管任何手机都会返回"02:00:00:00:00:00"这个默认的mac地址，
 这是googel官方为了加强权限管理而禁用了getSYstemService(Context.WIFI_SERVICE)方法来获得mac地址。
         */
        // String macAddress= "";
        // WifiManager wifiManager = (WifiManager) MyApp.getContext().getSystemService(Context.WIFI_SERVICE);
        // WifiInfo wifiInfo = wifiManager.getConnectionInfo();
        // macAddress = wifiInfo.getMacAddress();
        // return macAddress;
        
        String macAddress = null;
        StringBuffer buf = new StringBuffer();
        NetworkInterface networkInterface = null;
        try {
            networkInterface = NetworkInterface.getByName("eth1");
            if (networkInterface == null) {
                networkInterface = NetworkInterface.getByName("wlan0");
            }
            if (networkInterface == null) {
                return "02:00:00:00:00:02";
            }
            byte[] addr = networkInterface.getHardwareAddress();
            for (byte b : addr) {
                buf.append(String.format("%02X:", b));
            }
            if (buf.length() > 0) {
                buf.deleteCharAt(buf.length() - 1);
            }
            macAddress = buf.toString();
        } catch (SocketException e) {
            e.printStackTrace();
            return "02:00:00:00:00:02";
        }
        return macAddress;
    }
```

##### 1.2.2 使用Mac地址存在的弊端

1. 如果使用Mac地址最重要的一点就是手机必须具有上网功能，
2. 在Android6.0以后 google 为了运行时权限对geMacAddress();作出修改通过该方法得到的mac地址永远是一样的， 但是可以其他途径获取。


#### 1.3关于ANDROID_ID

> 在设备首次运行的时候，系统会随机生成一64位的数字，并把这个数值以16进制保存下来，这个16进制的数字就是ANDROID_ID，但是如果手机恢复出厂设置这个值会发生改变。

##### 1.3.1获取ANDROID_ID

```java
String ANDROID_ID = Settings.System.getString(getContentResolver(), Settings.System.ANDROID_ID); 
```

##### 1.3.2使用ANDROID_ID存在的弊端 

1. 手机恢复出厂设置以后该值会发生变化
2. 在国内Android定制的大环境下，有些设备是不会返回ANDROID_ID的

#### 1.4 Serial Number, SN(设备序列号)

##### 1.4.1 获取序列号

````java
String SerialNumber = android.os.Build.SERIAL;  
````

获取序列号不需要权限，但是有一定的局限性，在有些手机上会出现垃圾数据，比如红米手机返回的就是连续的非随机数

#### 1.5 UniquePsuedoID

> 具体称呼不明， 但是从以下的情况看出是一些列硬件信息拼装获取到的内容。

##### 1.5.1 具体的获取方式

```java
public static String getUniquePsuedoID()  
{ 
    String m_szDevIDShort = "35" + (Build.BOARD.length() % 10) + (Build.BRAND.length() % 10) + (Build.CPU_ABI.length() % 10) + (Build.DEVICE.length() % 10) + (Build.MANUFACTURER.length() % 10) + (Build.MODEL.length() % 10) + (Build.PRODUCT.length() % 10);  
  
    // Thanks to @Roman SL!  
    // http://stackoverflow.com/a/4789483/950427  
    // Only devices with API >= 9 have android.os.Build.SERIAL  
    // http://developer.android.com/reference/android/os/Build.html#SERIAL  
    // If a user upgrades software or roots their phone, there will be a duplicate entry  
    String serial = null;  
    try  
    {  
        serial = android.os.Build.class.getField("SERIAL").get(null).toString();  
  
        // Go ahead and return the serial for api => 9  
        return new UUID(m_szDevIDShort.hashCode(), serial.hashCode()).toString();  
    }  
    catch (Exception e)  
    {  
        // String needs to be initialized  
        serial = "serial"; // some value  
    }  
  
    // Thanks @Joe!  
    // http://stackoverflow.com/a/2853253/950427  
    // Finally, combine the values we have found by using the UUID class to create a unique identifier  
    return new UUID(m_szDevIDShort.hashCode(), serial.hashCode()).toString();  
}  
```

