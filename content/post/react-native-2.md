+++
date = "2016-08-15T20:54:34+02:00"
categories = [ "Development" ]
tags = [ "Development", "Tutorial", "Mobile", "React" ]
series = [ "React Native"]
title = "Creating a mobile application with React Native - part 2"
draft = true
+++

[Native Base](http://nativebase.io) is a complement to React Native. It provides higher level, polished components and layouts, for both iOS and Android.
<!--more-->

### Install Native Base


With Native Base you have much less boilerplate code to write. At first sight, it looks like a kind of [Twitter Bootstrap](http://getbootstrap.com/) for mobile development with React Native.

``` bash
cd AwesomeNativeBase
npm install native-base --save
```

```
mkdir android/app/src/main/assets/fonts
cp node_modules/react-native-vector-icons/Fonts/*.ttf android/app/src/main/assets/fonts/
```

Edit android/settings.gradle and add the following:

```
include ':react-native-vector-icons'
project(':react-native-vector-icons').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-vector-icons/android')
```

Edit android/app/build.gradle and add the following:

```
dependencies {
    .   .   .
    compile project(':react-native-vector-icons')
}
```

```
npm run build
```

## Create a native base GUI

Replace the content of `index.android.js with

``` html
import React, { Component } from 'react';
import { AppRegistry } from 'react-native';
import { Content, Container, Header, Text, Title } from 'native-base';

class AwesomeNativeBase extends Component {
    render() {
        return (
            <Container>
                <Header>
                    <Title>Header</Title>
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