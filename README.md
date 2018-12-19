# Создание приватного пода


## Исходный код открыт


1) Создаем проект Cocoa Touch Framework
2) Создаем 2 репозитория github/bitbucket... (для библиотеки и для спек)
3) Создаем `podspec` файл командой `pod spec create FrameworkName`
Пример содержимого:
 ```
Pod::Spec.new do |s|

s.name         = "SixtSDK"
s.version      = "1.0.1"
s.summary      = "SDK for SIXT"
s.description  = "SDK for SIXT company"
s.homepage     = "https://www.sixt.com"
s.license      = "BSD"
s.author       = { "Alexey Rossoshansky" => "alexeyrossoshw@gmail.com" }
s.platform     = :ios, "10.0"
s.swift_version = '4.0'
s.source       = { :git => "git@bitbucket.org:rentateam/sixt-ios-sdk.git", :tag => "1.0.6" }
s.source_files  = "SixtSDK/**/*.swift"

s.dependency 'CocoaLumberjack', '~> 3.4.2'
s.dependency 'SwiftLint', '~> 0.29.1'

end
```
/**
* s.version - версия библиотеки 
* s.source - куда обращаемся за библиотекой
*/

4) Чтобы доставать библиотеку по тегу библиотеку заливаем  в репозиторий (репа с SDK)  следующим образом:

```
git commit -m "Initial commit"
git tag 1.0.6
git push -u origin master --tags
```

5) Спеку заливаем в репозиторий для спек следующим образом:

```
pod repo add name(любое) [Ссылка на репозиторий со спеками. Пр. git@bitbucket.org:rentateam/sixt-ios-sdk.git]
pod repo push name FrameworkName.podspec

```
На данном этапе осуществляется валидация. 

. Если ошибки, то решаем...
. Если варнинги, то добавлем флаг: 

```
pod repo push name FrameworkName.podspec --allow-warning
```
. Если все окей, то  можно встраивать под в новые проекты. Пример Podfile:

```
source 'https://github.com/CocoaPods/Specs.git'    <------Добавляем обязательно, когда добавлен хотя бы 1 источник
source 'git@bitbucket.org:rentateam/sixt-sdk-spec.git' <------Откуда берем спеку

target 'Sixt' do

use_frameworks!

# Pods for Sixt
pod 'M13ProgressSuite', '1.2.9'
pod 'KVOController', '1.2.0'
pod 'SixtSDK'                    <----- Наша либа  

pod 'Fabric'
pod 'Crashlytics'

end

```

## Исходный код скрыт

Все аналогично варанту с открытым исхожным кодом, но в дополнение нужно проделать следующее:
1) Билдим  Cocoa Touch Framework проект, чтобы получить `FrameworkName.framework`
2) Show in finder, находим `FrameworkName.framework` и архивируем его и заливаем в репозиторий


### Источники
1) Открытый исходный код: https://www.raywenderlich.com/5823-how-to-create-a-cocoapod-in-swift
2) Скрытый исходны код: https://medium.com/@crafttang/hide-implementation-of-swift-framework-when-distributing-35cbd39dda58

