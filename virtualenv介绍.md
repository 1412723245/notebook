## virtuakenv Python虚拟环境介绍 ##virtual
使用条件：当我们碰到我们的系统中存在多个Python环境的时候，或者是我们的项目有时候是要求是在python2下的有时候是在python3下的，在这种情况下，我们可以使用virtualenv，他可以帮助我们创建独立的环境，各个环境之间独立，切不会污染系统环境。 
### vritualenv环境安装 ###
方式：pip install virtualenv
### 创建一个virtualenv环境 ###
方式：virtualenv 名称
### 激活当前的虚拟环境 ###
source 名称/bin/activate
（其实activate是一个shell脚本，通过环境变量的相关的配置来达到隔离环）
