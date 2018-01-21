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

##### 1.5.2 关于使用UniquePsuedoID的弊端

1. 由于是与设备信息直接相关，如果是同一批次出厂的的设备有可能出现生成的内容可能是一样的。（通过模拟器实验过，打开两个完全一样的模拟器，生成的内容是完全一下），所以如果单独使用该方法也是不能用于生成唯一标识符的。

#### 1.6 以上方法比较

比较以上5种方法。如果只是考虑单独使用，那么在不同程度上在用户使用的情况下都会出现无法生成或者生成无效设备唯一标识符的情况。所以我在开发中采用混合使用的方式，同时结合SD卡 以及 sharepreference进行本地持久化处理。

### 二、我所使用的获取设备唯一标识实现方式

#### 2.1 实现简要描述

在开发中通过结合 device_id 、 MacAddress 以及 随机生成的 UUID 进行生成设备唯一标识符（优先级依次由高到低）。 然后通过把生成的唯一标识符写到SD卡中一个隐藏目录中（为什么是写到影藏文件中呢，主要是避避免用户看见然后手动删除。）。 同时会在相应App sharepreference 进行保存一份，该内容只要内容只会在App 第一次启动或者App清除数据以后会进行重写操作，在其他时候不会进行写操作，只会进行读取操作。

#### 2.2 如何生成唯一标识符

##### 2.2.1 先上代码

```java
package com.wdsunday.utils;

import android.content.Context;
import android.os.Environment;
import android.telephony.TelephonyManager;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.io.Writer;
import java.net.NetworkInterface;
import java.net.SocketException;
import java.security.MessageDigest;
import java.util.UUID;

/**
 * @author liangjun on 2018/1/21.
 */

public class GetDeviceId {

    //保存文件的路径
    private static final String CACHE_IMAGE_DIR = "aray/cache/devices";
    //保存的文件 采用隐藏文件的形式进行保存
    private static final String DEVICES_FILE_NAME = ".DEVICES";

    /**
     * 获取设备唯一标识符
     *
     * @param context
     * @return
     */
    public static String getDeviceId(Context context) {
        //读取保存的在sd卡中的唯一标识符
        String deviceId = readDeviceID(context);
        //用于生成最终的唯一标识符
        StringBuffer s = new StringBuffer();
        //判断是否已经生成过,
        if (deviceId != null && !"".equals(deviceId)) {
            return deviceId;
        }
        try {
            //获取IMES(也就是常说的DeviceId)
            deviceId = getIMIEStatus(context);
            s.append(deviceId);
        } catch (Exception e) {
            e.printStackTrace();
        }

        try {
            //获取设备的MACAddress地址 去掉中间相隔的冒号
            deviceId = getLocalMac(context).replace(":", "");
            s.append(deviceId);
        } catch (Exception e) {
            e.printStackTrace();
        }
//        }

        //如果以上搜没有获取相应的则自己生成相应的UUID作为相应设备唯一标识符
        if (s == null || s.length() <= 0) {
            UUID uuid = UUID.randomUUID();
            deviceId = uuid.toString().replace("-", "");
            s.append(deviceId);
        }
        //为了统一格式对设备的唯一标识进行md5加密 最终生成32位字符串
        String md5 = getMD5(s.toString(), false);
        if (s.length() > 0) {
            //持久化操作, 进行保存到SD卡中
            saveDeviceID(md5, context);
        }
        return md5;
    }


    /**
     * 读取固定的文件中的内容,这里就是读取sd卡中保存的设备唯一标识符
     *
     * @param context
     * @return
     */
    public static String readDeviceID(Context context) {
        File file = getDevicesDir(context);
        StringBuffer buffer = new StringBuffer();
        try {
            FileInputStream fis = new FileInputStream(file);
            InputStreamReader isr = new InputStreamReader(fis, "UTF-8");
            Reader in = new BufferedReader(isr);
            int i;
            while ((i = in.read()) > -1) {
                buffer.append((char) i);
            }
            in.close();
            return buffer.toString();
        } catch (IOException e) {
            e.printStackTrace();
            return null;
        }
    }

    /**
     * 获取设备的DeviceId(IMES) 这里需要相应的权限<br/>
     * 需要 READ_PHONE_STATE 权限
     *
     * @param context
     * @return
     */
    private static String getIMIEStatus(Context context) {
        TelephonyManager tm = (TelephonyManager) context
                .getSystemService(Context.TELEPHONY_SERVICE);
        String deviceId = tm.getDeviceId();
        return deviceId;
    }


    /**
     * 获取设备MAC 地址 由于 6.0 以后 WifiManager 得到的 MacAddress得到都是 相同的没有意义的内容
     * 所以采用以下方法获取Mac地址
     * @param context
     * @return
     */
    private static String getLocalMac(Context context) {
//        WifiManager wifi = (WifiManager) context
//                .getSystemService(Context.WIFI_SERVICE);
//        WifiInfo info = wifi.getConnectionInfo();
//        return info.getMacAddress();


        String macAddress = null;
        StringBuffer buf = new StringBuffer();
        NetworkInterface networkInterface = null;
        try {
            networkInterface = NetworkInterface.getByName("eth1");
            if (networkInterface == null) {
                networkInterface = NetworkInterface.getByName("wlan0");
            }
            if (networkInterface == null) {
                return "";
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
            return "";
        }
        return macAddress;


    }

    /**
     * 保存 内容到 SD卡中,  这里保存的就是 设备唯一标识符
     * @param str
     * @param context
     */
    public static void saveDeviceID(String str, Context context) {
        File file = getDevicesDir(context);
        try {
            FileOutputStream fos = new FileOutputStream(file);
            Writer out = new OutputStreamWriter(fos, "UTF-8");
            out.write(str);
            out.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 对挺特定的 内容进行 md5 加密
     * @param message  加密明文
     * @param upperCase  加密以后的字符串是是大写还是小写  true 大写  false 小写
     * @return
     */
    public static String getMD5(String message, boolean upperCase) {
        String md5str = "";
        try {
            MessageDigest md = MessageDigest.getInstance("MD5");

            byte[] input = message.getBytes();

            byte[] buff = md.digest(input);

            md5str = bytesToHex(buff, upperCase);

        } catch (Exception e) {
            e.printStackTrace();
        }
        return md5str;
    }


    public static String bytesToHex(byte[] bytes, boolean upperCase) {
        StringBuffer md5str = new StringBuffer();
        int digital;
        for (int i = 0; i < bytes.length; i++) {
            digital = bytes[i];

            if (digital < 0) {
                digital += 256;
            }
            if (digital < 16) {
                md5str.append("0");
            }
            md5str.append(Integer.toHexString(digital));
        }
        if (upperCase) {
            return md5str.toString().toUpperCase();
        }
        return md5str.toString().toLowerCase();
    }

    /**
     * 统一处理设备唯一标识 保存的文件的地址
     * @param context
     * @return
     */
    private static File getDevicesDir(Context context) {
        File mCropFile = null;
        if (Environment.getExternalStorageState().equals(Environment.MEDIA_MOUNTED)) {
            File cropdir = new File(Environment.getExternalStorageDirectory(), CACHE_IMAGE_DIR);
            if (!cropdir.exists()) {
                cropdir.mkdirs();
            }
            mCropFile = new File(cropdir, DEVICES_FILE_NAME); // 用当前时间给取得的图片命名
        } else {
            File cropdir = new File(context.getFilesDir(), CACHE_IMAGE_DIR);
            if (!cropdir.exists()) {
                cropdir.mkdirs();
            }
            mCropFile = new File(cropdir, DEVICES_FILE_NAME);
        }
        return mCropFile;
    }
}

```

