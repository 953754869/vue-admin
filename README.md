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





const { defineConfig } = require('@vue/cli-service')
const path = require('path')
const webpack = require('webpack')
 
function resolve(dir) {
  return path.join(__dirname, dir)
}
 
module.exports = defineConfig({
  transpileDependencies: true,
  lintOnSave: process.env.NODE_ENV === 'development',
  productionSourceMap: false,
  
  // 修正后的配置
  configureWebpack: {
    resolve: {
      alias: {
        '@': resolve('src')
      }
    },
    plugins: [
      new webpack.ProvidePlugin({
        'window.Quill': 'quill/dist/quill.js',
        'Quill': 'quill/dist/quill.js'
      })
    ]
  },
  
  chainWebpack(config) {
    config.plugins.delete('preload')
    config.plugins.delete('prefetch')
    
    config.module
      .rule('svg')
      .exclude.add(resolve('src/icons'))
      .end()
    config.module
      .rule('icons')
      .test(/\.svg$/)
      .include.add(resolve('src/icons'))
      .end()
      .use('svg-sprite-loader')
      .loader('svg-sprite-loader')
      .options({
        symbolId: 'icon-[name]'
      })
      .end()
 
    config
      .when(process.env.NODE_ENV !== 'development',
        config => {
          config
            .plugin('ScriptExtHtmlWebpackPlugin')
            .after('html')
            .use('script-ext-html-webpack-plugin', [{
              inline: /runtime\..*\.js$/
            }])
            .end()
          config
            .optimization.splitChunks({
              chunks: 'all',
              cacheGroups: {
                libs: {
                  name: 'chunk-libs',
                  test: /[\\/]node_modules[\\/]/,
                  priority: 10,
                  chunks: 'initial'
                },
                elementUI: {
                  name: 'chunk-elementUI',
                  priority: 20,
                  test: /[\\/]node_modules[\\/]_?element-ui(.*)/
                },
                commons: {
                  name: 'chunk-commons',
                  test: resolve('src/components'),
                  minChunks: 3,
                  priority: 5,
                  reuseExistingChunk: true
                }
              }
            })
          config.optimization.runtimeChunk('single')
        }
      )
  },
  
  // 修正后的 devServer 配置
  devServer: {
    port: 9527,
    open: true,
    hot: true,
    client: {
      overlay: {
        warnings: false,
        errors: true,
        progress: true // 如果需要进度显示，可以添加以下配置
      }
    },
    proxy: {
      '/api': {
        target: 'http://localhost:8080',
        changeOrigin: true,
        pathRewrite: {
          '^/api': ''
        }
      }
    }
  }
})










module.exports = {
  presets: [
    '@vue/cli-plugin-babel/preset'
  ],
  plugins: [
    [
      'component',
      {
        libraryName: 'element-ui',
        styleLibraryName: 'theme-chalk'
      }
    ]
  ]
}






NODE_ENV=development
VUE_APP_BASE_API=/api
VUE_APP_BASE_URL=http://localhost:9527


NODE_ENV=production
VUE_APP_BASE_API=/api
VUE_APP_BASE_URL=https://production-domain.com

npm install path-browserify -D
npm install crypto-browserify -D
npm install stream-browserify -D
npm install buffer -D
npm install process -D


const { defineConfig } = require('@vue/cli-service')
const path = require('path')
const webpack = require('webpack')
 
function resolve(dir) {
  return path.join(__dirname, dir)
}
 
module.exports = defineConfig({
  transpileDependencies: true,
  lintOnSave: process.env.NODE_ENV === 'development',
  productionSourceMap: false,
  
  configureWebpack: {
    resolve: {
      alias: {
        '@': resolve('src'),
        // 添加其他常用别名
        'components': resolve('src/components'),
        'views': resolve('src/views'),
        'utils': resolve('src/utils'),
        'api': resolve('src/api')
      },
      // 添加 polyfill 配置
      fallback: {
        "path": require.resolve("path-browserify"),
        "crypto": require.resolve("crypto-browserify"),
        "stream": require.resolve("stream-browserify"),
        "buffer": require.resolve("buffer/"),
        "util": require.resolve("util/"),
        "process": require.resolve("process/browser")
      }
    },
    plugins: [
      new webpack.ProvidePlugin({
        'window.Quill': 'quill/dist/quill.js',
        'Quill': 'quill/dist/quill.js',
        // 提供全局的 process 和 Buffer
        process: 'process/browser',
        Buffer: ['buffer', 'Buffer']
      })
    ]
  },
  
  chainWebpack(config) {
    config.plugins.delete('preload')
    config.plugins.delete('prefetch')
    
    config.module
      .rule('svg')
      .exclude.add(resolve('src/icons'))
      .end()
    config.module
      .rule('icons')
      .test(/\.svg$/)
      .include.add(resolve('src/icons'))
      .end()
      .use('svg-sprite-loader')
      .loader('svg-sprite-loader')
      .options({
        symbolId: 'icon-[name]'
      })
      .end()
 
    config
      .when(process.env.NODE_ENV !== 'development',
        config => {
          config
            .plugin('ScriptExtHtmlWebpackPlugin')
            .after('html')
            .use('script-ext-html-webpack-plugin', [{
              inline: /runtime\..*\.js$/
            }])
            .end()
          config
            .optimization.splitChunks({
              chunks: 'all',
              cacheGroups: {
                libs: {
                  name: 'chunk-libs',
                  test: /[\\/]node_modules[\\/]/,
                  priority: 10,
                  chunks: 'initial'
                },
                elementUI: {
                  name: 'chunk-elementUI',
                  priority: 20,
                  test: /[\\/]node_modules[\\/]_?element-ui(.*)/
                },
                commons: {
                  name: 'chunk-commons',
                  test: resolve('src/components'),
                  minChunks: 3,
                  priority: 5,
                  reuseExistingChunk: true
                }
              }
            })
          config.optimization.runtimeChunk('single')
        }
      )
  },
  
  devServer: {
    port: 9527,
    open: true,
    hot: true,
    client: {
      overlay: {
        warnings: false,
        errors: true
      }
    },
    proxy: {
      '/api': {
        target: 'http://localhost:8080',
        changeOrigin: true,
        pathRewrite: {
          '^/api': ''
        }
      }
    }
  }
})


const path = require('path')


