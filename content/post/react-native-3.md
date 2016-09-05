+++
date = "2016-08-18T17:11:22+02:00"
tags = [ "Development", "Tutorial", "Mobile", "React", "React Native" ]
series = [ "React Native"]
title = "Creating a mobile application with React Native - part 3"

+++


In this third part of the [series]({{< ref "post/react-native-1.md" >}}) we will see of to navigate through different screens in the application.
<!--more-->
## Add a Navigator

We will use a navigator, which is a component for selecting the current scene to display.
A scene is just a component rendered full screen.

The first thing to do is to transform the render function of the album list to make it a scene, and not just a component:

```javascript

    render() {
        if(this.state.loading) {
          return (
            <Container>
              <Spinner color="#440099"/>
            </Container>
          );
        }
        else {
          return(
            <Container>
                <Header>
                    <Title>{this.props.title}</Title>
                </Header>
                <Content>
                  <List dataArray={this.state.albums}
                        renderRow={(item) =>
                            <ListItem onPress={()=>this.displayTracks(item.id)}>
                                <Thumbnail square size={60} source={{uri: item.cover_small}} />
                                <Text>{item.title}</Text>
                            </ListItem>
                        }>
                  </List>
                </Content>
            </Container>
          );
        }
    }
```

The Container definition is now included in the rendering function of the AlbumList component.
Don't forget to update the list of imported components for NativeBase, always in `albumlist.js`:

```
import { Content, Container, Header, List, ListItem, Spinner, Text, Thumbnail, Title } from 'native-base';
```

You may have noticed the Header content has changed too, in order to display a title which will be injected when mounting the album list.

Then, going back to the `index.android.js` file, we need to update the list of imports, to remove those who were moved to AlbumList, and import the Navigator component:
```
import { AppRegistry, Navigator } from 'react-native';
```

Then we replace the render function with these lines:


```html
renderScene(route, navigator) {
  return(
    <AlbumList title={ route.title } artistId={ route.artistId } />
  )
}

render() {
  return (
    <Navigator
      initialRoute={{ title: 'Album List for Daft Punk', artistId:27 }}
      renderScene={ this.renderScene }
    />
  );
}
```

The initialRoute property represents the context for this first scene.
Each scene is indeed associated to a *route* (which is another name for a context in React Native). You can add any property on a route.
In our example, we'll include two properties in the routes:
- an Id: albumId or artistId according to the type of the view;
- the title to be displayed in the header.


So in `albumlist.js` we change the URL to be requested, to use the ID of the artist injected when displaying the AlbumList component:

```javascript
superagent.get('http://api.deezer.com/artist/' + this.props.artistId + '/albums')
```

