---
layout: post
title: Axis2 Web服务配置文件services.xml详解
date: 2013-09-14 01:00:00 +0800
description: ""
tags: [Axis2, Web Service]
---


#### 简介

在Axis1中部署服务时，我们使用service.wsdd文件来配置服务。在Axis2中，不再使用service.wsdd文件来配置服务，改用services.xml了。这两个配置文件的语法是截然不同的。

本文涵盖了services.xml文件的语法和使用说明。在Apache Axis2/Java中，同一个服务包文件既可以用于部署单个服务，也可以部署多个服务。不论以何种方式部署服务，一个有效的服务包文件必须包含services.xml文件。随着我们部署服务的方式不同，services.xml文件的语法也不同。Services.xml文件主要有两种：一种用于部署单个服务，一种用于部署服务组。

##### 编写用于部署单个服务的services.xml文件

用于部署单个服务的services.xml文件的根节点是service,整个文件看起来就像这样：



{% highlight xml %}
<service>
...
</service>
{% endhighlight %}

##### 服务名

我们用服务包部署单个服务时，如果我们没有给service节点指定name属性，那么服务包文件名称就是服务名称。例如假设服务包文件名是foo.aar，那么服务名就是foo。我们也可以给service节点添加name属性来指定不同的服务名称。如下所述：

{% highlight xml %}
<service name="foo">
...
</service>
{% endhighlight %}

##### 服务描述

服务编写者可以使用description元素来描述该服务。在Axis2 Web管理控制台中查看服务时，我们只能看到服务名和服务描述。如果我们不给services.xml文件添加description元素，则服务描述栏会显示服务名称。对于那些访问该服务的用户来说，服务描述是非常有用的。添加服务描述信息非常简单，给services.xml文件添加一个可选的description节点就可以了。该节点的值既可以是纯文本，也可以是HTML代码片段。

{% highlight xml linenos %}
<service>
 <description>
  计算矩形面积
 </description>
...
</service>
{% endhighlight %}

也可以写成这样：

{% highlight xml linenos %}
<service>
 <description>
  <b>计算矩形面积</b>
 </description>
...
</service>
{% endhighlight %}

*注：description节点是可选节点*

##### 服务级参数

在services.xml文件中，我们可以直接在service节点下定义参数，这些参数供消息上下文(在运行时)、AxisService或者AxisOperation访问。参数有一个必选参数和可选参数：参数名称是必选参数，locked属性是可选参数。

locked属性指明了是否允许参数值被子节点覆盖。举例来说，如果我们在axis2.xml文件中添加了一个locked属性值为true的参数，那么如果服务试图在services.xml文件中定义同名参数，是会抛出异常的。

举例来说，假设axis2.xml文件中定义了一个名叫foo的参数，该参数locked属性为true,而services.xml文件也有一个同名参数，那么部署时会抛出异常。如果参数的locked属性为true，则不能在子节点中覆盖父节点中定义的参数。

参数值可以是任何东西，它既可以是纯文本也可以是XML片段。要添加服务级参数，请参考下文：

{% highlight xml linenos %}
<service>
.....
 <parameter name="location">
  Colombo , Sri Lanka
 </parameter>
.....
</service>
{% endhighlight %}

##### 服务类

在Axis2中，Web服务并不强制要求指定服务实现类！Axis2架构允许编写没有服务实现类的Web服务，这是因为消息接收器允许这样做。Axis2中，一旦请求交予消息接收器处理，Axis2引擎就认为自己的事情做完了，剩余工作都是消息接收器的了。因此，services.xml并不强制要求提供服务实现类。但是绝大多数情况下，我们还是需要服务类的，我们可以在services.xml文件中添加ServiceClass参数来指定服务类。该参数的值是服务类的全路径名。示例如下：

{% highlight xml %}
<parameter name="ServiceClass" locked="false">
 org.apache.Foo
</parameter>
{% endhighlight %}

##### 服务级消息接收器

Axis2中消息接收器是特殊的处理器，是In路径（请求路径）中的最后一个处理器。Web服务中的每个操作都有他自己的消息接收器，而且不同的操作可以有不同的消息接收器。消息接收器是依赖于消息交换模式的,所以我们必须为不同的消息交换模式指定不同的消息接收器。