以上代码就是生成 设备唯一标识的具体实现方式。同时包括读取设备唯一标识的方法 。 关键点的说明已经在注释中进行描述。这里不再重复讲述。

##### 2.2.2具体的使用 

在app 的启动页中增加以下代码 

```java
new Thread(new Runnable() {
            @Override
            public void run() {
                try {
                    //获取保存在sd中的 设备唯一标识符
                    String readDeviceID = Utils.readDeviceID(WelcomeActivity.this);
                    //获取缓存在  sharepreference 里面的 设备唯一标识
                    String string = SimplePreference.getPreference(WelcomeActivity.this).getString(SpConstant.SP_DEVICES_ID, readDeviceID);
                    //判断 app 内部是否已经缓存,  若已经缓存则使用app 缓存的 设备id
                    if (string != null) {
                        //app 缓存的和SD卡中保存的不相同 以app 保存的为准, 同时更新SD卡中保存的 唯一标识符
                        if (StringUtil.isBlank(readDeviceID) && !string.equals(readDeviceID)) {
                            // 取有效地 app缓存 进行更新操作
                            if (StringUtil.isBlank(readDeviceID) && !StringUtil.isBlank(string)) {
                                readDeviceID = string;
                                Utils.saveDeviceID(readDeviceID, WelcomeActivity.this);
                            }
                        }
                    }
                    // app 没有缓存 (这种情况只会发生在第一次启动的时候)
                    if (StringUtil.isBlank(readDeviceID)) {
                        //保存设备id
                        readDeviceID = Utils.getDeviceId(WelcomeActivity.this);
                    }
                    //左后再次更新app 的缓存 
                    SimplePreference.getPreference(WelcomeActivity.this).saveString(SpConstant.SP_DEVICES_ID, readDeviceID);
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }).start();
```

以上代码只是生成设备唯一标识符，同时保证app 在一次启动以后能够保持使用是统一个设备唯一标识符。

##### 2.2.3 注意

在 app 使用的时候只取 sharepreference 保存的内容，不要取sd 卡中保存的内容

#### 2.3 使用总结

以上方法只是能够最大限度的保持app 能够使用同一个设备唯一标识符。  在特殊情况下设备唯一标识符还是会发生变化的。  例如 用户没有给SD卡的读取权限， 那么app 在清楚数据以后再次生成的设备唯一标识符是有课能会发生变化的。



### 三、写在最后

以上是如何最大限度生成一个唯一标识符的思考，如有不正确之处请忽略，