#Config

Config is a tool that can convert the configuration information of the configuration file to  a NSDictionary or NSArray.

##SUPORT FILE TYPE
- xml
- json
- plist

##INSTALL
Download the code, copy the `Config_Lib` folder in the demo to your project


##USE


```
///ResolverClass can be nil , and it will use default resolver
[ConfigManager buildUpWithFileName:@"testBasicConfig.xml" ResolverClass:nil];

NSLog(@"%@",ConfigInfo());

```

###获取key中的值
可以通过使用符号`.`来往下层抓取数据
xml data:

```
<config>
    <ccc>
        <A>5</A>
        <cache>
            <volumne>98</volumne>
            <isTestMode>10</isTestMode>
        </cache>
        <session  import = "testBasicConfig.json"></session>
    </ccc>
</config>
```

code:

```
id data = ConfigInfo_CatchValueByKey(@"ccc.A");

```


##RESOLVER
Sometimes, we need to use a special analytical method to resolve our configuration data, so you can also use your own definition of analytical methods.
At this time, you need to create a class that inherits from the BaseResolver, and implements the `resolveWithFileName:`or`resolveWithFilePath:`to return a result you expect

```
 [ConfigManager buildUpWithFileName:@"testBasicConfig.xml" ResolverClass:[AResolver class]];
 NSLog(@"%@",ConfigInfo());
```
AResolver:

```
@implementation AResolver
+(id)resolveWithFileName:(NSString *)fileName{
    return @{@"test":@"hello world ~~~",@"fileName":fileName};
}
@end
```

##IMPORT
If one of the key corresponding to the value in the configuration file is the content of the other configuration file, then, in the different file types, the configuration rules are as follows:

### xml
example:

```
<?xml version="1.0" encoding="UTF-8" ?>
<config>
    <ccc>
        <A>5</A>
        <cache>
            <volumne>98</volumne>
            <isTestMode>10</isTestMode>
        </cache>
        <session  import = "testBasicConfig.json"></session>
    </ccc>
    <yy >111</yy>
    <bb >test</bb>
    <filter  import = "testBasicConfig.plist" resolver = "AResolver"></filter>
    <filter  import = "testBasicConfig.plist" resolver = "AResolver"></filter>
    <filter  import = "testBasicConfig.plist"></filter>
    <filter  import = "testBasicConfig.plist"></filter>
    <filter  import = "testBasicConfig.plist"></filter>
    
    <filter1>98</filter1>
    <filter1>97</filter1>
    <filter1>96</filter1>
    <filter1>95</filter1>
    
</config>
```

- the outermost layer using rulekey:config, and then fill in the contents of the configuration
- When a key value need to import another file, then you can use the `IDENTIFIER:import` (the specified file name, including file type suffix) and `IDENTIFIER:resolver` (designated resolver, fill in the name of the class)

### json & plist
example:

```
{
    "A": {
        "value": "5"
    },
    "cache": {
        "value": {
            "volumne": 98,
            "isTestMode": true
        }
    },
    "session":"$subtree/import:testBasicConfig.plist"
    ,
    "se":[
          {
          "B":"$subtree/import:testBasicConfig.plist",
          "C":"cccccc",
          "D":"dddddd"
          },
          "AAA",
          "$subtree/import:testBasicConfig.plist/resolver:TGResolver"
         ]
}

```

Roughly the same with the usage of XML is somewhat special, need to refer to another configuration file, value must have `IDENTIFIER:$subtree` and user '/' separated `file name` and `resolver name`, the file name is a must. If within the `resolver`,it will default to use the default resolver.

##Definition of IDENTIFIER
You can change your writing specifications by modifying the special flag in the ConfigConstan.h

 
##PS:
If you still do not understand, please review code.






#Config

Config 是一个工具，可以将配置文件中的配置信息到一个 NSArray 或 NSDictionary。


##支持的配置文件的文件类型
- xml
- json
- plist

##安装
下载代码，将项目中的 `Config_Lib` 文件夹复制到你的项目中

##使用


```
///ResolverClass can be nil , and it will use default resolver
[ConfigManager buildUpWithFileName:@"testBasicConfig.xml" ResolverClass:nil];

NSLog(@"%@",ConfigInfo());

```

###获取key中的值
可以通过使用符号`.`来往下层抓取数据
xml data:

```
<config>
    <ccc>
        <A>5</A>
        <cache>
            <volumne>98</volumne>
            <isTestMode>10</isTestMode>
        </cache>
        <session  import = "testBasicConfig.json"></session>
    </ccc>
</config>
```

code:

```
id data = ConfigInfo_CatchValueByKey(@"ccc.A");

```



##文件与文件的连接
如果在配置文件中的某一个key对应的value是另一个配置文件的内容，那么，在不同的文件类型中，配置的规则如下
  
### xml
例子:

```
<?xml version="1.0" encoding="UTF-8" ?>
<config>
    <ccc>
        <A>5</A>
        <cache>
            <volumne>98</volumne>
            <isTestMode>10</isTestMode>
        </cache>
        <session  import = "testBasicConfig.json"></session>
    </ccc>
    <yy >111</yy>
    <bb >test</bb>
    <filter  import = "testBasicConfig.plist" resolver = "AResolver"></filter>
    <filter  import = "testBasicConfig.plist" resolver = "AResolver"></filter>
    <filter  import = "testBasicConfig.plist"></filter>
    <filter  import = "testBasicConfig.plist"></filter>
    <filter  import = "testBasicConfig.plist"></filter>
    
    <filter1>98</filter1>
    <filter1>97</filter1>
    <filter1>96</filter1>
    <filter1>95</filter1>
    
</config>
```

- 最外层使用 rulekey:config ，然后在里面填写配置内容 
- 当一个key的value需要引入另一个文件的内容，此时，可以使用 `IDENTIFIER:import` （指定文件名，包含文件类型的后缀名）和 `IDENTIFIER:resolver`(指定resolver的类，填写类名)


### json & plist

例子：

```
{
    "A": {
        "value": "5"
    },
    "cache": {
        "value": {
            "volumne": 98,
            "isTestMode": true
        }
    },
    "session":"$subtree/import:testBasicConfig.plist"
    ,
    "se":[
          {
          "B":"$subtree/import:testBasicConfig.plist",
          "C":"cccccc",
          "D":"dddddd"
          },
          "AAA",
          "$subtree/import:testBasicConfig.plist/resolver:TGResolver"
         ]
}

```
与xml的用法大致相同，只是写法有些特殊，需要引用另一个配置文件的内容，value就必须带有 `IDENTIFIER：$subtree` 后面用/隔开 文件名称和resolver名称，`文件名是必须的，没有填写resolver的就默认使用默认的resolver`。

## 定义 IDENTIFIER
你可以在ConfigConstan.h中修改特殊标志来改变自己的写作规范

 
##PS:
如果还搞不懂，请review demo代码 
