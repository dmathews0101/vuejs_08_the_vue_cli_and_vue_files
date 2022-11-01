# vuejs_08_the_vue_cli_and_vue_files

1. In the setup in jsfiddle, we drop in a script import, take control over a part of our DOM, with the el property, and output some data there. this is the best way when we are controlling smaller part of our DOM.
2. If we are building a bigger or even medium sized applications, this is not the preferred way, because it doesnt integrate very well into an existing workflow, if we are already building and bundling sass code, we are not doing the same for our vuejs code, and we might also want to split our vuejs code up over multiple files and so on. 
3. summary : how to easily get started with a nice setup for a bigger or medium sized vuejs application, using webpack as a build tool
4. in order to get a nice preconfigured workflow, which we can fine tune to our needs we can use the vue cli here. 
5. the vue cli is a nice tool which makes scaffolding out preconfigured vue projects very simple. project here means a basic folder structure, and the basic workflow setup using webpack.
6. it also gives us some commands we can run to bundle our code, to build it, to split it up over multiple files and to have it then running on a development server in the end, all with one command, and a second command which allows us to build our files for production.
7. we could set up such a workflow, on our own, but we can also fine tune an existing one 
8. to quickly get started using the vue cli, 
9. to install the cli : npm install -g vue-cli
10. we have webpack vuejs-templates that we can use with the vue cli, a full list of all the available templates is found on the vue cli github page. The template list includes webpack, webpack-simple, browserify, browserify-simple and simple.
11. templates are really just template for the project you want to use. we could use a webpack template or a simplified one without unit test and so on. we can also choose browserify or a simple template which basically is the same approach used in the previous projects, where the local environment is used in the browser.
12. here, the webpack-simple template is used for this project, because it is a template which is quick and easy webpack template to get started with.
13. in order to use it, we have to go into a folder where we want to create the new project folder, and there we can simply type : 
vue init name-of-the-template name-of-the-folder
vue init webpack-simple vue-webpack
14. If vue cli version 3+ is installed. Command vue init requires a global addon to be installed
npm install -g @vue/cli-init
or
yarn global add @vue/cli-init
15. next step is to cd into the newly created folder, thereafter we have to run npm install, because this template doesnt install the dependencies, or this command doesnt install the dependencies our project needs, because  ofcourse we do have a more complex workflow here, with a couple of dependencies, a couple of third party packages we use and we need to install these packages with npm install. 
16. running this command installs webpack, various loaders to work with webpack, and so on.
17. After installing all the dependencies, we can run the last command : npm run dev
18. this will start the development workflow, that means it will bundle our project, and it will start up a development server, 
19. This is a little server which runs on our machine, driven by nodejs(another reason to install it , other than using npm), which will host our application, so that we can see it running on our machine without deploying it to an actual server.
20. At localhost:8080 we can see the basic application used with this template, 
21. summary: how to install a template with the cli 
22. what got installed, which folders and files were created for us
23. node_modules folder holds all our dependencies like webpack and vuejs. We can see a full list of the dependencies installed in the package.json. There is only one runtime dependency : vue ^2.1.0, and a couple of development only dependencies, which are needed for the workflow.
"devDependencies" : {
    "babel-core": "^6.0.0",
    "babel-loader": "^6.0.0",
    "babel-preset-es2015": "^6.0.0",
    "cross-env": "^3.0.0",
    "css-loader": "^0.25.0",
    "file-loader": "^0.9.0",
    "vue-loader": "^10.0.0",
    "vue-template-compiler": "^2.1.0",
    "webpack": "^2.1.0-beta.25",
    "webpack-dev-server": "^2.1.0-beta.0"
} 
24. we also have the src folder, this is an important folder, because, here in this folder the application is developed.
25. Here a .vue file is there. Earlier there was only one file where the script was in. We also have an html file outside the src folder, this is the file which will actually get served. It has a div with the id app, and it imports a script. they are importing some script, which will then again take over this id, or the div with id app.
<script src="/dist/build.js"></script>
26. this build.js script is the bundle created by webpack, and it bundles all our scripts in our source folder. It starts with the main.js file, this is just how it is set up, which we can see in the webpack.config.js file. 

=> main.js
import Vue from 'vue'
import App from './App.vue'

new Vue({
  el: '#app',
  render: h => h(App)
})

=> webpack.config.js

var path = require('path')
var webpack = require('webpack')

