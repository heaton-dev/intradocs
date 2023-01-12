---
title: Task 002
description: Create example front-end
---

The plan is to go with this approach, Javascript, HTML, pretty standard stuff.

```js
// pages/_app.js
import React from 'react';
import App, { Container } from 'next/app';
import Layout from '../components/Layout';

class MyApp extends App {
  render() {
    const { Component, pageProps } = this.props;

    return (
      <Container>
        <Layout>
          <Component {...pageProps} />
        </Layout>
      </Container>
    );
  }
}

export default MyApp;
```
