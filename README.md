# ReScript Project Template

The only official ReScript starter template.

My note: Had to add some stuff to make it work properly

## Steps to create this repo

1. Tempalte cloned: `git clone https://github.com/rescript-lang/rescript-project-template`
2. Install deps: `npm i @rescript/react`
3. configure bsconfig.json
  ```json
  "bs-dependencies": ["@rescript/react"],
  "reason": { "react-jsx": 3 }
  ```
4. Setup dev dependencies: `npm i webpack webpack-cli webpack-dev-server html-webpack-plugin concurrently -D`
5. Create html template
  ```sh
  mkdir public
  touch public.index.html
  ```
  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8" />
      <title>Rescript starter</title>
  </head>
  <body>
      <div id="root"></div>
      <script src="/Index.js"></script>
  </body>
  </html>
  ```
6. Setup app root render at `src/Index.res`
  ```
  // src/App.res
  @react.component
  let make = () => {
      <div> {React.string("Hello World")} </div>
  }

  // src/Index.res
  switch(ReactDOM.querySelector("#root")) {
  | Some(root) => ReactDOM.render(<App />, root)
  | None => () // do nothing
  }

  ```
7. Setup webpack bundling
  ```js
  // webpack.config.js
  const path = require("path")
  const HtmlWebpackPlugin = require("html-webpack-plugin")
  const outputDir = path.join(__dirname, "build/")

  const isProd = process.env.NODE_ENV === "production"

  module.exports = {
    entry: "./src/Index.bs.js",
    mode: isProd ? "production" : "development",
    devtool: "source-map",
    output: {
        path: outputDir,
        filename: "Index.js"
    },
    plugins: [
        new HtmlWebpackPlugin({
        template: "public/index.html",
        inject: false
        })
    ],
    devServer: {
        compress: true,
        contentBase: outputDir,
        port: process.env.PORT || 8000,
        historyApiFallback: true
    }
  }
  ```
8. Add some package json scripts
  ```js
    "build:js:watch": "rescript build -w",
    "start:dev": "concurrently --kill-others \"npm run build:js:watch\" \"webpack serve\""
  ```
