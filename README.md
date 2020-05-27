# Simple Webapp With Webpack Configured Manually
 
 > Lets skip the CRA and build our simple webpack powered by our webpack configurations

## Steps involved in creating this project

1. Load the `JS` file from `src/index.js` and run `webpack` with config file. You need mode, entry and output configs

```javascript
const path = require('path');

module.exports = {
    mode: 'development',
    entry: './src/index.js',
    output: {
        filename: 'main.js',
        path: path.resolve(__dirname, 'dist'),
    },
};
```
2. Let us bundle CSS files by installing CSS loaders. There are 2 things which come in play
    * css-loader will load css into webpack. With just this loader alone we cannot see any output
    * style-loader will hook up the loaded css into DOM
    * the order of loader execution is from right to left or bottom to top

3. Let us load other assets like images, fonts with `file-loader`. These files will be replaced with a hashed name in the output html or css

After the `Asset Management` or input management, the config file looks as below.

```javascript
const path = require('path');

module.exports = {
    mode: 'development',
    entry: './src/index.js',
    output: {
        filename: 'main.js',
        path: path.resolve(__dirname, 'dist'),
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    'style-loader',
                    'css-loader'
                ]
            },
            {
                test: /\.(svg|png|jpg|gif)/,
                use: 'file-loader'
            },
            {
                test: /\.(woff|woff2|eot|ttf|otf)/,
                use: 'file-loader'
            }
        ]
    }
};
```

4. On the output end we have to manage `html` to handle dynamic bundles and css file names because `main.js` wont work in production due to browser caching.

When you have multiple entry points, change the output accordingly.

Use a html plugin to generate `index.html`. Then use a clean up plugin to delete the output folder.

The `Output management` is easy. You got to configure the output file names and the index.html.

```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {
    mode: 'development',
    entry: {
        main: './src/index.js',
        hello: './src/sayHello.js'
    },
    output: {
        filename: '[name].[contentHash].bundle.js',
        path: path.resolve(__dirname, 'dist'),
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    'style-loader',
                    'css-loader'
                ]
            },
            {
                test: /\.(svg|png|jpg|gif)/,
                use: 'file-loader'
            },
            {
                test: /\.(woff|woff2|eot|ttf|otf)/,
                use: 'file-loader'
            }
        ]
    },
    plugins: [
        new CleanWebpackPlugin(),
        new HtmlWebpackPlugin({
            title: 'Sample webpack app'
        })
    ]
};
```

5. Set up development

 * Set up source map with `devtool`. Without source map we cannot find the exact line number in which it errored.
 * Set up `webpack-dev-server` with hot module reloading

 ```javascript
 const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {
    mode: 'development',
    devtool: 'inline-source-map',
    devServer: {
        contentBase: './dist',
        hot: true
    },
    entry: {
        main: './src/index.js',
        hello: './src/sayHello.js'
    },
    output: {
        filename: '[name].[contentHash].bundle.js',
        path: path.resolve(__dirname, 'dist'),
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    'style-loader',
                    'css-loader'
                ]
            },
            {
                test: /\.(svg|png|jpg|gif)/,
                use: 'file-loader'
            },
            {
                test: /\.(woff|woff2|eot|ttf|otf)/,
                use: 'file-loader'
            }
        ]
    },
    plugins: [
        new CleanWebpackPlugin(),
        new HtmlWebpackPlugin({
            title: 'Sample webpack app'
        })
    ]
};
 ```


