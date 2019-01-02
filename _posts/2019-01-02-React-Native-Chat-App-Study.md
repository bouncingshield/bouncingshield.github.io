---
layout: post
title: React Native Chat App Study
---

This is a chat app study, In here we try to study an implementation of a chat app using Firebase auth and Pusher ChatKit.

![](https://cl.ly/8252d3e128cc/Image%202018-12-13%20at%201.09.13%20PM.png){:width="300px" }

## Create the base React Native app

{% highlight javascript %}
	react-native init BouncingChatApp
{% endhighlight %}

## Create basic login screen

{% highlight javascript %}
  import React, { Component } from 'react'
  import { 
    StyleSheet,
    KeyboardAvoidingView,
    View,
    Text,
    TextInput,
    TouchableOpacity
  } from 'react-native'

  export default class LoginScreen extends Component {
    constructor(props){
      super(props)

      this.state = {
        email: null,
        password: null
      }
    }

    updateFields(field, value){
      this.setState({[field]: value})
    }

    onButtonPress(){
    // auth with firebase
    }

    render() {
      return (
        <KeyboardAvoidingView style={styles.container} behavior="padding" enabled>
          <View style={styles.container}>
            <Text style={styles.text}>Login Form</Text>

            <View>
              <Text style={styles.text}>Enter your email</Text>
              <TextInput style = {styles.input} 
                  autoCapitalize="none"
                  onChangeText={(e) => this.updateFields('email', e)}
                  autoCorrect={false}
                  keyboardType='email-address'
                  placeholder='Enter your email'/>
            </View>
            <View>
              <Text style={styles.text}>Enter your password</Text>
              <TextInput style = {styles.input}
                  onChangeText={(e) => this.updateFields('password', e)}
                  autoCapitalize="none"
                  autoCorrect={false}
                  placeholder='Enter your password'/>
            </View>

            <TouchableOpacity 
              style={styles.buttonContainer} 
              onPress={this.onButtonPress}>
               <Text style={styles.buttonText}>LOGIN</Text>
            </TouchableOpacity>
          </View>
        </KeyboardAvoidingView>
      )
    }
  }

  const styles = StyleSheet.create({
    container: {
      flex: 1,
      justifyContent: 'center',
      alignItems: 'center',
      backgroundColor: '#F5FCFF',
    },
    text: {
      fontSize: 18,
      paddingVertical: 10
    },
    input: {
      width: 200,
      height: 40,
      backgroundColor: 'rgba(225,225,225,0.2)',
      borderWidth: 1,
      borderColor: 'rgba(225,225,225,0.9)',
      marginBottom: 10,
      padding: 10
    },
    buttonContainer: {
      backgroundColor: '#2980b6',
      paddingVertical: 15,
      paddingHorizontal: 15
    },
    buttonText: {
      color: '#fff',
      textAlign: 'center',
     fontWeight: '700'
    }
  });
{% endhighlight %}

## Integrate firebase and add auth login with firebase (IOs Only)

{% highlight javascript %}
	npm install --save react-native-firebase
{% endhighlight %}

Follow our integration guide here [https://bouncingshield.github.io/React-Native-Firebase-Integration/](https://bouncingshield.github.io/React-Native-Firebase-Integration/)


## Create basic chat screen

{% highlight javascript %}
  import React, { Component } from 'react'
  import { 
    StyleSheet,
    View,
    Text,
  } from 'react-native'

  export default class ChatScreen extends Component {

    render() {
      return (
        <View style={styles.container}>
          <Text>Chat SCreen</Text>
        </View>
      )
    }
  }

  const styles = StyleSheet.create({
    container: {
      flex: 1,
      justifyContent: 'center',
      alignItems: 'center',
      backgroundColor: '#F5FCFF',
    }
  });
{% endhighlight %}

## Create Basic auth methods

{% highlight javascript %}
  import React, { Component } from 'react'
  import { 
    StyleSheet,
    KeyboardAvoidingView,
    View,
    Text,
    TextInput,
    TouchableOpacity
  } from 'react-native'

  import firebase from 'react-native-firebase';

  export default class LoginScreen extends Component {
    constructor(props){
      super(props)

      this.state = {
        email: '',
        password: '',
        error: null
      }

      this.onButtonPress = this.onButtonPress.bind(this)
    }

    updateFields(field, value){
      this.setState({[field]: value})
    }

    onButtonPress(){
      if (this.state.email && this.state.password) {
        firebase.auth().signInWithEmailAndPassword(
         this.state.email, this.state.password
        ).then(() => {
          this.setState({ isAuthenticated: true });
        }).catch((error) => {
          this.setState({error: error.message})
        });
      } else {
        this.setState({error: 'Please add the email address and password'})
      }
    }

    render() {
      //firebase.auth().signOut()
      return (
        <KeyboardAvoidingView style={styles.container} behavior="padding" enabled>
          <View style={styles.container}>
            <Text style={styles.text}>Login Form</Text>

            <View>
              <Text style={styles.text}>Enter your email</Text>
              <TextInput style = {styles.input} 
                  autoCapitalize="none"
                  onChangeText={(e) => this.updateFields('email', e)}
                  autoCorrect={false}
                  keyboardType='email-address'
                  placeholder='Enter your email'/>
            </View>
            <View>
              <Text style={styles.text}>Enter your password</Text>
              <TextInput style = {styles.input}
                  onChangeText={(e) => this.updateFields('password', e)}
                  autoCapitalize="none"
                  autoCorrect={false}
                  placeholder='Enter your password'/>
            </View>

            <View>
              <Text style={styles.error}>{this.state.error && (this.state.error) }</Text>
            </View>

            <TouchableOpacity 
              style={styles.buttonContainer} 
              onPress={this.onButtonPress}>
               <Text style={styles.buttonText}>LOGIN</Text>
            </TouchableOpacity>
          </View>
        </KeyboardAvoidingView>
      )
    }
  }

  const styles = StyleSheet.create({
    container: {
      flex: 1,
      justifyContent: 'center',
      alignItems: 'center',
      backgroundColor: '#F5FCFF',
    },
    text: {
      fontSize: 18,
      paddingVertical: 10
    },
    input: {
      width: 200,
      height: 40,
      backgroundColor: 'rgba(225,225,225,0.2)',
      borderWidth: 1,
      borderColor: 'rgba(225,225,225,0.9)',
      marginBottom: 10,
      padding: 10
    },
    buttonContainer: {
      backgroundColor: '#2980b6',
      paddingVertical: 15,
      paddingHorizontal: 15
    },
    buttonText: {
      color: '#fff',
      textAlign: 'center',
      fontWeight: '700'
    },
    error: {
      color: 'red',
      paddingVertical: 15
    }
  });
{% endhighlight %}


## Add navigation dependency

{% highlight javascript %}
  npm install react-navigation
  npm install --save react-native-gesture-handler
{% endhighlight %}


## Add navigation config

Follow the guide here https://reactnavigation.org/docs/en/app-containers.html

## Show basic list of messages

{% highlight javascript %}
  import React, { Component } from 'react'
  import { Dimensions, StyleSheet, View, Text, FlatList } from 'react-native'

  const { width, height } = Dimensions.get("window")

  const DUMMY_DATA = [{
    senderId: 'Os',
    text: 'Hey, how is it going?'
  }, {
    senderId: 'Jane',
    text: 'Great! How about you?'
  }, {
    senderId: 'Os',
    text: 'Good to hear! I am great as well'
  }]

  export default class ChatMessageList extends Component {
    render() {
      return(
        <View style={styles.messagesContainer}>
          <FlatList
            data={DUMMY_DATA}
            renderItem={({item, index}) => (
              <View key={index} style={styles.message}>
                <Text style={styles.messageUsername}>{item.senderId}:</Text>
                <View style={styles.messageTextBox}>
                  <Text style={styles.messageText}>{item.text}</Text>
                </View>
              </View>
            )}
          />
        </View>
      )
    }
  }

  const styles = StyleSheet.create({
    container: {
      flex: 1,
      backgroundColor: '#F5FCFF',
    },
    messagesContainer: { 
      flex: 1,
      height: height - 200,
      paddingHorizontal: 10
    },
    message: {
      marginVertical: 15
    },
    messageUsername: {
      fontSize: 16,
      color: '#3e5869',
      marginBottom: 6
    },
    messageTextBox: {
      backgroundColor: '#5ea3d0',
      paddingVertical: 6,
      paddingHorizontal: 8,
      borderRadius: 10
    }, 
    messageText: {
      color: '#ffffff',
      fontSize: 18,
    }
  });
{% endhighlight %}

## Show list of messages in ChatKit test mode

{% highlight javascript %}
  import React, { Component } from 'react'
  import { Dimensions, StyleSheet, View, Text, FlatList } from 'react-native'
  import { ChatManager, TokenProvider } from '@pusher/chatkit-client'

  const { width, height } = Dimensions.get("window")

  export default class ChatMessageList extends Component {
    constructor(props){
      super(props)

      this.state = {
        currentUser: null,
        roomId: 'YOUR ROOM ID',
        messages: []
      }

      this.getChatKitUser()
    }

    getChatKitUser(){
      // Config ChatKit
      const chatManager = new ChatManager({
        instanceLocator: 'YPUR INSTANCE LOCATOR',
        userId: 'YOUR USER ID',
        tokenProvider: new TokenProvider({ url: 'YOUT TEST TOKEN PROVIDER' })
      })

      // Connect to Room
      chatManager.connect()
      .then(currentUser => {
        currentUser.subscribeToRoom({ roomId: this.state.roomId })
        this.setState({ currentUser: currentUser }, this.getMessages)
      })
    }

    getMessages(){
      let { currentUser } = this.state

      currentUser.fetchMessages({
        roomId: this.state.roomId,
        direction: 'newer',
        limit: 10,
      }).then(messages => {
        this.setState({ messages })
        console.log(messages)
      }).catch(err => {
        console.log(`Error fetching messages: ${err}`)
      })
    }

    render() {
      return(
        <View style={styles.messagesContainer}>
          <FlatList
            data={this.state.messages}
            renderItem={({item, index}) => (
              <View key={index} style={styles.message}>
                <Text style={styles.messageUsername}>{item.senderId}:</Text>
                <View style={styles.messageTextBox}>
                  <Text style={styles.messageText}>{item.text}</Text>
                </View>
              </View>
            )}
          />
        </View>
      )
    }
  }

  const styles = StyleSheet.create({
    container: {
      flex: 1,
      backgroundColor: '#F5FCFF',
    },
    messagesContainer: { 
      flex: 1,
      height: height - 200,
      paddingHorizontal: 10
    },
    message: {
      marginVertical: 15
    },
    messageUsername: {
      fontSize: 16,
      color: '#3e5869',
      marginBottom: 6
    },
    messageTextBox: {
      backgroundColor: '#5ea3d0',
      paddingVertical: 6,
      paddingHorizontal: 8,
      borderRadius: 10
    }, 
    messageText: {
      color: '#ffffff',
      fontSize: 18,
    }
  });
{% endhighlight %}

## Create a server to use

To create a user, we need a server that does the request to chatkit, we can't do it from the client.
I am going to use chatkit-server-ruby and sinatra https://github.com/pusher/chatkit-server-ruby
http://sinatrarb.com/

{% highlight javascript %}
  require 'sinatra'
  require 'sinatra/contrib'
  require "sinatra/reloader" if development?

  # Chatkit
  require 'chatkit'
  chatkit = Chatkit::Client.new({
    instance_locator: YOUR_INSTACE_LOCATOR,
    key: YOUR_KEY,
  })

  get '/' do
    "hello world"
  end

  get '/create_user/:user_name' do
    begin
      response = chatkit.create_user({ id: params['user_name'], name: params['user_name'] })
      logger.info response.inspect
      return response['body']
    rescue => e
      logger.info e.inspect
      return "There was an error with the user creation"
    end
  end

  get '/get_user/:user_name' do
    begin
      response = chatkit.get_user({ id: params['user_name'] })
      logger.info response.inspect
      return response['body']
    rescue => e
      logger.info e.inspect
      return "There was an error with getting the user"
    end
  end
{% endhighlight %}

To run the server 
{% highlight javascript %}
	foreman start
{% endhighlight %}


## Create a new user

{% highlight javascript %}
  getChatKitUser(){
    fetch('http://localhost:5000/get_user/'+this.state.userName)
      .then((response) => response.json())
      .then((responseJson) => {
        if (responseJson.name) {
          //connect to room
          console.log('connect to room after getChatKitUser')
        }
      }).catch((error)=> {
        // create ChatKit user
        console.log('create chatkit user')
        this.createChatKitUser()
      })
  }

  createChatKitUser(){
    fetch('http://localhost:5000/create_user/'+this.state.userName)
    .then((response) => response.json())
    .then((responseJson) => {
      if (responseJson.name) {
        //connect to room
        console.log('connect to room after createChatKitUser')
      }
    }).catch((error)=> {
      console.log('error', error)
    })
  }
{% endhighlight %}

## Connect to a room

{% highlight javascript %}
  connectToRoom(roomId = this.state.roomId) {
    console.log('lets connect to a room')
    fetch('http://localhost:5000/connect_user_to_room/'+ this.state.userName +'/' + roomId)
      .then((response) => response.json())
      .then((responseJson) => {
        console.log('responseJson', responseJson)
        // lets get the messages
      }).catch((error)=> {
        console.log('error', error)
      })
  }
{% endhighlight %}

{% highlight javascript %}
  get '/connect_user_to_room/:user_id/:room_id' do
    begin
      response = chatkit.add_users_to_room({
        room_id: params[:room_id],
        user_ids: [params[:user_id]]
      })
      logger.info response.inspect
      return response[:body].to_json
    rescuee
      logger.info e.inspect
      return "There was an error with adding the user to the room"
    end
  end
{% endhighlight %}

## Get messages

{% highlight javascript %}
  getMessages(roomId = this.state.roomId) {
    fetch('http://localhost:5000/get_messages/' + roomId)
      .then((response) => response.json())
      .then((responseJson) => {
        this.setState({messages: responseJson.reverse(), ready: true})
      }).catch((error)=> {
        console.log('getMessages error', error)
      })
  }
{% endhighlight %}

{% highlight javascript %}
  get '/get_messages/:room_id' do
    begin
      response = chatkit.get_room_messages({
        room_id: params[:room_id],
        limit: 10,
      })
      logger.info response.inspect
      return response[:body].to_json
    rescuee
      logger.info e.inspect
      return "There was an error with getting the room messages"
    end
  end
{% endhighlight %}

## Send messages

{% highlight javascript %}
  onMessageSend(){
      console.log('onMessageSend')
      let message = this.state.message
       fetch('http://localhost:5000/send_message/' + this.state.roomId + '/' + this.state.userName + '/' + message)
        .then((response) => response.json())
        .then((responseJson) => {
          this.getMessages()
          this.setState({message: null})
        }).catch((error)=> {
          console.log('onMessageSend error', error)
        })
    }
{% endhighlight %}

![Image%202018-12-13%20at%201.09.13%20PM.png](https://cdn.filestackcontent.com/ssoDGV5eT7y2GZKY94Vo){:width="300px" }


Need help with a project? I'm open for freelance jobs. http://bouncingshield.com