At this stage your code should look like [this](https://github.com/PascalLeMerrer/react-native-tuto/tree/step3-1).

## Add the track list scene

Next, create a new file named `tracklist.js`, with the following content:

{{< highlight html "linenos=inline" >}}
import React, { Component } from 'react';
import { Button, Content, Container, Header, List, ListItem, Icon, Spinner, Text, Title } from 'native-base';
var superagent = require('superagent');

export default class TrackList extends Component {

    constructor(props) {
      super(props);
      this.state = {
          loading: true,
          tracks: null
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

    back() {
      this.props.navigator.pop();
    }

    render() {
        if(this.state.loading) {
          return (
            <Container>
                <Header>
                   <Button info>
                    <Icon name="md-arrow-back"/>
                   </Button>
                   <Title>{this.props.title}</Title>
                </Header>
              <Spinner color="#440099"/>
            </Container>
          );
        }
        else {
          return(
            <Container>
                <Header>
                   <Button info onPress={()=>this.back()}>
                    <Icon name="md-arrow-back" />
                   </Button>
                   <Title>{this.props.title}</Title>
                </Header>
                <Content>
                  <List dataArray={this.state.tracks}
                        renderRow={(item) =>
                            <ListItem>
                                <Text>{item.title}</Text>
                            </ListItem>
                        }>
                  </List>
                </Content>
            </Container>
          );
        }
    }
}
{{< / highlight >}}

This defines a new scene named TrackList, which displays the list of tracks for a given album.
This new component is rather similar to the AlbumList.
It loads a list of tracks for the album whose id will be provided as a property of the TrackList instance (line 20).
It also expects the navigator instance to be passed as a property. It is used in the `back` function (line 38) to drop this scene and go back to the AlbumList. The `back` function is invoked when pressing the button in the header (line 59).

Now we must add an import directive in `index.android.js` to be able to use this new scene:

``` javascript
import TrackList from './tracklist';
```

And then we change the rendering functions of the app:

```
class AwesomeNativeBase extends Component {

    renderScene(route, navigator) {
      if (route.scene == 1) {
        return(
          <TrackList title={ route.title } albumId={ route.albumId } navigator={ navigator }/>
        )
      }
      else {
        return(
          <AlbumList title={ route.title } artistId={ route.artistId } navigator={ navigator } />
        )
      }
    }

    render() {

      const routes = [ { scene: 0,
                         title: 'Album List for Daft Punk',
                         artistId: 27
                       }
                     ];
      return (
        <Navigator
          initialRoute={ routes[0] }
          renderScene={ this.renderScene }
        />
      );
    }
}
```
The `renderScene` function displays either an AlbumList or a TrackList, according to the selected route. It also provides the selected scene with contextual properties (a title, an artist or album ID) and injects the navigator instance. The scene property in the routes identifies the type of scene to render (AlbumList or TrackList).

Finally, we have to modify the album list, to make the list items interactive and trigger the display of the track list when one of them is tapped.

In `albumlist.js`, add a function named `displayTracks`:

``` javascript
displayTracks(album) {
    var route = {
                  albumId: album.id,
                  title: album.title,
                  scene: 1
                }
    this.props.navigator.push(route);
}
```

And in the render function, replace `<List Item>` with
```html
<ListItem button onPress={()=>this.displayTracks(item)}>
```

Adding the `button` attribute to the `ListItem` component makes it interactive. It allows it to detect the onPress event and so invoke `displayTracks` when it is tapped. The camel case is important there, make sure to write `onPress` and not `onpress`.

You should now be able to go back and forth between the Album List and Track List.
{{< img src="/post/img/react-native-3/screenshot_1.png" >}}


## Performance issues

The display of the list of tracks works, but it is slow. It may even be very slow.
To improve this, we have three easy steps to perform.

First, disable the remote debugging if it is active (press the menu button in the app). The remote debugger has a dramatic impact on transition performances.

Second, remove the console.log() calls you may have inserted. Their impact may be rather significant, as usual with logs.

And last but not least, wait for the transition to be finished before making a request to the Deezer API and displaying the response. To do this, we must import the InteractionManager in `tracklist.js`:

```javascript
import { InteractionManager } from 'react-native'
```

And then replace the `componentDidMount` function with:
``` javascript
componentDidMount() {
  // delay the loading after the end of the transition
  InteractionManager.runAfterInteractions(() => {
    this.load();
  });
}
```

After these three simple modifications, the performance is quite good. It should be even better when we will package the application in release mode, after the development is finished.

The final code for this article is available [here](https://github.com/PascalLeMerrer/react-native-tuto/tree/step3-2).

## Conclusion

The way to a working application wih several view using the basic Navigator component was not as easy as I thought it would be. I had to try several solutions, and did not find a way to do it without injecting the navigator into the scenes. I would have prefered to inject a fonction only, to avoid the scenes to be aware of the implementation details of the navigation. There are many alternative navigation components available, maybe it's a sign there is some room for improvement on the basic one.

On the positive side, the [documentation](https://facebook.github.io/react-native/docs/performance.html) gives some simple and effective hints to improve the performance of your application.

In the next article, we will add a last scene to this marvelous application. It could be nice indeed, if the app was not limited to display Daft Punk album and track lists ;)















