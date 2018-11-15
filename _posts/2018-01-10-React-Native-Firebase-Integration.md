---
layout: post
title: Integración React Native Firebase 
---

En esta serie de artículos vamos a explorar la integración de Firebase a React Native. Veremos como trabajar con diversos módulos, por ejemplo, Analytics, Autenticación, DataBase, etc.

En Bouncing Shield hacemos uso constante de esta genial plataforma, Firebase nos permite escalar nuestras aplicaciones de manera rápida y eficiente, con el tiempo que ahorramos usando Firebase podemos dedicarnos a idear y aplicar nuevas funcionalidades a nuestros clientes.

En este tutorial veremos como integrar el modulo principal de RNFirebase a nuestra nueva aplicación. Y comenzaremos a utilizar uno de los módulos mas utilizados de Firebase, el módulo de Analytics. Con este módulo podremos tener un log de eventos que suceden en nuestra aplicación; Analytics nos permite guardar eventos como un login de usuario, cuántas personas ven cierta pantalla de nuestra aplicación, cuántas dan click a un botón, etc.

Tener Analytics en nuestra aplicación sera muy importante a lo largo de la vida del producto, pues podremos iterar y hacer cambios en nuestra plataforma y sabremos el impacto que estos cambios tienen en el tiempo.

Puedes ver toda la documentación de RNFirebase aquí: https://rnFirebase.io

# Nuestra aplicación

Empezaremos por crear nuestra aplicación react-native base. Por ahora nuestra aplicación sera muy simple y sólo mostrará un botón, al presionar este botón crearemos un evento que llamaremos 'onPressComprar' y ese evento lo veremos reflejado en nuestra consola de Firebase.

He creado una aplicación base con:

{% highlight bash %}
react-native init rn_firebase_analytics_aplication
{% endhighlight  %}



# Creando nuestra aplicación en la consola de Firebase

