+++
date = "2016-08-20T00:12:11+02:00"
tags = [ "Development", "Tutorial", "Mobile", "React", "React Native" ]
series = [ "React Native"]
title = "Creating a mobile application with React Native - part 4"
draft = true

+++

This fourth part in the React Native [article series]({{< ref "post/react-native-1.md" >}}) will be dedicated to adding an artist search feature.

<!--more-->

## Create the search scene

The initial scene will stay the album list, but we will add it a search button. A tap on this button will display a search screen on top of the album list. This last scene will be created in a file named `search.js`:

{{< highlight html "linenos=inline" >}}
import React, { Component } from 'react';
import { InteractionManager } from 'react-native'
import { Button,
         Content,
         Container,
         Header,
         Input,
         InputGroup,
         List,
         ListItem,
         Icon,
         Spinner,
         Text,
         Thumbnail,
         Title } from 'native-base';
var superagent = require('superagent');

export default class TrackList extends Component {

    constructor(props) {
      super(props);
      this.state = {
          loading: false,
          results: null
      }
    }

    search(name) {
      if(name.length < 3) {
        return
      }
      superagent.get('http://api.deezer.com/search/artist?q=artist:' + name)
        .set('Accept', 'application/json')
        .end((err, response) => {
            if (response.ok) {
              this.setState({
                  loading: false,
                  results: response.body.data
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

    displayAlbums(artist) {
      var route = {
        scene: 0,
        title: artist.name,
        artistId: artist.id
      }
      this.props.navigator.replacePrevious(route)
      this.back();
    }

    render() {
        if(this.state.loading) {
          return (
            <Container>
                <Header>
                  <Title>search in progress...</Title>
                </Header>
              <Spinner color="#440099"/>
            </Container>
          );
        }
        else {
          return(
            <Container>
                <Header searchBar rounded>
                  <InputGroup borderType="regular">
                      <Icon name="md-search" />
                      <Input placeholder="Artist name" onChangeText={(text) => this.search(text)}/>
                      <Icon name="md-people" />
                  </InputGroup>
                  <Button transparent>
                      Search
                  </Button>
                </Header>
                <Content>
                  <List dataArray={this.state.results}
                        renderRow={(item) =>
                            <ListItem button onPress={()=>this.displayAlbums(item)}>
                                <Thumbnail square size={56} source={{uri: item.picture_small}} />
                                <Text>{item.name}</Text>
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

There are some specific parts in this scene, compared to TrackList and AlbumList:

* first, the Header component (line 77) has an attribute "searchBar", and contains the required components for displaying a search bar as defined by [Native Base](http://nativebase.io/docs/v0.5.2/components#searchBar);
* second, the main content of the scene is a list of artists (line 88), updated by the `search` function (line 28) each time the content of the search field changes (as long as it contains at least three characters);
* last but not least, a tap on an item in the result list (line 90) invokes the `displayAlbums` function (line 53) that replaces the route for the underlying album list. The new route contains the reference of the artist on which the user tapped.

## Update the main class to integrate the search scene
Then we need, as usual, to import this new scene in the app. In `index.android/js`, add:

```javascript
import Search from './search';
```

We could add an `else if` clause in the `renderScene` function, or replace the existing `if...else` with a `switch...case`. But these solutions, although effective, will produce a code with a poor readability when the number of scenes continues to grow. That's why I prefer the following form, with an array:

```javascript
  renderScene(route, navigator) {

    var scenes = [
      <AlbumList title={ route.title } artistId={ route.artistId } navigator={ navigator } />,
      <TrackList title={ route.title } albumId={ route.albumId } navigator={ navigator } />,
      <Search navigator={ navigator } />
    ]
    return(scenes[route.scene]);
  }
```

We will now simplify the initial rendering: by default we not not display anything in the album view (Good bye Daft Punk!). Doing this just require to remove the album ID and title from the initial route:

```
  render() {
    return (<Navigator
        initialRoute={ { scene: 0 } }
        renderScene={ this.renderScene.bind(this) }
      />);
  }
```

This results in a rather austere home screen for the app, but I'll let the improvement of this screen as an exercise to the reader ^^.

{{< img src="/post/img/react-native-4/screenshot_1.png" >}}


## Adapt the AlbumList scene
Finally, modify the `render` function in `albumlist.js` to add a search button in the header of the Album list:

``` html
<Header>
   <Button transparent>
      <Icon name="md-disc" />
  </Button>
   <Title>{this.props.title}</Title>
   <Button transparent onPress={()=>this.displaySearch()}>
      <Icon name="md-search" />
   </Button>
</Header>
```

And add a `displaySearch` function in this same file:

```javascript
// display the search scene
displaySearch() {
  var route = {
                scene: 2
              }
  this.props.navigator.push(route);
}
```
A tap on the search button in the header will invoke `displaySearch` and so display the search scene in front of the album list.

The search feature is now complete:

{{< img src="/post/img/react-native-4/screenshot_2.png" >}}



{{< img src="/post/img/react-native-4/screenshot_3.png" >}}



{{< img src="/post/img/react-native-4/screenshot_4.png" >}}


## Conclusion

The array of scenes in the renderScene function could probably be replaced with a better solution. Maybe each scene could define it's of rendering function and initialize it with data passed through the route. Some frameworks could  offer elegant solutions too. But for the sake of simplicity, we'll stick to this implementation.

In the next part, we will add the support of the Android back button.












