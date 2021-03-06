# Static Next.js

In this example we will be deploying a simple "Hello World" example with Next.js static export

### Getting started with Next.js

- Create a `pages` folder with an `index.js` file with the following code:

```jsx
import Link from 'next/link'
import Header from '../components/header'

export default () => (
  <main>
    <Header />
    <section>
      <Link href="/about">
        <a>Go to About Me - hola</a>
      </Link>
    </section>
  </main>
)
```

- Now lets create an `about.js` file inside the `pages` folder with the following code:

```jsx
import { Component } from 'react'
import Link from 'next/link'
import Header from '../components/header'

class AboutPage extends Component {
  render() {
    return (
      <main>
        <Header />
        <section>
          <Link href="/">
            <a>Go to Home - hola</a>
          </Link>
        </section>
      </main>
    )
  }
}

export default AboutPage
```

- As you might noticed we have a component that is share by both `index.js` and `about.js` files, lets create that one now. Create a folder named `components` with a file named `header.js` on it and add the following code:

```jsx
export default () => (
  <header>
    <h1>Next.js Example</h1>
  </header>
)
```

- Finally in order for Next.js to be deployed we could either have a `next.config.js` or a `package.json`, for this example we are just going to create a `next.config.js` with the following code:

```js
module.exports = {}
```

### Deploy a Static version to Now

First we need to create a `now.json` configuration file to instruct Now how to build the project.

For this example we will be using our newest version [Now 2.0](https://zeit.co/now).

By adding the `version` key to the `now.json` file, we can specify which Now Platform version to use.

We also need to define each builders we would like to use. [Builders](https://zeit.co/docs/v2/deployments/builders/overview/) are modules that take a deployment's source and return an output, consisting of [either static files or dynamic Lambdas](https://zeit.co/docs/v2/deployments/builds/#sources-and-outputs).

In this case we are going to use `@now/static-build` to build and deploy our Next.js application selecting the `package.json` as our entry point. We will also define a name for our project (optional).

```json
{
    "version": 2,
    "name": "nextjs",
    "builds": [
        { "src": "package.json", "use": "@now/static-build" }
    ]
}
```

Visit our [documentation](https://zeit.co/docs/v2/deployments/configuration) for more information on the `now.json` configuration file.

We also need to include a script in `package.json` named `"now-build"` that specifies what command Now will run on the server to "build" your application. Also notice that we are using `next export -o dist` to export all the files generated by `next` to the `dist` folder which Now identifies as the static folder.

```json
{
  "scripts": {
    ...
    "now-build": "next build && next export -o dist"
  }
}
```

We are now ready to deploy the app.

```shell
$ now
```
