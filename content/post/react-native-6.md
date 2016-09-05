+++
date = "2016-08-21T19:57:49+02:00"
tags = []
title = "Creating a mobile application with React Native - part 6"
draft = true

+++

The previous articles in this [series]({{< ref "post/react-native-1.md" >}}) were dedicated to create the Android version of a React Native application.
You may have worked until now with a computer running Linux or Windows. But for this one, we will create the iOS version of the application, so you will need a Mac, with Xcode installed.

## Configure Native Base for iOS

Before starting, we need to configure the Xcode project to embedd the icon font used in the app.
Launch Xcode, then open the file named `AwesomeNativeBase.xcodeproj` under the iOS subdirectory of the ReactNAtive project.

Right click on the project in Xcode, and select `Add files to "AwesomeNativeBase"...`.
Then browse to `AwesomeNativeBase/node_modules/react-native-vector-icons/Fonts` and select `Ionicons.ttf`.

Click Info.plist, then the "+" icon at the right of "Information property list".
Start typing `Fonts provided by application`, to add a new property. Then click the "+" icon on the right of this new property, to add it a new item, and type `Ionicons.ttf`.

## Create the iOS version

Copy the content of `index.android.js` and use it to replace the content of `index.ios.js`.

Then open a new terminal tab, go to the project root and type:

`react-native run-ios`.

It compiles the app and launches the iOS simulator to execute it. You should get the exact same app as the one we built for Android.

## About the iOS simulator

The iOS simulator is the easiest way to test your app on iOS. Testing it on a real device requires a valid iOS Developper paid account, and following a rather complex procedure to be authorized to deploy it on a device.
This is outside of the scope of this tutorial, so we'll stick to using the simulator (unless you already are familiar with this process, so you can choose the other option).

As the time of writing, the simulator will emulate an iPhone 6 by default. You may change the device to be simulated by adding a flag to the launch command:
`react-native run-ios --simulator "iPhone 4s"`

To get the list of allowed values for the device name, just type the following commmand in a terminal: `xcrun simctl list`.

## Fixing the HTTP requests

At first glance the app seems to be OK... Until we try to search for an artist or a music band.

{{< img src="/post/img/react-native-6/screenshot_1.png" >}}

The origin of this error is that apps compiled for iOS9, with Xcode 7, are not allowed to access network resources without https. That's why we must change the URL for each request to Deezer in the app:

* in `search.js`:

```
superagent.get('https://api.deezer.com/search/artist?q=artist:' + name)
```

* in `album.js`:

```
superagent.get('https://api.deezer.com/artist/' + this.props.artistId + '/albums')
```

* in `tracklist.js`

```
superagent.get('https://api.deezer.com/album/' + this.props.albumId + '/tracks')
```

