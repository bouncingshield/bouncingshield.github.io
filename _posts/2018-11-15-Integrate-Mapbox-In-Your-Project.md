---
layout: post
title: How we integrate Google Maps or Mapbox Maps in your web, native mobile app or project.
---

If you need your web, Android app or iOS app integrate with a map, geo-location services,
extensible, custom maps, location search there are many solutions. For the developer side there are solutions like Google Maps, Mapbox, Open Street Maps and more.

In Bouncing Shield, our preferred choice is MapboxGL. MaboxGL provides an SDK for React Native, but also for web apps as does Google Maps. [You can view our article about integrating Google Maps in React Native here]( https://bouncingshield.github.io/React-Native-Airbnb-Google-Maps/)


In this article I'm going to explain how we integrate MapboxGL in you ReactNative project.

We are developing a custom bike lane apps for Barcelona city, BcnByBike. The client needed a native app that can show a map of the city of Barcelona, in that map we need to display the public city bike lanes. And the lanes come from the public Open Data Barcelona city information in a json format.

The easier way to add Mapbox SDK to your project is following the steps in the [GitHub repository](https://github.com/mapbox/react-native-mapbox-gl), but we have found the manual install a better option.

{% highlight bash %}
npm install @mapbox/react-native-mapbox-gl --save
{% endhighlight  %}

## Instructions for IOS

### Add Native Mapbox SDK Framework

In the Project navigator. Click General tab then add node_modules/@mapbox/react-native-mapbox-gl/ios/Mapbox.framework to Embedded Binaries.

![](https://camo.githubusercontent.com/a87a1d99b366d6b4049b240425f0569d62942465/68747470733a2f2f636c6475702e636f6d2f733455334a66535f2d6c2e706e67)

### Add React Native Mapbox SDK Files
In the Xcode's Project navigator, right click on the Libraries folder ➜ Add Files to <...>. Add 

{% highlight bash %}
node_modules/@mapbox/react-native-mapbox-gl/ios/RCTMGL.xcodeproj
{% endhighlight %}

Then in Xcode navigate to Build Phases click on it and you should see Link Binary with Libraries, we need to add libRCTMGL.a

![](https://cl.ly/31b926a2209b/Image%202018-11-15%20at%209.49.42%20PM.png)

### Add Framework Header Search Paths
In the Build Settings of your application target search for FRAMEWORK_SEARCH_PATHS.

Add non-recursive to your Framework Search Paths

{% highlight bash %}
$(PROJECT_DIR)/../node_modules/@mapbox/react-native-mapbox-gl/ios
{% endhighlight %}

![](https://cl.ly/c4879c5391bd/Image%202018-11-15%20at%209.51.14%20PM.png)

### Add Run Script
In the Build Phases tab, click the plus sign and then New Run Script Phase

![](https://camo.githubusercontent.com/e7b8e57110264c48c6af6622909afd43bcddd574/68747470733a2f2f636c6475702e636f6d2f6a677438705f64486a442e706e67)

### Add Mapbox Component to React Native app
First, you’ll import the components that you will need. This includes components from React, React Native, and Mapbox. To display a map you’ll need a [Mapbox access token](https://www.mapbox.com/help/define-access-token/). Mapbox uses access tokens to associate requests to API resources with your account. As soon as you have imported Mapbox, you should set your Mapbox access token using Mapbox.setAccessToken().

Add the import for the Component and configure your access token

{% highlight javascript %}
import Mapbox from '@mapbox/react-native-mapbox-gl';
Mapbox.setAccessToken('<your access token here>');
{% endhighlight  %}

Then add the code to include the component in a View element, remember that the container view has to have a style of flex: 1 for it to cover the complete screen

![](https://cl.ly/9d7231c82ebe/Image%202018-11-15%20at%2010.04.01%20PM.png){:width="300px" }

## Instructions for Android

We need to add some repositories in order to get our dependencies.

In project:build.gradle
![](https://cl.ly/90a693b0a452/Image%202018-11-15%20at%2010.08.05%20PM.png)

In app:build.gradle. Add project under dependencies
![](https://cl.ly/1ac5bc602b0f/Image%202018-11-15%20at%2010.10.35%20PM.png)

In settings.gradle
Include project, so gradle knows where to find the project

{% highlight java %}
include ':mapbox-react-native-mapbox-gl'
project(':mapbox-react-native-mapbox-gl').projectDir = new File(rootProject.projectDir, '../node_modules/@mapbox/react-native-mapbox-gl/android/rctmgl')
{% endhighlight  %}

In MainApplication.java
Add import com.mapbox.rctmgl.RCTMGLPackage; as an import statement and new RCTMGLPackage() in getPackages()

![](https://cl.ly/d7aeb11cfd5f/Image%202018-11-15%20at%2010.17.04%20PM.png)

## Possible errors

If you get an error like this: No static method toHumanReadableAscii
The solution is to edit the compile in build.gradle to this:

{% highlight java %}
compile (project(':mapbox-react-native-mapbox-gl')) {
  compile ('com.squareup.okhttp3:okhttp:3.6.0') {
    force = true
  }
}
{% endhighlight  %}

The Android app will look like this

![](https://cl.ly/5278a40adb9c/Image%202018-11-15%20at%2010.31.52%20PM.png){:width="300px" }


## Showing bike lanes on the map
import your line data into a variable 

{% highlight javascript %}
import biciLanes from 'carril_bici.js'
{% endhighlight  %}

Add the ShapeSource and LineLayer Mapbox Components

{% highlight javascript %}
<Mapbox.ShapeSource id='biciLanesSource' shape={biciLanes}>
  <Mapbox.LineLayer
    id='biciLanes'
    style={{
      lineWidth: 3,
      lineCap: Mapbox.LineCap.Round,
      lineJoin: Mapbox.LineJoin.Round,
      lineColor: '#19C144',
      lineOpacity: 1
    }} />
</Mapbox.ShapeSource>
{% endhighlight  %}

The complete code is like this:

![](https://cl.ly/9ff645764df5/Image%202018-11-15%20at%2010.41.01%20PM.png)

The Map View after this, is like this:

![](https://cl.ly/190ffe29b088/Image%202018-11-15%20at%2010.42.39%20PM.png ){:width="300px" }