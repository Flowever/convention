# 私有Specs & Pod

# 问题
团队生产了私有的公共组件，希望通过pod管理，以便于集成到具体项目中  
但是，私有pod有个问题——难以被其它pod依赖  
因此通过私有Specs解决

# 私有Specs
[http://git.flowever.net/component/Specs.git](http://git.flowever.net/component/Specs)

# 私有pod项目
目前私有pod项目包括：

1. Github（[Flowever](https://github.com/Flowever)) fork了其他项目，修改.（ 约定打tag格式 `<版本>-flow`，如 "3.1.4-flow" )
2. Gitlab私有组件

# Q&A
### 1. 私有pod存放在哪？
Github 或 Gitlab

### 2. 私有pod的.podspec是怎样的？

如 SwiftyJSON-flow.podspec  
在原来SwiftyJSON.podspec基础上，主要改写了`s.name` `s.source`

```ruby
Pod::Spec.new do |s|
  s.name         = "SwiftyJSON-flow"
  s.version      = "3.1.4"
  s.summary      = "SwiftyJSON-flow"
  s.description  = <<-DESC
  SwiftyJSON makes it easy to deal with JSON data in Swift
                   DESC

  s.homepage     = "https://github.com/Flowever/SwiftyJSON"
  s.license      = "MIT"
  s.author       = {'luosch' => 'i@lsich.com'}

  s.source       = { :git => "https://github.com/Flowever/SwiftyJSON.git", :tag => "#{s.version}-flow" }
  s.requires_arc = true
  s.osx.deployment_target = "10.9"
  s.ios.deployment_target = "8.0"
  s.watchos.deployment_target = "2.0"
  s.tvos.deployment_target = "9.0"
  s.source_files = "Source/*.swift"
  s.pod_target_xcconfig =  {
        'SWIFT_VERSION' => '3.0',
  }

end

```

### 3. podspec如何提交到Specs？
1. 首先本地添加Specs `pod repo add Specs http://git.flowever.net/component/Specs.git`
2. pod repo push Specs <pod-name>.podspec


### 4. 在Podfile如何使用私有pod？

```ruby
source 'https://github.com/CocoaPods/Specs.git'
source 'http://git.flowever.net/component/Specs.git'

target 'a target' do
  use_frameworks!
  
  pod 'SwiftyJSON-flow', '~> 3.1.4'
end
```

### 5. 是否解决了私有pod被其它pod依赖的问题？
解决了  

在.podspec依赖私有pod如下

```ruby
s.dependency 'SwiftyJSON-flow', '~> 3.1.4'
```

