---
layout: post
title: React Native y AirBnB Google Maps
---

En este post explicare como integrar Google Maps a tus aplicaciones React Native, ya que me he encontrado que la instalación por defecto genera muchos errores y react-native no termina de reconocer la librería de Google Maps.

Después de estar aproximadamente 1 semana intentando integrar la librería de AirBnB de Google Maps a una aplicación he llegado a una serie de pasos fáciles de seguir.

Creación del proyecto React Native e instalación de librería AirBnB Google Maps
Primero creamos nuestra app en React Native, nuestra app se llamará myMapApp.


{% highlight bash %}
react-native init myMapApp
cd myMapApp
{% endhighlight  %}

Ahora instalamos la librería para Google Maps de AirBnB.

{% highlight bash %}
npm install --save react-native-maps
{% endhighlight  %}

Enlazamos la librería con React Native

{% highlight bash %}
npm install
react-native link react-native-maps
{% endhighlight  %}

En teoría ya nuestra app debería funcionar con los mapas por defecto de cada plataforma Maps en iOS y Maps en Android. Pero si queremos usar Google Maps en ambas plataformas tendremos que hacer algunos pasos adicionales:

Antes de comenzar a trabajar en la app vamos a generar la clave de Api (Api Key) de Google Maps para que usemos en nuestro proyecto

Crear la clave API de Google Maps
Iremos a la consola de Google para desarrolladores

Crearemos un nuevo proyecto

