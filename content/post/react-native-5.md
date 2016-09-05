+++
date = "2016-08-20T16:24:44+02:00"
tags = [ "Development", "Tutorial", "Mobile", "React", "React Native" ]
series = [ "React Native"]
title = "Creating a mobile application with React Native - part 5"
draft = true

+++

In this fifth part of the [article series about React Native]({{< ref "post/react-native-1.md" >}}), we'll see how to support the native back button on Android devices.

<!--more-->

To support the back button, we only have to modify the main android file, `index.android.js`.

React Native provides a module named `BackAndroid`, that exposes an object on which you can register listeners. The  listeners are invoked when the back button is pressed. Then the app exits if there are no listeners or if none of the listeners returned true.

In our implementation, the listener checks how many views are in the navigator's stack, by calling `navigator.getCurrentRoutes()`:

```javascript
componentDidMount() {
  BackAndroid.addEventListener('hardwareBackPress', () => {
    if (this.navigator && this.navigator.getCurrentRoutes().length > 1) {
      this.navigator.pop();
      return true;
    }
    return false;
  });
}
```
If there are more than one scene in the stack, it drops the top one so we get back to the previous view. Otherwise, it returns false, to exit the app.

To ensure this listener is registered only once, we do it in one of the [lifecycle methods](https://facebook.github.io/react/docs/component-specs.html#lifecycle-methods) offered by React Components: `componentDidMount`. This method is invoked once, just after the first rendering of the component.

The listener uses the navigator. By default there is no simple way to access it outside of the rendering methods (`render` and `renderScene`). That's why we declare a navigator property in the constructor (more for documentation than anything else, as it is not required from a technical point of view):

``` javascript
  constructor(props) {
    super(props);
    this.navigator = null;
  }
```

And then we set this property in `renderScene`:

``` javascript
  renderScene(route, navigator) {
    this.navigator = navigator;
    ...
```

You could suppose this should do the trick. But running the app like this generates an error message:

{{< img src="/post/img/react-native-5/screenshot_1.png" >}}

We made a very common mistake in javascript: `this` in the `renderScene` function does not point to the AwesomeNativeBase object, but the object that "owns" the function, that is to say the navigator itself.

To solve this, just replace

``` javascript
renderScene={ this.renderScene }
```

with:

``` javascript
renderScene={ this.renderScene.bind(this) }
```



Here is the final code:

``` html
  constructor(props) {
    super(props);
    this.navigator = null;
  }

  componentDidMount() {
    BackAndroid.addEventListener('hardwareBackPress', () => {
      if (this.navigator && this.navigator.getCurrentRoutes().length > 1) {
        this.navigator.pop();
        return true;
      }
      return false;
    });
  }

  renderScene(route, navigator) {
    this.navigator = navigator;

    var scenes = [
      <AlbumList title={ route.title } artistId={ route.artistId } navigator={ navigator } />,
      <TrackList title={ route.title } albumId={ route.albumId } navigator={ navigator } />,
      <Search navigator={ navigator } />
    ]
    return(scenes[route.scene]);
  }

  render() {
    return (<Navigator
        initialRoute={ { scene: 0 } }
        renderScene={ this.renderScene.bind(this) }
      />);
  }
  ```

  As usual, the code of the app is available on github.

## Conclusion

React Native provides a simple way to reproduce this Android common feature.

In the suite of these series, I will stop adding features to the app.
Now, it's about time building an iOS version!



