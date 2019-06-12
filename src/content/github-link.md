---
layout: post
title: React Gotchas and Code Smells!
image: img/smell.jpg
author: Thulani S. Chivandikwa
date: 2019-03-10T10:00:00.000Z
tags: [React, Web]
draft: true
---

## My path to React greatness

It has been exactly about a year now since I started my journey with React. I am at a point now where I am very comfortable with the concepts and have begun to master the library. I am at a point where I am aware of many of the common pitfalls/gotchas, I can pick up on code smells and conforming to community accepted best practices. It's as good a time as ever for me to share some of this knowledge. Let's do this! :sunglasses:

### Setting props from state in ctor :disappointed:
A very simple and easily overlooked mistake is to use props to initialize the state in the constructor as shown in the gist below. The catch with this is that when the props change and the component rerenders the constructor is not called again and hence any mutations to the props will not be reflected in the state. Running this sample will not update the user name when the delayed function to mutate the user name to jane completes.

`gist:chivandikwa/97d7545134a23f301f7b8886be60cf08#PropsSetToStateInCtor.tsx`

A simple and clean fix to this is to use the props directly. Building on the previous example the component render can be updated to the following:

```TypeScript
  public render(): React.ReactElement {
    const userName = this.props.user.name;
    return <div>{userName}</div>;
  }
```
