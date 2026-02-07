{
  "name": "vue-admin-template",
  "version": "5.0.0",
  "description": "A vue admin template with Vue CLI 5",
  "scripts": {
    "dev": "vue-cli-service serve",
    "build:prod": "vue-cli-service build",
    "build:stage": "vue-cli-service build --mode staging",
    "preview": "node build/index.js --preview",
    "lint": "eslint --ext .js,.vue src",
    "test:unit": "vue-cli-service test:unit",
    "test:e2e": "vue-cli-service test:e2e"
  },
  "dependencies": {
    "vue": "^2.7.14",
    "vue-router": "^3.6.5",
    "vuex": "^3.6.2",
    "axios": "^1.4.0",
    "element-ui": "^2.15.13",
    "js-cookie": "^3.0.1",
    "normalize.css": "^8.0.1",
    "nprogress": "^0.2.0",
    "vue-count-to": "^1.0.13"
  },
  "devDependencies": {
    "@vue/cli-service": "^5.0.8",
    "vue-template-compiler": "^2.7.14",
    "sass": "^1.63.6",
    "sass-loader": "^13.3.2",
    "compression-webpack-plugin": "^10.0.0",
    "script-ext-html-webpack-plugin": "^2.1.5",
    "babel-plugin-component": "^1.1.1",
    "@babel/core": "^7.22.5",
    "@babel/preset-env": "^7.22.5",
    "babel-eslint": "^10.1.0",
    "@vue/cli-plugin-babel": "^5.0.8",
    "@vue/cli-plugin-eslint": "^5.0.8",
    "@vue/cli-plugin-unit-jest": "^5.0.8",
    "@vue/cli-plugin-e2e-cypress": "^5.0.8",
    "@vue/test-utils": "^1.3.3",
    "eslint": "^7.32.0",
    "eslint-plugin-vue": "^7.20.0",
    "chalk": "^4.1.2",
    "connect": "^3.7.0",
    "runjs": "^4.4.2",
    "webpack-bundle-analyzer": "^4.8.0"
  },
  "engines": {
    "node": ">=14.0.0",
    "npm": ">=6.0.0"
  },
  "browserslist": [
    "> 1%",
    "last 2 versions",
    "not dead"
  ]
}
