
##########
自定义扩展
##########

一个 reStructuredText 扩展就是一个 Python 模块,
首先, 需要在文档的 conf.py 中,
将扩展模块文件所在的目录添加到 :data:`sys.path` 之中.

然后, 根据扩展中定义的指令, 角色编写 ``setup`` 函数::

    def setup(app):
        app.add_directive("name", DirectiveClass)
        app.add_role("name", RoleClass)

        #....

参数 app 是由 sphinx 在调用时传递的.

.. warning::

    以下内容未完成.
    代码可能无效或出错.

自定义指令
==========

HelloWorld 扩展
---------------

定义一个指令, 需要继承 :class:`docutils.parser.rst.Directive`::

    from docutils.parser.rst import Directive

    class HelloWorld(Directive):
        pass

对于子类, 需要定义一个 ``run`` 方法::

    class HelloWorld(Directive):
        def run(self):
            pass

在 run 方法中, 返回一个 :mod:`docutils.nodes` 实例列表::

    class HelloWorld(Directive):
        def run(self):
            return [nodes.paragraph(text="Hello World!")]

以下为完整代码::

    from docutils.parser.rst import Directive
    from docutils import nodes

    class Hello(Directive):
        def run(self):
            main = nodes.paragraph(text="Hello World!")
            return [main]

    def setup(app):
        app.add_directive("hello", Hello)

然后在 rst 文档中::

    .. hello::

编译后该指令被替换为::

    Hello World!

接受参数的指令
==============

一个指令如下使用参数::

    .. 指令名:: 指令的 content
        :指令的 option:

        指令的 content

指令的 content 是除了包裹在 ``:option:`` 之外的一切内容,
包括双冒号后的输入, 以及次级缩进块中的普通文本.
