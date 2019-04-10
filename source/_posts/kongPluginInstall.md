title: Kong Plugin安装的几种方式
date: 2018-08-06 15:11:06
tags: Kong
---

众所周知，GateWay强大的一面在于可以进行限流，熔断，鉴权等一系列的处理工作，Kong也不例外，对于Kong来说，主要的功能都以Plugin的形式暴露给开发者，社区版的Kong自身带有一些列的Plugin，如果是企业版自带的则支持更多。 但这些相对众多的开发需求，肯定是不能一一满足，也就涉及到一个定制化plugin的问题，定制化Plugin在之后会讲到，这一篇文章主要讲述一些Plugin的安装方式。
<!--more-->

首先，是我认为最方便的安装方式，一般linux机器上都会自带 luarocks（lua包管理程序），这样一来我们只要把 Plugins 所在的文件夹给移动到服务器的任意目录，然后在该目录下，执行`luarocks make` 这样一来插件便会自动安装到系统中，不过需要注意的是，此时插件还需要进行手动开启，`首先进入/etc/kong/目录，然后cp kong.conf.default kong.conf， 这里注意一定要复制一份单独的kong.conf文件，不能直接对kong.conf.default进行修改，这样是不生效的，然后关闭plugin = bundled注释，在末尾增加你的插件名，这里注意插件名是不包含前缀 kong-plugin的，重启Kong即可在可视化界面里发现`

还有一种方式是`手动安装`如果不是对Lua环境要求需求特别高，可以不用折腾。 首先一样，把 Plugin 项目文件放到服务器上，然后自定义你的插件目录，比如`/usr/local/custom/kong/plugins`，这里注意`/kong/plugins`之前的可以自定义，但是末尾的`/kong/plugins`是固定的。然后把插件移到`../kong/plugins/`内，这里切记插件的项目名称得去掉`kong-plugin`, 然后和上述一样，修改`kong.conf`文件中的`lua_package_path`, 如按例子中的path则填写为`/usr/local/custom/?.lua;;`，同样重启Kong之后就可以选择自己所写的Plugin了