*If later, when creating another application, you do not have the possibilty to use https, you may find a solution in the step 3 of [this tutorial](http://moduscreate.com/cordova-5-ios-9-security-policy-changes/).*

Now our app is fully functional, both under iOS and Android! But it's definitely not enough: it's an Android app running under iOS, not a real iOS app. To create one, we should take in account the special features of each platform. Hopefully, React Native provides us several ways to create apps with platform specific behaviors and/or UI components.

## Platform specific icons

The icons used until now in the app come from [Ionicons](http://ionicframework.com/docs/v2/ionicons/), a font created for [Ionic](http://ionicframework.com/). Ionic is an alternative to React Native —a major difference between Ionic and ReactNative being that Ionic uses a webview for rendering, so Ionic based apps usually have a slower UI.

Ionicons provides iOS and Material Design variants for most of the icons. To select the icon to be used according to the current OS, start by importing the `Platform` class in `album.js`:

```
import { Platform } from 'react-native';
```

Then modify the `else` branch in `render()`, to match the following lines:

{{< highlight html "linenos=inline" >}}
else {

  var searchIconName;
  if(Platform.OS === 'ios') {
    searchIconName = "ios-search";
  } else {
    searchIconName = "md-search";
  }

  return(
    <Container>
        <Header>
           <Button transparent>
              <Icon name="md-disc" />
          </Button>
           <Title>{this.props.title}</Title>
           <Button transparent onPress={()=>this.displaySearch()}>
              <Icon name={searchIconName} />
           </Button>
        </Header>
        ...
    </Container>
  );
}
{{< / highlight >}}

The lines 3 to 8 define the value for the name attribute of the search icon declared line 18. It varies according to the value of `Platform.OS`, which may be either "ios" or "android". This is a first and simple way of customizing the UI.

An alternative solution would have been to make vary the full JSX tag, and not only an attribute. We will use this solution for the menu icon:

{{< highlight html "linenos=inline" >}}
else {

  var menuIcon;
  var searchIconName;
  if(Platform.OS === 'ios') {
    menuIcon = <Icon name="ios-disc" />;
    searchIconName = "ios-search";
  } else {
    menuIcon = <Icon name="md-disc" />;
    searchIconName = "md-search";
  }

  return(
    <Container>
        <Header>
           <Button transparent>
              {menuIcon}
          </Button>
           <Title>{this.props.title}</Title>
           <Button transparent onPress={()=>this.displaySearch()}>
              <Icon name={searchIconName} />
           </Button>
        </Header>
        ...
    </Container>
  );
}
{{< / highlight >}}

Now in line 17 above, the child component of the left button in the header will be defined by the value of `menuIcon`. This is a second tool React Native offers us to create A platform specific UI.

## OS dependent components

In the previous examples, the modifications were rather limited. We just modified icons.
Now we will use one a more powerful feature of React Native: component selection.

You already noticed that the app contains two main files, named `index.ios.js` and `index.android.js`. The same naming convention may be applied to create two variants for any component. At build time, React Native will use the one matching the target platform.

To illustrate this feature, we can define specific item renderers for the album list.

Start by replacing the `renderRow` attribute of the List tag in `albumlist.js` with a new component named `AlbumListItem`:

```
<List dataArray={this.state.albums}
  renderRow={ (item) =>
      <AlbumListItem item={item} onPress={()=>this.displayTracks(item)} />
  }>
</List>
```

The AlbumListItem instances will be instanciated with two properties: `item` and `onPress`. The former contains the album object to be rendered, the latter a function which invokes `displayTracks` with the same album object as a parameter. `displayTracks()` is a function of AlbumList, declared a few lines above.

Now create a file named `albumlistitem.android.js`, with the following content:

```
import React, { Component } from 'react';
import { Button,
         Content,
         Container,
         Header,
         Icon,
         List,
         ListItem,
         Spinner,
         Text,
         Thumbnail,
         Title
       } from 'native-base';

export default class AlbumListItem extends Component {

    render() {
      return(
          <ListItem button onPress={()=>this.props.onPress(this.props.item)}>
              <Thumbnail square size={60} source={{uri: this.props.item.cover_small}} />
              <Text>{this.props.item.title}</Text>
          </ListItem>
      );
    }
}
```

It declares a component that may be used as a list item renderer. The JSX code is very similar to the one returned by the `renderRow` function of `albumlist.js`. The only difference resides in the value of the attributes. By example, `{item.title}` is replaced with `{this.props.item.title}`. This is a way to access the item property of the albumListItem, defined in album list by `<AlbumListItem item={item} onPress={()=>this.displayTracks(item)} />`.

Similarly, the onPress attribute of ListItem in `albumlistitem.android.js` invokes the function passed through the onPress attribute of AlbumListItem in `albumlist.js` —that is to say, the onPress event on a ListItem will invoke the `AlbumList.displayTrack` function.

Now create a file named `albumlistitem.ios.js`, with the following content:

```
import React, { Component } from 'react';
import { StyleSheet } from 'react-native';
import { Button,
         Content,
         Container,
         Header,
         Icon,
         List,
         ListItem,
         Spinner,
         Text,
         Thumbnail,
         Title
       } from 'native-base';

export default class AlbumListItem extends Component {

    render() {
      return(
          <ListItem button onPress={()=>this.props.onPress(this.props.item)}>
              <Thumbnail square size={60} source={{uri: this.props.item.cover_small}} />
              <Text>{this.props.item.title}</Text>
              <Icon name="ios-arrow-dropright" style={styles.icon}/>
          </ListItem>
      );
    }
}


const styles = StyleSheet.create({
  icon: {
    flex: 0,
    alignSelf: 'flex-end'
  }
});
```

This second AlbumListItem has only one difference with the Android one: it adds an arrow icon at the end of each line. It's just a way to illustrate the mechanism, nothing more. I won't even try to design a real iOS list here.

The final touch to complete this trick is to import the new component into `albumlist.js`, by adding an import line:

```
import AlbumListItem from './albumlistitem';
```

ReactNative will select the variant to be included according to the target platfom.

## A sidenote about imports

Be very careful with the way you import components. Adding brackets around AlbumListItem in the above directive would lead React Native to display a unfriendly error message, that probably would leave you without any clue about how to fix it.

There are two ways to declare and import components in React (and so in React Native). The first is to declare the class without the default keyword, and then it's called a `named export`. You must import a named exported component using its exact name, and braces:

``` javascript
// in albumlistitem.js
export class AlbumListItem extends Component {
...
}

// in albumlist.js
import { AlbumListItem } from 'albumlistitem.js';
```

Using named exports, you can declare several components in a unique file.

The second way of exporting components is like this:

``` javascript
// in albumlistitem.js
export default class AlbumListItem extends Component {
...
}

// in albumlist.js
import AlbumListItem from 'albumlistitem.js';

// or
import ItemRenderer from 'albumlistitem.js';

```
As you can see, the default keyword removes the possibility to declare multiple classes in a file; but it allows renaming the component at import time. This is the usual way to declare and export components in React.


## Conclusion

We have seen how to create an iOS version of our app rather easily. We also discovered several mechanisms React Native offers to include platform specific UI components and behaviors. This seems well designed and quite easy to implement. Moreover, React Native plays well with the iOS simulator —which is nicer to use than its Android counterpart. The only cloud on the horizon remains the error messages, which, more than once, left me quite disconcerted.

In the next (and probably last) article of this series, we'll see how to share code between platformx@@specific components.


