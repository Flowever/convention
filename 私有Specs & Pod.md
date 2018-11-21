# 私有Specs & Pod

## 问题
团队生产了私有的公共组件，希望通过pod管理，以便于集成到具体项目中  
但是，私有pod有个问题——难以被其它pod依赖  
因此通过私有Specs解决

## 私有Specs
[http://git.flowever.net/component/Specs.git](http://git.flowever.net/component/Specs)

## 私有pod
目前私有pod项目包括：

1. Github（[Flowever](https://github.com/Flowever)) fork了其他项目，修改.（ 约定打tag格式 `<版本>-<comment>`，如 "3.1.4-fix-something")
2. Gitlab私有组件

## Q&A
#### 1. 私有pod存放在哪？

Github 或 Gitlab

#### 2. podspec如何提交到私有Specs？
1. 首先本地添加Specs `pod repo add Specs http://git.flowever.net/component/Specs.git`
2. `pod repo push Specs <pod-name>.podspec`


#### 3. 在Podfile如何使用私有pod？

```ruby
source 'https://github.com/CocoaPods/Specs.git'
source 'http://git.flowever.net/component/Specs.git'

target 'a target' do
  use_frameworks!
  
  pod 'SwiftyJSON-flow', '~> 3.1.4'
  # 或 pod 'SwiftyJSON-flow', :git => 'https://github.com/Flowever/SwiftyJSON.git', :tag => '3.1.4-fix-something'
  
  pod 'Common', '~> 0.1.0'
  pod 'Ad', '~> 0.1.0'
end
```

#### 4. 是否解决了私有pod被其它pod依赖的问题？
解决了  

在.podspec依赖私有pod如下

```ruby
s.dependency 'SwiftyJSON-flow', '~> 3.1.4'
```

