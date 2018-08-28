
document:[creat-react-app](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md)

---

#### Using the public Folder
Inside index.html, you can use it like this:
```
<link rel="shortcut icon" href="%PUBLIC_URL%/favicon.ico">
```
In JavaScript code, you can use process.env.PUBLIC_URL for similar purposes:
```
render() {
  return <img src={process.env.PUBLIC_URL + '/img/logo.png'} />;
}
```
downsides of this approach

- None of the files in public folder get post-processed or minified.
- Missing files will not be called at compilation time, and will cause 404 errors for your users.
- Result filenames won’t include content hashes so you’ll need to add query arguments or rename them every time they change.

---

so we recommand import assets in JavaScript files instead.This mechanism provides a number of benefits:
- Scripts and stylesheets get minified and bundled together to avoid extra network requests.
- Missing files cause compilation errors instead of 404 errors for your users.
- Result filenames include content hashes so you don’t need to worry about browsers caching their old versions.

---

When to Use the public Folder

- You need a file with a specific name in the build output, such as manifest.webmanifest.
- You have thousands of images and need to dynamically reference their paths.
- You want to include a small script like pace.js outside of the bundled code.
- Some library may be incompatible with Webpack and you have no other option but to include it as a <script> tag.


---

### Environment Variables
process.env.NODE_ENV:'development'/'production'

%PUBLIC_URL%/ : It will be replaced with the URL of the `public` folder during the build.

#### webpack.config
```
const publicUrl = publicPath.slice(0, -1);
// `publicUrl` is just like `publicPath`, but we will provide it to our app
// as %PUBLIC_URL% in `index.html` and `process.env.PUBLIC_URL` in JavaScript.
```



#### Code spilit

点击按钮时才加载组件,注意import是个promise
```
import React, { Component } from 'react';

class App extends Component {
  handleClick = () => {
    import('./moduleA')
      .then(({ moduleA }) => {
        // Use moduleA
      })
      .catch(err => {
        // Handle failure
      });
  };

  render() {
    return (
      <div>
        <button onClick={this.handleClick}>Load</button>
      </div>
    );
  }
}

export default App;
```
另外webpack和react-router也有自己的代码分割方法



