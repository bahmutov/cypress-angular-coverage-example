## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory. Use the `--prod` flag for a production build.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `npm run cy:open` to execute the end-to-end tests via [Cypress](https://www.cypress.io/).

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI README](https://github.com/angular/angular-cli/blob/master/README.md).

# Get code coverage on angular

- Create a new angular app using angular cli
```
ng new cypress-angular-coverage-example
```
- Install cypress-schematic to switch from protractor to cypress e2e framework
```
npm i -D  @briebug/cypress-schematic
```
- Switch to cypress from protractor
```
ng g @briebug/cypress-schematic:cypress true
```
- Install ngx-build-plus to extends the Angular CLI's build process and instrument the code
```
npm i -D ngx-build-plus
```
- Add webpack coverage config file coverage.webpack.js to cypress folder
```
module.exports = {
  module: {
    rules: [
      {
        test: /\.(js|ts)$/,
        loader: 'istanbul-instrumenter-loader',
        options: { esModules: true },
        enforce: 'post',
        include: require('path').join(__dirname, '..', 'src'),
        exclude: [
          /\.(e2e|spec)\.ts$/,
          /node_modules/,
          /(ngfactory|ngstyle)\.js/
        ]
      }
    ]
  }
};
```
- Update angular.json to use ngx-build with extra config
```
"serve": {
          "builder": "ngx-build-plus:dev-server",
          "options": {
            "browserTarget": "cypress-angular-coverage-example:build",
            "extraWebpackConfig": "./cypress/coverage.webpack.js"
          },
```
- Instrument JS files with istanbul-lib-instrument for subsequent code coverage reporting
```
npm i -D istanbul-instrumenter-loader
```
- Make Istanbul understand your Typescript source files