怎样才能给所有的操作指定相同的消息接收器呢？只要添加服务级消息接收器即可。如此我们就不必在操作级别指定消息接收器了。我们要做的是指定服务级消息接收器。而在部署时，Axis2会自动给操作选择正确的消息接收器。

{% highlight xml linenos %}
<messagereceivers>
 <messagereceiver mep="http://www.w3.org/2004/08/wsdl/in-only" class="org.apache.axis2.rpc.receivers.RPCInOnlyMessageReceiver" />
 <messagereceiver mep="http://www.w3.org/2004/08/wsdl/in-out" class="org.apache.axis2.rpc.receivers.RPCMessageReceiver" />
</messagereceivers>
{% endhighlight %}

##### 使用模块

在一些情况下，如果不使用WS-Security模块，我们就不应该运行该服务。这时我们只需往services.xml文件中添加module标签，就能使用该模块了。需要注意的是，如果模块不可用会导致服务变成有错误的服务而无法使用。

{% highlight xml linenos %}
<service>
 <module ref="foo/">
 </module>
</service>
{% endhighlight %}

##### 服务会话范围

Axis2中Web服务有四种会话范围。如果不指定，则默认为 request
会话范围。我们可以通过给 service 节点添加一个可选的
scope参数来指定会话范围，会话范围共有如下四种：

0.  application : 应用级别。生命周期和Axis2引擎生命周期相同。
0.  soapsession : 使用addressing headers中的自定义引用属性来管理会话。
0.  transportsession : 使用transport
    cookies来管理会话，生命周期和底层的transport相同。
0.  request :生命周期很短，和请求处理周期相同。

{% highlight xml %}
<service scope="application">
...
</service>
{% endhighlight %}

##### Service目标名字空间

服务目标名字空间仅仅在WSDL生成过程中起作用。在运行时，如果有人试图使用?wsdl来查看WSDL，那么生成的WSDL文件中的目标名字空间就是services.xml文件中指定的值。同时，我们为了使用自定义的WSDL文件，把WSDL文件放到META-INF目录中，这种情况下要覆盖原有的目标名字空间，也是通过往services.xml文件中添加目标名字空间来实现的。目标名字空间的缺省值是http://ws.apache.org/axis2。

要指定目标名字空间，我们需要给service节点添加可选的targetNamespace属性，示例如下：

{% highlight xml %}
<service targetnamespace="http://foo.org">
...
</service>
{% endhighlight %}

##### Schema目标名字空间

当生成WSDL文件（运行时）或者生成schema（部署时）的时候，如果在META-INF目录中找不到WSDL文件，则可以通过往services.xml文件中添加schema节点来指定自定义的schema目标名字空间。

在部署的时候，如果不指定schema目标名字空间，则根据服务实现类的全路径名来生成目标名字空间。例如，如果服务类的全路径名是org.apache.axis2.FooService，那生成的schema名字空间是http://FooService.axis2.apache.org/xsd

如果你想使用自定义值，只需要在services.xml文件中添加下述节点即可。

{% highlight xml linenos %}
<service>
 <schema schemanamespace="http://foo.org/xsd/">
 </schema>
</service>
{% endhighlight %}

##### WSDL中的elementFormDefault值

在使用Java类生成WSDL文件时，WSDL文件中schema定义中的elementFormDefault默认是设置为qualified。如果qualified为true，则响应消息中的所有元素都是受限的。但是有些时候我们并不需要这种行为，我们希望把elementFormDefault设置为unqualified。这时我们只需要在services.xml文件中添加下述条目即可。

{% highlight xml %}
<schema elementformdefaultqualified="false" />
{% endhighlight %}

*注：我们还可以同时指定schemaNamespace属性。*

##### 在指定的传输通道上暴露服务

Axis2可以在多种传输通道上暴露服务，这是通过Lister
Manager完成的。例如，使用Listener Manager你可以在HTTP和TCP上暴露服务。

