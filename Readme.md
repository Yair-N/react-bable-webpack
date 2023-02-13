# Creating a React app from scratch with babel and webpack

## Why?

The main motivation to replacing the traditional `create-react-app` is to get a clean error and warning record while installing.  

I used this tutorial https://dev.to/shivampawar/setup-webpack-and-babel-for-a-react-js-application-24f5 as a starting point but it turns out that some libraries on this tutorial are deprecated or about-to-be so I changed those to latest

## Creating a folder structure

(from tutorial)  

1. Create a folder and name it as per your application name.  
2. Open folder in command prompt (cmd).  
3. Run the following command in cmd:  

``` console
npm init

```

1. This will ask you some basic information like package name, author name. description, and license. With   this info it will create a file called package.json  
2. Create a src folder inside your project folder and add empty files named as index.js and index.html and      create two config files at your project level called .babelrc and webpack.config.js

## Install our main dependency package, React and React DOM.
npm i -S react react-dom
Install Babel as a dev dependency for your application.

`originally was npm i -D babel-core babel-loader babel-preset-env babel-preset-react`

``` console
npm install --save-dev @babel/core @babel/preset-env @babel/preset-react babel-loader core-js
```

## Install Webpack as a dev dependency for your application

``` console
npm i -D webpack webpack-cli webpack-dev-server html-webpack-plugin
```

## Configuring Babel  

In ._babelrc_file we will define the presets which we will be using in your application.

`was {"presets":["env", "react"]}`

``` js
{
    "presets": [
        "@babel/preset-env",
        "@babel/preset-react"
    ]
}
```



## Configuring Webpack

In webpack.config.js you need to add following configs  
`here i made several changes upon the tutorial version to enable all the normal behavior i was used to while developing`
``` js
//webpack.config.js
const { SourceMapDevToolPlugin } = require("webpack");
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const InterpolateHtmlPlugin = require('interpolate-html-plugin');



module.exports = {
    entry: './src/index.js',
    resolve: {
        alias: {
            "styled-components": path.resolve(__dirname, "node_modules", "styled-components"),
        }
    },
    output: {
        path: path.join(__dirname, '/dist'),
        filename: 'bundle.js'
    },
    devServer: {
        port: 3000
    },
    module: {
        rules: [
            {
                test: /\.js$/,
                enforce: 'pre',
                use: ['source-map-loader'],
            },
            {
                test: /\.jsx?$/,
                exclude: /node_modules/,
                loader: 'babel-loader',
            },
            {
                test: /\.css$/,
                use: ['style-loader', 'css-loader']
            },
            {
                test: /\.svg$/,
                use: [
                    {
                        loader: 'svg-url-loader',
                        options: {
                            limit: 10000,
                        },
                    },
                ],
            },
            {
                test: /\.(jpe?g|png|gif)$/i,
                loader: 'file-loader',

            }
        ]
    },
    plugins: [
        new InterpolateHtmlPlugin({ PUBLIC_URL: '.' }),
        new HtmlWebpackPlugin({
            inject: true,
            template: path.join(__dirname, '/public/index.html')
        }),
        new SourceMapDevToolPlugin({
            filename: '[file].map'
        }),

    ],
    mode: 'development',

}
```

* entry: Here we will define entry point of our application. From this point webpack will start bundling.
* output: We will define the location where the bundled file will reside. i.e., at /dist/bundle.js
* devServer: Here development server related configurations present like we provided port number 8080 for development server.
* test: We define some regular expression that define which files will pass through which loader.
* exclude: Define files that should be excluded by loader.
* loader: Define the loaders here which we are going to use.  

## Setting Scripts in package.json

We require some script command to run and build application, for that we need to define some script in package.json.

```js
"start": "webpack serve --mode development --open --hot",
"build": "webpack --mode production" 
```

I added the folders and structured the project at its most common forme.  
This template will be used as my create-react-app for some projects in the future.  
