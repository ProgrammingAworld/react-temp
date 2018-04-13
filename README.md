## 在create-react-app 的基础上进行修改
###可以加载scss，在run build的时候可以对scss进行单独的打包

需要单独安装 sass-loader， node-sass

修改代码： 在webpack.config.dev.js中添加如下代码：
		
	 {
        test: /\.scss$/,
        use: [
          {loader: "style-loader"},
          {loader: "css-loader"},
          {loader: "sass-loader"},
          {
              loader: "postcss-loader",
              options: {
                  plugins: ()=>[require('autoprefixer')]
              }

          },
      ]
     }, {
        exclude: [/\.(js|jsx|mjs)$/, /\.html$/, /\.json$/, /\.scss$/],
        loader: require.resolve('file-loader'),
        options: {
          name: 'static/media/[name].[hash:8].[ext]',
        },
      }
      此代码用于实时编译
          
  webpack.config.prod.js文件修改的代码如下：
  
	  {
        test: /\.scss$/,
        loader: ExtractTextPlugin.extract(
          Object.assign(
            {
              fallback: {
                loader: require.resolve('style-loader'),
                options: {
                  hmr: false,
                },
              },
              use: [
                {
                  loader: require.resolve('css-loader'),
                  options: {
                    importLoaders: 1,
                    minimize: true,
                    sourceMap: shouldUseSourceMap,
                  },
                }, 
                {
                  loader: require.resolve('postcss-loader'),
                  options: {
                    ident: 'postcss',
                    plugins: () => [
                      require('postcss-flexbugs-fixes'),
                      autoprefixer({
                        browsers: [
                          '>1%',
                          'last 4 versions',
                          'Firefox ESR',
                          'not ie < 9', // React doesn't support IE8 anyway
                        ],
                        flexbox: 'no-2009',
                      }),
                    ],
                  },
                }, {
                  loader: require.resolve('sass-loader'),                      
               }
              ],
            },
            extractTextPluginOptions
          )
        )
      },
       {
            loader: require.resolve('file-loader'),
            exclude: [/\.(js|jsx|mjs)$/, /\.html$/, /\.json$/, /\.scss$/],
            options: {
              name: 'static/media/[name].[hash:8].[ext]',
            },
          }
  