由于Axis2支持多种传输通道，所以我们可以在选定的传输通道上暴露服务。比如说系统管理服务，我们希望该服务只在支持SSL的通道上暴露，这样才能保证系统管理的安全性。

当服务端支持多种传输通道时，所有的服务都是在所有的通道上暴露的。如果只希望在选定的通道上暴露服务，我们需要在services.xml文件中添加transport标签，如下所述：

{% highlight xml linenos %}
<transports>
 <transport>
  https
 </transport>
</transports>
{% endhighlight %}

操作和操作相关元素
------------------

上一节描述了会影响服务中所有操作的服务级配置。例如，我们在服务级使用模块，这是会影响服务中所有操作的。但是有时我们希望在操作中覆写这些配置或者添加新配置。

*注：如果服务实现类是用Java编写的，那么服务类中的所有public方法，默认都是要暴露出去的。如果服务类使用其它语言编写的，那么我们必须指定要发布的操作，否则默认是不暴露出去的。*

##### 覆写操作

上面提到了我们想在操作中覆写服务级配置。要覆写服务级参数，我们需要添加operation标签：

{% highlight xml linenos %}
<service>
 <parameter name="location" locked="false">
  Colombo , Sri Lanka 
 </parameter>
 <operation name="doSmt">
  <parameter name="location" locked="false">
   California ,USA
  </parameter>
 </operation>
</service>
{% endhighlight %}

##### Operation 级消息接收器

前文描述了如何指定服务级消息接收器。但是，我们也可以为不同的操作指定不同的消息接收器，这需要在operation中指定messageReceiver标签

{% highlight xml %}
<operation name="doSmt">
 <messagereceiver class="org.apache.axis2.MyMessageReceiver" />
</operation>
{% endhighlight %}

##### 添加actionMapping

actionMapping相当于操作的别名。我们可以为操作添加任意数量的别名。我们可以根据ActionMapping来过滤请求，也可以为不同的action
mapping
执行不同的处理逻辑。客户端请求消息中通过指定SOAPAction或者wsa:action来发送action
mapping。这样Axis2的分发器就可以把请求消息分发给正确的操作。

{% highlight xml linenos %}
<operation name="doSmt">
 <actionmapping>
  mapping1
 </actionmapping>
 <actionmapping>
  http://foo.org/doSmt
 </actionmapping>
</operation>
{% endhighlight %}

##### 使用模块

就像我们在服务中使用模块，我们也可以在操作中使用模块。举例来说，如果我们只希望在指定的操作上提供安全访问，最好的方法是在这些操作中使用模块。

要使用模块，只需要在operation元素中添加module元素即可。

{% highlight xml %}
<operation name="doSmt">
 <module ref="foo" />
</operation>
{% endhighlight %}

##### 排除操作

Axis2默认会暴露实现类中的所有public方法（如果是用Java实现的）。如果我们把.wsdl文件放到META-INF目录中，而由不希望发布文件中的所有操作，那么我们可以添加excludeOperations标签来排除那些不希望暴露的操作。

{% highlight xml linenos %}
<excludeoperations>
 <operation>
  op1
 </operation>
</excludeoperations>
{% endhighlight %}

#### 编写用于部署服务组的services.xml文件

要在单个服务包文件中部署多个服务，服务组是一个便捷方法。当然，这些服务之间应该存在逻辑关系。用于服务组的services.xml文件和用于单个服务的，它们之间唯一的区别就是根元素。用于服务组的，根元素是serviceGroup，我们可以在serviceGroup元素内部定义多个service元素。

{% highlight xml linenos %}
<servicegroup>
 <service name="service1">
...
  <service>
   <service name="service2">
...
   </service>
  </service>
 </service>
</servicegroup>
{% endhighlight %}

*注：在这种情况下，服务包文件名成了服务组的名称。Service元素的name属性成了必选属性，而且在整个服务器中必须保证唯一。系统不允许两个服务同名。*

一旦我们了解了如何编写用于单个服务的services.xml文件，那编写用于服务组的services.xml文件就很容易了。就像上面提到的一样，服务组只是服务元素的集合，前面提到的所有语法在服务组中是一样的。