![](https://cl.ly/0U0Z2w1m2e2r/download/Image%202017-10-11%20at%203.02.09%20PM.png)


Y agregaremos una api en nuestro caso Google Maps SDK for iOS y Google Maps SDK for Android

![](https://cl.ly/0g0N130h3p0E/download/Screen%20Recording%202017-10-11%20at%2003.07%20PM.gif)


Ahora nos iremos al menú de la izquierda en Credenciales. Y crearemos nuestra API Key, la guardaremos para mas adelante

![](https://cl.ly/2R1k2C1t0v0e/download/Image%202017-10-11%20at%203.48.00%20PM.png)

![](https://cl.ly/2M0d130z421h/download/Image%202017-10-11%20at%203.48.33%20PM.public.png)

Enlace de AirBnB Google Maps para iOS
Instalar Cocoapods (si lo tenemos podemos saltar este paso)

{% highlight javascript %}
sudo gem install cocoapods
{% endhighlight  %}

Configurar el proyecto para iOS

{% highlight javascript %}
cd ios
pod init
{% endhighlight  %}

Esto genera un archivo Podfile en nuestra, lo abriremos y editaremos su contenido

{% highlight javascript %}
# Uncomment the next line to define a global platform for your project
platform :ios, '9.0'

target 'myMapApp' do
  # Uncomment the next line if you're using Swift or would like to use dynamic frameworks
  # use_frameworks!

  # Pods for myMapApp

  target 'myMapApp-tvOSTests' do
    inherit! :search_paths
    # Pods for testing
  end

  target 'myMapAppTests' do
    inherit! :search_paths
    # Pods for testing
  end

  react_native_path = "../node_modules/react-native"
  pod "yoga", :path => "#{react_native_path}/ReactCommon/yoga"
  pod "React", :path => react_native_path

  pod 'GoogleMaps'

end
{% endhighlight  %}

Recuerda reemplazar el nombre de tu aplicación donde dice target

Y no te olvides de especificar la plataforma de iOS que utilizaras

Si recibes este error:

{% highlight javascript %}
[!] The name of the given podspec `yoga` doesn't match the expected one `Yoga`
{% endhighlight  %}

Cambia la linea de este modo (yoga en minúscula):

{% highlight javascript %}
pod "yoga", :path => "#{react_native_path}/ReactCommon/yoga"
{% endhighlight  %}

Ahora en la terminal

{% highlight javascript %}
pod install
{% endhighlight  %}

Al hacer esto se generara un nuevo proyecto en nuestra carpeta de iOS, lo abriremos con XCode

{% highlight javascript %}
open myMapApp.xcworkspace
{% endhighlight  %}

Y copiamos la carpeta AirGoogleMaps que se encuentra dentro de /node_modules/react-native-maps/lib/ios al proyecto xcworkspace que tenemos abierto en el Xcode.

![](https://cl.ly/3q3c1K0y082U/download/Screen%20Recording%202017-10-11%20at%2002.58%20PM.gif)


Quedando de esta forma

![](https://cl.ly/0Y0m2p2n0H1F/download/Image%202017-10-11%20at%202.58.29%20PM.png)


Para usar las credenciales API abrimos el archivo AppDelegate.m en Xcode.

{% highlight javascript %}
#import "AppDelegate.h"

#import <React/RCTBundleURLProvider.h>
#import <React/RCTRootView.h>
@import GoogleMaps;
@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  NSURL *jsCodeLocation;
  [GMSServices provideAPIKey:@"YOUR_GOOGLE_API_KEY"];
  jsCodeLocation = [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@"index" fallbackResource:nil];

  RCTRootView *rootView = [[RCTRootView alloc] initWithBundleURL:jsCodeLocation
                                                      moduleName:@"myMapApp"
                                              initialProperties:nil
                                                  launchOptions:launchOptions];
  rootView.backgroundColor = [[UIColor alloc] initWithRed:1.0f green:1.0f blue:1.0f alpha:1];

  self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
  UIViewController *rootViewController = [UIViewController new];
  rootViewController.view = rootView;
  self.window.rootViewController = rootViewController;
  [self.window makeKeyAndVisible];
  return YES;
}

@end
{% endhighlight  %}

Ahora iremos a Product -> Clean y luego Product -> Build para ejecutar nuestra aplicación.

![](https://cl.ly/2U0R213L341Z/download/Screen%20Recording%202017-10-11%20at%2003.55%20PM.gif)

Enlace de AirBnB Google Maps para Android
En nuestro proyecto debemos editar el archivo android/app/build.gradle

{% highlight javascript %}
dependencies {
  ...
  // Paste these line
  compile(project(':react-native-maps')){
      exclude group: 'com.google.android.gms', module: 'play-services-base'
      exclude group: 'com.google.android.gms', module: 'play-services-maps'
  }
  compile 'com.google.android.gms:play-services-base:10.0.1'
  compile 'com.google.android.gms:play-services-maps:10.0.1'
}
Y también debemos agregar nuestra Api Key en Android/app/src/AndroidManifest.xml

<application>
    <!-- You will only need to add this meta-data tag, but make sure it's a child of application -->
    <meta-data
      android:name="com.google.android.geo.API_KEY"
      android:value="YOUR GOOGLE MAPS API KEY HERE"/>
</application>
{% endhighlight  %}

Con esto ya tenemos nuestro proyecto React Native configurado en iOS y Android con Google Maps, ahora actualizamos el archivo App.js en nuestro proyecto React Native para incluir el Mapa

{% highlight javascript %}
import React, { Component } from 'react';
import {
  StyleSheet,
  Text,
  View
} from 'react-native';
import MapView, {PROVIDER_GOOGLE} from 'react-native-maps';

const styles = StyleSheet.create({
  container: {
    ...StyleSheet.absoluteFillObject,
    height: 400,
    width: 400,
    justifyContent: 'flex-end',
    alignItems: 'center',
  },
  map: {
    ...StyleSheet.absoluteFillObject,
  },
});

export default class App extends Component {
  render() {
    const { region } = this.props;
    console.log(region);

    return (
      <View style ={styles.container}>
        <MapView
          provider={PROVIDER_GOOGLE}
          style={styles.map}
          region={{ latitude: 37.78825, longitude: -122.4324, latitudeDelta: 0.015, longitudeDelta: 0.0121 }}
        >
        </MapView>
      </View>
    )
  }
}
{% endhighlight  %}