# package.json 说明
1. name 字段
模块名会成为模块 url、命令行中的一个参数或者一个文件夹名称，任何非 url 安全的字符在模块名中都不能使用（我们可以使用 validate-npm-package-name 包来检测模块名是否合法）；
字段不能与其他模块名重复，我们可以执行以下命令查看模块名是否已经被使用：
npm view <packageName>
如果模块存在，可以查看该模块的一些基本信息
如果该模块名从未被使用过，则会抛出 404 错误

2. version 字段
npm 包中的模块版本都需要遵循 SemVer 规范，该规范的标准版本号采用 X.Y.Z 的格式，其中 X、Y 和 Z 均为非负的整数，且禁止在数字前方补零
X 是主版本号(major)：修改了不兼容的 API
Y 是次版本号(minor)：新增了向下兼容的功能
Z 为修订号(patch)：修正了向下兼容的问题
当某个版本改动比较大、并非稳定而且可能无法满足预期的兼容性需求时，我们可能要先发布一个先行版本。
先行版本号可以加到主版本号.次版本号.修订号的后面，通过 - 号连接一连串以句点分隔的标识符和版本编译信息：

内部版本(alpha)
公测版本(beta)
正式版本的候选版本rc（即 Release candiate）


我们可以执行以下命令查看模块的版本：
npm view <packageName> version # 查看某个模块的最新版本
npm view <packageName> versions # 查看某个模块的所有历史版本

可以看到，antd 是严格按照 SemVer 规范来发版的，版本号是严格按照主版本号.次版本号.修订号的格式命名和严格递增的，在发布的版本改动较大时，还会先发布alpha、beta、rc等先行版本。

3. description 字段用于添加模块的描述信息，便于用户了解该模块。

4. keywords 字段用于给模块添加关键字。
当我们使用 npm 检索模块时，会对模块中的 description 字段和 keywords 字段进行匹配，写好 package.json中的 description 和 keywords 将有利于增加我们模块的曝光率。

5. dependencies字段指定了项目运行所依赖的模块（生产环境使用），如 antd、 react、 moment等插件库：
它们是我们生产环境所需要的依赖项，在把项目作为一个 npm 包的时候，用户安装 npm 包时只会安装 dependencies 里面的依赖。

6. devDependencies 字段指定了项目开发所需要的模块（开发环境使用），如 webpack、typescript、babel等：
在代码打包提交线上时，我们并不需要这些工具，所以我们将它放入 devDependencies 中。


# webpack 和 webpack-cli 区别 ？
webpack 从 4.0 版本开始，在安装时，就必须要安装这两个东西。webpack 是打包代码时依赖的核心内容，而 webpack-cli 是一个用来在命令行中运行 webpack 的工具。

# npx 指令是什么？
1. npx 是 npm5.2.0版本新增的一个工具包，定义为npm包的执行者，相比 npm，npx 会自动安装依赖包并执行某个命令。

2. npm自带npx，可以直接使用，如果没有可以手动安装一下：npm i -g npx

3. npx命令参数:
--no-install
--no-install 告诉npx不要自动下载，也就意味着如果本地没有该模块则无法执行后续的命令。
npx --no-install create-react-app my-react-app
--ignore-existing
--ignore-existing 告诉npx忽略本地已经存在的模块，每次都去执行下载操作，也就是每次都会下载安装临时模块并在用完后删除。
-p
-p 用于指定npx所要安装的模块，它可以指定某一个版本进行安装：
npx -p node@12.0.0 node index.js
-p 还可以用于同时安装多个模块：
npx -p lolcatjs -p cowsay [command]
-c
-c 告诉npx所有命令都用npx解释。
npx -p lolcatjs -p cowsay 'cowsay hello | lolcatjs'
这样运行会报错，因为第一项命令cowsay hello默认有npx解释，但第二项命令localcatjs会有shell解释，此时lolcatjs并没有全局安装，所有就报错了。这时候可以用-c参数来解决。
npx -p lolcatjs -p cowsay -c 'cowsay hello | lolcatjs'

4. npx 会在当前目录下的./node_modules/.bin里去查找是否有可执行的命令，没有找到的话再从全局里查找是否有安装对应的模块，全局也没有的话就会自动下载对应的模块，如上面的 create-react-app，npx 会将 create-react-app 下载到一个临时目录，用完即删，不会占用本地资源。