Para enviar los eventos de nuestra aplicación a la consola de Firebase, debemos tener un proyecto creado allí, para ello iremos a la pagina web de Firebase [https://console.Firebase.google.com](https://console.Firebase.google.com) y crearemos nuestro proyecto:

Haciendo click en Añadir Proyecto, y llenaremos los datos que nos solicitan:

![](https://cl.ly/2g2Y3u0u0G3p/download/Image%202018-01-07%20at%202.41.21%20PM.png)

Ahora que tenemos el proyecto creado en la consola de Firebase, vamos a instalar el plugin de RNFirebase en nuestra aplicación.

En el siguiente paso, instalaremos el plugin RNFirebase en IOS. Puedes encontrar toda la documentación oficial en este enlace: [https://rnFirebase.io/docs/v3.2.x/installation/ios](https://rnFirebase.io/docs/v3.2.x/installation/ios).

# Instalando RNFirebase en IOS

Iremos a nuestro terminal y ejecutaremos el comando de instalación del plugin:

{% highlight bash %}
npm install --save react-native-Firebase
{% endhighlight  %}

Para que nuestra aplicación pueda conectarse con la consola de Firebase necesitamos un archivo que indique los valores asociados a nuestra cuenta. Ese archivo es el `GoogleService-Info.plist`. Este archivo lo podemos generar desde la web de la consola de Firebase: 

![](https://cl.ly/0A1V0a1H0X1U/download/Image%202018-01-08%20at%206.40.53%20PM.png)

Seguiremos los pasos y descargaremos el archivo `GoogleService-Info.plist`, el cual guardaremos en la carpeta ios de nuestro proyecto asi:

![](https://cl.ly/0T2H00221k0g/download/Image%202018-01-08%20at%206.43.05%20PM.png)

Ahora agregaremos el SDK de Firebase a nuestra aplicación:
Necesitamos tener instalado `CocoaPods`; Para ello ejecutaremos lo siguiente en nuestro terminal desde la carpeta ios de nuestra aplicación:

{% highlight bash %}
cd ios
sudo gem install cocoapods
pod init
{% endhighlight  %}

Abre el archivo Podfile y añade lo siguiente:
{% highlight bash %}
pod 'Firebase/Core'
{% endhighlight  %}

Guarda el archivo.

![](https://cl.ly/2y2N0f1u0H1j/download/Image%202018-01-08%20at%206.50.51%20PM.png)

Y ejecuta lo siguiente:
{% highlight bash %}
pod install
{% endhighlight  %}

Veras que se instalan algunos módulos de RNFirebase, además
esto creara un archivo `.xcworkspace` para la aplicación. Usa el archivo para las futuras tareas de desarrollo de tu aplicación.

Ahora iniciamos el plugin en nuestra aplicación, en el archivo `ios/[tu_aplicación]/aplicaciónDelegate.m`:

En las primeras lineas del archivo importamos la librería:

{% highlight c %}
#import <Firebase.h>
{% endhighlight  %}

Al comienzo del método `didFinishLaunchingWithOptions:(NSDictionary *)launchOptions`

{% highlight c %}
[FIRaplicación configure];
{% endhighlight  %}

Quedando de este modo:

![](https://cl.ly/2F173E0h2w2U/download/Image%202018-01-08%20at%206.55.44%20PM.png)

Y eso es todo, puedes encontrar la documentación de la instalación en la pagina oficial de React Native Firebase [https://rnFirebase.io/docs/v3.2.x/installation/ios](https://rnFirebase.io/docs/v3.2.x/installation/ios).

Para android: [https://rnFirebase.io/docs/v3.2.x/installation/android](https://rnFirebase.io/docs/v3.2.x/installation/android)

# Agregando el módulo de Analytics

El módulo de FirebaseAnalytics se instala por defecto al hacer la instalación del Core Plugin, asi que ya podremos usar la librería sin agregar nada mas.

![](https://cl.ly/1v2S1z0T1111/download/Image%202018-01-08%20at%206.59.17%20PM.png)

Puedes ver toda la documentación del módulo en este enlace: [https://rnFirebase.io/docs/v3.2.x/Analytics/reference/Analytics](https://rnFirebase.io/docs/v3.2.x/Analytics/reference/Analytics)

# ¿Qué queremos lograr?

Esta será una aplicación sencilla y básicamente queremos tener información de dos eventos puntuales:
- Queremos saber cuando se cargue la primera pantalla y,
- Queremos saber cuando se presiona un botón de nuestra pantalla.

Esta sera la pantalla de nuestra aplicación:

![](https://cl.ly/2i3a2u3J0g2f/download/Image%202018-01-08%20at%207.19.28%20PM.png){:width="300px" }

{% highlight javascript %}
import React, { Component } from 'react';
import {
  Platform,
  StyleSheet,
  Text,
  View,
  Button
} from 'react-native';
import firebase from 'react-native-firebase';
const firebaseAnalytics = firebase.analytics();

export default class App extends Component<{}> {
  onPressComprar(){
    console.log('Boton comprar presionado');
  };

  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.welcome}>
          React Native Firebase!
        </Text>
        <Button
          onPress={this.onPressComprar}
          title="Cromprar"
          color="#841584"
        />
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
  }
});
{% endhighlight  %}

[https://gist.github.com/uokesita/394287f1df95787c4c81d4d39f7dad3a](https://gist.github.com/uokesita/394287f1df95787c4c81d4d39f7dad3a)



Importaremos Firebase y Firebase.Analytics en nuestra aplicación y usaremos el método `componentDidMount` para enviar nuestro primer evento a la consola de Firebase.

{% highlight bash %}
import Firebase from 'react-native-Firebase';
const FirebaseAnalytics = Firebase.Analytics();
{% endhighlight  %}

# LogEvent(name, params)

Para resolver nuestro primer punto usaremos el método logEvent de Firebase en el evento de carga de nuestra pantalla:

{% highlight javascript %}
  componentDidMount(){
    FirebaseAnalytics.logEvent('pantalla_principal_cargada')
  }
{% endhighlight  %}

Si corremos nuestra aplicación, y vamos a la consola de Firebase veremos que no se ha guardado ningún evento, esto es porque debemos habilitar el `DebugView` en el simulador IOS para ver los eventos en tiempo real.

![](https://cl.ly/0q0O1K0h313n/download/Image%202018-01-09%20at%204.34.08%20PM.png)

Esto lo hacemos desde xCode, en el menu Product -> Scheme -> Edit Scheme... (abajo del todo) -> `Arguments Tab -> y agregamos '-FIRDebugEnabled'

![](https://cl.ly/0D3S2H141q0l/download/Image%202018-01-09%20at%204.36.08%20PM.png)

Y ahora si ejecutamos nuestra aplicación podrás ver los eventos de tu simulador en tiempo real.

![](https://cl.ly/0K3b1n292G2i/download/Image%202018-01-09%20at%204.37.30%20PM.png)

Recuerda que puedes pasar parámetros a la función `logEvent` y estos pueden ser un objeto cualquiera u otro tipo de dato que necesites.

Y con esta función podemos pasar parámetro a nuestro log del evento al presionar el botón. asi:

{% highlight javascript %}
  onPressComprar(){
    FirebaseAnalytics.logEvent('onPressComprar', {productId: 123})
  };
{% endhighlight  %}

Firebase Posee unos eventos por defecto que puedes verificar para tener mas detalles de tus aplicaciones [https://rnFirebase.io/docs/v3.2.x/Analytics/reserved-events](https://rnFirebase.io/docs/v3.2.x/Analytics/reserved-events).

# setAnalyticsCollectionEnable

Firebase por defecto obtiene y analiza varios eventos para IOS y Android por igual, si necesitas desactivar alguno de estos eventos por privacidad de tus usuarios, puedes deshabilitarlos.

# setCurrentScreen
Firebase hace un seguimiento de las transiciones de pantalla y adjunta información sobre la pantalla actual a los eventos, lo que te permite hacer un seguimiento de métricas como la participación de los usuarios o el comportamiento de los usuarios por pantalla. Gran parte de esta recopilación de datos ocurre de forma automática, pero también puedes hacer un seguimiento manual de los nombres de las pantallas.

# setMinimumSessionDuration y setSessionTimeoutDuration
Firebase permite hacer un seguimiento de tus usuarios via sesiones en la aplicación, estas sesiones tienen por defecto un periodo de tiempo mínimo, por ejemplo si tu usuario abre tu aplicación, la usa un poco y luego va a la aplicación de mensajes, envía un mensaje y regresa a tu aplicación, eso es considerado una sesión, hay un mínimo de 10 segundos para considerarse una session y un máximo de 30 minutos por defecto, pero con este método puedes cambiar esa duración si lo requieres.

# setUserId y setUserProperty
Estos métodos son usados por los desarrolladores por ejemplo cuando tienes distintas aplicaciones, plataformas o servicios y necesitas que el usuario tenga el mismo id en todas ellas. Recuerda que este userId tiene que seguir las políticas de privacidad de google [https://www.google.com/policies/privacy/](https://www.google.com/policies/privacy).


En futuros artículos veremos como integrar y usar otros módulos de RNFirebase, puedes contactarnos si requieres más información.
