+++
date = "2016-08-18T20:54:34+02:00"
categories = [ "Development" ]
tags = [ "Development", "Tutorial", "Mobile", "React", "React Native" ]
series = [ "React Native"]
title = "Creating a mobile application with React Native - part 2"
draft = true
+++

This article is the second in a series started with [Creating a mobile application with React Native - part 1]({{< ref "post/react-native-1.md" >}}).

Today, we will enrich our basic app with a list of albums provided by the deezer API. We will use [Native Base](http://nativebase.io), which is a complement to React Native. It provides higher level, polished components and layouts, for both iOS and Android.
<!--more-->

With Native Base you have much less boilerplate code to write than with React Native alone. At first sight, it looks like a kind of [Twitter Bootstrap](http://getbootstrap.com/) for mobile development with React Native.

## Install Native Base



First thing first, we download NativeBase and add it to the dependencies in package.json:
``` bash
cd AwesomeNativeBase
npm install native-base --save
```

Then we include the icon fonts provided by React Native into the app:
```
mkdir android/app/src/main/assets/fonts
cp node_modules/react-native-vector-icons/Fonts/*.ttf android/app/src/main/assets/fonts/
```

Edit `android/settings.gradle` and add the following lines:

```
include ':react-native-vector-icons'
project(':react-native-vector-icons').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-vector-icons/android')
```


Edit `android/app/build.gradle` and add the following line to the `dependencies` node:

```
compile project(':react-native-vector-icons')
```

Finally build the app.

```
npm run build
```

## Create a Native Base GUI

Replace the content of `index.android.js with:

``` html
import React, { Component } from 'react';
import { AppRegistry } from 'react-native';
import { Content, Container, Header, Text, Title } from 'native-base';

class AwesomeNativeBase extends Component {
    render() {
        return (
            <Container>
                <Header>
                    <Title>Albums</Title>
                </Header>

                <Content>
                  <Text>
                    Hello World!
                  </Text>
                </Content>
            </Container>
        );
    }
}

AppRegistry.registerComponent('AwesomeNativeBase', () => AwesomeNativeBase);
```
The import directives list all the React, React Native and NativeBase components used.
Several React Native basic components are replaced by their Native Base counterpart in the render function.

You should be able to run the app and see the Hello World! message under the header.
Refer to the [first part]({{< ref "post/react-native-1.md" >}}) if you need some explanation to run it.


{{< img src="/post/img/react-native-2/screenshot_1.png" >}}


## Create the AlbumList component

Now let's create a file named `albumlist.js`, at the same level than `index.android.js`. It will contain the description for a basic list component:

``` javascript
import React, { Component } from 'react';
import { ListItem, List, Text } from 'native-base';

export default class AlbumList extends Component {

  constructor(props) {
    super(props);
    this.state = {
        albums: ["foo", "bar", "baz"]
    }
  }

  render() {
    return(
      <List dataArray={this.state.albums}
            renderRow={(item) =>
                <ListItem>
                    <Text>{item}</Text>
                </ListItem>
            }>
      </List>
    );
  }
}
```
The constructor is used to define the states of the component. At this stage, the list of albums is just a list of strings. The Native Base List component iterates on these strings and render a ListItem for each of them.

Then change the content of `index.android.js`, to include the AlbumList component:

{{< highlight html "linenos=inline" >}}
import React, { Component } from 'react';
import { AppRegistry } from 'react-native';
import { Button, Content, Container, Footer, Header, Icon, Title } from 'native-base';

import AlbumList from './albumlist';

class AwesomeNativeBase extends Component {

    render() {
        return (
            <Container>
                <Header>
                   <Title>Albums</Title>
                </Header>
                <Content>
                  <AlbumList />
                </Content>
            </Container>
        );
    }
}

AppRegistry.registerComponent('AwesomeNativeBase', () => AwesomeNativeBase);
{{< / highlight >}}

We imported the component (line 5), and replaced the content of the main container (line 16).
If you reload the app, it should look like this:

{{< img src="/post/img/react-native-2/screenshot_2.png" >}}

## Install Superagent

[Fetch](https://facebook.github.io/react-native/docs/network.html) is the default API in React Native for making HTTP request. It's still in experimental status, and I had an annoying bug with it: the request was not executed unless I tap the device screen. So I replaced it with a more mature library, [Superagent](http://visionmedia.github.io/superagent/).

Integrating Superagent in a ReactNative app is dead simple. First you download it and add it to the dependencies of the project:

```
npm install superagent --save
```

Then you add this line to albumlist.js, just after the imports:

``` javascript
var superagent = require('superagent');
```

That's all! Now you are ready to make HTTP requests.

Now replace the constructor into `albumlist.js` by these three functions:

``` javascript
    constructor(props) {
      super(props);
      this.state = {
          loading: false,
          albums: null
      }
    }

    componentDidMount() {
      this.load();
    }

    load() {
      superagent.get('http://api.deezer.com/album/' + this.props.albumId + '/tracks')
        .set('Accept', 'application/json')
        .end((err, response) => {
            if (response.ok) {
              this.setState({
                  loading: false,
                  tracks: response.body.data
              });
            }
            else {
              this.setState({
                  loading: false
              });
            }
        })
    }
```
This will automatically make a request to the Deezer API in order to load a list of albums for the artist with id 27 (Daft Punk). The result of the request is then put in the "albums" state.

During the request execution, the "loading" state is set to true to display the spinner.

Note how we use ES6 arrow notation for the request callback (the parameter of the `end` function). It allows us to use `this` in the callback. We do not need to write something like `var self=this;` and then using `self` instead of `this`.


Now replace the render function with the following code:

``` javascript
    render() {
        if(this.state.loading) {
          return (
            <Spinner color="#440099"/>
          );
        }
        else {
          return(
            <List dataArray={this.state.albums}
                  renderRow={(item) =>
                      <ListItem>
                          <Thumbnail square size={60} source={{uri: item.cover_small}} />
                          <Text>{item.title}</Text>
                      </ListItem>
                  }>
            </List>
          );
        }
    }
```

This new implementation displays a spinner during the request execution. After the response is received, it hides the spinner and replace it with a list of albums, represented by their cover and title.

{{< img src="/post/img/react-native-2/screenshot_3.png" >}}

The source code for the app is available [on Github](https://github.com/PascalLeMerrer/react-native-tuto/tree/step2).

## Conclusion

At this stage my feelings about React Native are mixed:

* it produces very easily a working application, and until now I did not require any knowledge about Android development (although it won't prevent me to learn Google's guidelines to create an app with a good UX);
* the Fetch API seems buggy;
* the error messages of the compiler often left me without any clue about what is causing them;
* sometimes the error messages appears in the app, sometimes in the terminal running the packager (which I tend to forget as it runs in background).

To sum up, the result looks very good (by now), but the tooling is still immature, which is understandable for a rather young technology.

This concludes the second part in this journey to discover React Native. In the next one, we'll add a detailed view for each album.
