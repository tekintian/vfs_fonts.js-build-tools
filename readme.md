# 中文字体 vfs_fonts.js 

用于解决 pdfmake ， pdfHtml5， DataTables 创建pdf时中文乱码问题

目前最小的支持中文的 vfs_fonts.js  base64编码后的字体文件的创建小工具

默认集成了最小中文字体2个 粗体样式和普通样式 仅1.7M  应该是目前最小的中文字体了！！


## 使用方法
首先你需要安装有node环境，目前所有的node版本v16，14，12，，10都可以

其次将你要生成的vfs字体文件放入到 fonts文件夹中，然后运行 cnpm install && npm run build 即可
~~~sh
cnpm install
# 构建 vfs_fonts.js 
npm run build
~~~

注意 fonts文件夹下面的所有字体都将会被打包 ，如果不需要官方默认的 Roboto字体，可以删除 Roboto-XXX 文件！

## 集成到你的项目中
修改package.json文件中对应的scripts构建脚本
~~~json
"scripts": {
    "build:vfs": "node node_modules/pdfmake/build-vfs.js \"./fonts\" \"./build/vfs_fonts.js\" "
  },
~~~
build-vfs.js 存放字体的文件夹  生成的vfs文件名


## 在DataTables 中使用pdfHtml5插件时支持pdf中文

pdfmake.js 文件中搜索 var defaultClientFonts = {} 在这里面注册你生成的vfs字体， 
注册字体名称和对应字体样式对应的字体 {普通:xxx, 加粗:xxx, 斜体:xxx, 加粗斜体:xxx}， 在使用的时候直接使用这里注册的字体名称即可
~~~js
var defaultClientFonts = {
	Hyzh: {
		normal: 'hyzjh_zh.ttf',
		bold: 'hylxt_bold.ttf',
		italics: 'hyzjh_zh.ttf',
		bolditalics: 'hylxt_bold.ttf'
	},
	Roboto: {
		normal: 'Roboto-Regular.ttf',
		bold: 'Roboto-Medium.ttf',
		italics: 'Roboto-Italic.ttf',
		bolditalics: 'Roboto-MediumItalic.ttf'
	}
};
~~~

默认的配置
~~~js
var defaultClientFonts = {
	Roboto: {
		normal: 'Roboto-Regular.ttf',
		bold: 'Roboto-Medium.ttf',
		italics: 'Roboto-Italic.ttf',
		bolditalics: 'Roboto-MediumItalic.ttf'
	}
};
~~~

## pdfHtml5使用示例
~~~html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
  <meta charset="utf-8">
  <title>Pdfmake export Chinese PDF</title>
  <script src="pdfmake.min.js"></script>
  <script src="vfs_fonts.js"></script>
  <script>
  function down(data) {
            var doc = {
                content: [
                    data,
                    'Another paragraph, this time a little bit longer to make sure, this line will be divided into at least two lines'
                ],
                defaultStyle: {
                    font: 'Hyzh',
                    fontSize: 12
                }
            };
            pdfMake.fonts = {
                Roboto: {
                    normal: 'Roboto-Regular.ttf',
                    bold: 'Roboto-Medium.ttf',
                    italics: 'Roboto-Italic.ttf',
                    bolditalics: 'Roboto-Italic.ttf'
                },
                Hyzh: {
					normal: 'hyzjh_zh.ttf',
					bold: 'hylxt_bold.ttf',
					italics: 'hyzjh_zh.ttf',
					bolditalics: 'hylxt_bold.ttf'
				}
            };
            pdfMake.createPdf(doc).download();
        }
  </script>
  </head>
  <body>
  <button onclick="down('最小的支持中文显示的pdf生成工具')">下载</button>
  </body>
</html>  
~~~

更多信息
https://datatables.net/reference/button/pdfHtml5
https://datatables.club/reference/button/pdfHtml5.html