module.exports = {
  entry: './src/main.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    publicPath: '/dist/',
    filename: 'build.js'
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          'vue-style-loader',
          'css-loader'
        ],
      },      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: {
          loaders: {
          }
          // other vue-loader options go here
        }
      },
      {
        test: /\.js$/,
        loader: 'babel-loader',
        exclude: /node_modules/
      },
      {
        test: /\.(png|jpg|gif|svg)$/,
        loader: 'file-loader',
        options: {
          name: '[name].[ext]?[hash]'
        }
      }
    ]
  },
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js'
    },
    extensions: ['*', '.js', '.vue', '.json']
  },
  devServer: {
    historyApiFallback: true,
    noInfo: true,
    overlay: true
  },
  performance: {
    hints: false
  },
  devtool: '#eval-source-map'
}

if (process.env.NODE_ENV === 'production') {
  module.exports.devtool = '#source-map'
  // http://vue-loader.vuejs.org/en/workflow/production.html
  module.exports.plugins = (module.exports.plugins || []).concat([
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: '"production"'
      }
    }),
    new webpack.optimize.UglifyJsPlugin({
      sourceMap: true,
      compress: {
        warnings: false
      }
    }),
    new webpack.LoaderOptionsPlugin({
      minimize: true
    })
  ])
}

27. entry: './src/main.js',
28. it starts at main.js, it applies some loaders( vue-loader, babel-loader, file-loader ) to transforms these files, and it outputs the build.js file to the ./dist folder, which we import in the index.html file.
29. path: path.resolve(__dirname, './dist')
output: {
    path: path.resolve(__dirname, './dist'),
    publicPath: '/dist/',
    filename: 'build.js'
}
30. in the main.js file, we are creating a vue instance here, we take over the element here with the id app selector, 

new Vue({
    el: '#app',
    render: h => h(App)
})

31. we then call the render method, the render method is a built in method vue instance knows which basically means replace the part you are selecting the DOM here, with some other code, 
32. the default behaviour of vuejs is to select a certain part in the DOM, with the el property here, so for example the div with the id app here, and then take this DOM as a template where we could use string interpolation and so on.
33. but the div here in index.html is empty, we dont have a (<p>{{ title }}</p>) paragraph where we output a title or something like this, 
34. what we are doing here instead is, we are selecting this part, but the render method now overrides it, it tells vuejs, dont use the DOM as the template, instead just use it as a hook, and now override it with some other template, or some other html code
35. the render function, simply takes an input here, which is provided by vuejs, which is a function which gets executed in the end, but the interesting thing here is not how this function works, which is relatively complicated complicated one,  
36. the interesting thing here is what we pass as an argument to this callback function, this h function here, and thats App, which we are importing from the App.vue file

import App from './App.vue'

render: h => h(App)
37. in App.vue file we have a template at the top, then we have a script tag, then we have some styles
38. .vue files are special files in vuejs which allow us to store all our template, our script and our styles in one file. we can therefore split our vuejs app into logical pieces - components, and each component could live in a .vue file. 
39. And each component has at least a template - something we render - some html code, we store that between template tags, which is always needed.
40. and then we have maybe a script, we dont need it necessarily, but if we want to have some logic, we probably do need, and also probably also optionally, we do have some styles we apply to this application. 
41. styles by standard are applied to the whole application, they are not scoped, to this vue file, though we could force this by adding scoped as an attribute to the style tag. with that we are telling vuejs, please only apply these styles to this file, to this template up here. If we dont have scoped here, they would be applied to our whole application.
42. in the template we have some html code, here one important restriction is that inside of this template, we must have only one root element, so this div in this case here. 
43. what does the vue file do: webpack has a special package, the vue-loader, which knows how to handle .vue files, what the vue loader will do is it will take this file, transform our template to html code kindoff, 
44. we can ofcourse represent html elements with javascript code, -- it transforms into javascript code
45. it will also pack in our script part here, it will add our styles to the top of the index.html file in the head section and therefore, it allows us since we only have javascript here, all this is transformed to javascript, to bundle this, to pass this javascript code to the render function, which needs javascript code, which it renders in the dom in the end, which it executes in the end, 
46. it does all that for us, therefore, one single bundle gets created, but it gives us the nice possibility of splitting up our code into multiple files of using this .vue file concept, to have a clear seperation of the different components in our application, 
47. here we are not really using a component, this is kind of our root component, we are just taking this app.vue file, the template and the script of it, and we are rendering this, and replacing our selected part in the dom, with the App selector
48. we can use .vue files to create multiple components 
49. summary: vue-cli, how to get started with better workflow, with a local project using webpack, basics of .vue files