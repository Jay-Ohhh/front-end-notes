# iOS Developer roadmap

https://roadmap.sh/ios



# 环境配置

1. xcode

2. ```bash
   brew install cocoapods
   pod setup
   pod --version
   如果 pod search 不到东西，运行：pod update
   https://cocoapods.org
   ```

3. ```bash
   # 如果 clash verge 设置了虚拟网卡（TUN Mode）可以不用设置代理的环境变量
   export http_proxy=127.0.0.1:7890 && export https_proxy=127.0.0.1:7890 && pod install
   ```
   
4. ```bash
   # xcode 默认不走系统代理
   git config --global http.https://github.com.proxy socks5://127.0.0.1:7890
   ```

5. 模拟器如果报错：

   SDK does not contain 'libarclite' at the path '/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/arc/libarclite_iphonesimulator.a'; try increasing the minimum deployment target

   参考：https://github.com/yuehuig/libarclite

6. lookin 配置（可选）

   ```
   # Uncomment the next line to define a global platform for your project
   platform :ios, '16.0'
   
   target 'demo' do
     # Comment the next line if you don't want to use dynamic frameworks
     use_frameworks!
     
     # Pods for demo
   
     target 'demoTests' do
       inherit! :search_paths
       # Pods for testing
     end
   
     target 'demoUITests' do
       # Pods for testing
     end
     
     # see this line
     pod 'LookinServer', :configurations => ['Debug']
     
   end
   ```
   
   xcode -> General -> Minimum Deployments -> ios 16.0
   
   如果 lookin 运行不了
   
   xcode -> build settings -> all -> 搜索 User Script Sandboxing -> 设置为 no (放开权限)



# 证书文件

iOS 开发相关的描述文件（Provisioning Profile）和证书文件：

1. `IwForDeveloper.mobileprovision` - 这是一个开发用的描述文件
2. `lw_ad_hoc.p12` - 这是一个 Ad Hoc 发布证书文件
3. `lw_dev.p12` - 这是一个开发证书文件
4. `IwForDeveloperAdHoc.mobileprovision` - 这是一个 Ad Hoc 发布用的描述文件



这些是 iOS 开发相关的描述文件（Provisioning Profile）和证书文件：

1. `IwForDeveloper.mobileprovision` - 这是一个开发用的描述文件
2. `lw_ad_hoc.p12` - 这是一个 Ad Hoc 发布证书文件
3. `lw_dev.p12` - 这是一个开发证书文件
4. `IwForDeveloperAdHoc.mobileprovision` - 这是一个 Ad Hoc 发布用的描述文件

这些文件在 iOS 开发中用于：

- 签名和部署应用
- 标识开发者身份
- 允许应用安装在特定设备上
- 管理应用的权限和功能

其中：

- `.p12` 文件是证书文件
- `.mobileprovision` 文件是描述文件，包含了应用的配置信息、设备列表等
- adhoc 用于内部测试分发
- development 用于开发调试



安装完证书后，如果 xcode 显示 No signing certificate，可以尝试 quit xcode，再重新启动。



## 如何获取

这些文件需要通过 Apple Developer 账号在 Apple Developer Portal (开发者网站) 上获取，具体步骤如下：

1. 证书文件 (.p12)：
- 登录 Apple Developer 网站
- 在 "Certificates, Identifiers & Profiles" 中创建证书
- 使用本地 Keychain Access 生成 CSR (Certificate Signing Request) 文件
- 上传 CSR 到开发者网站
- 下载生成的证书
- 从 Keychain Access 导出为 .p12 格式

2. 描述文件 (.mobileprovision)：
- 同样在 Apple Developer 网站
- 先创建 App ID
- 添加需要调试的设备 UDID
- 创建 Provisioning Profile，选择：
  * 对应的 App ID
  * 证书
  * 设备列表
- 生成并下载 .mobileprovision 文件

注意事项：
- 需要付费的开发者账号才能创建这些文件
- 证书有效期通常为一年
- 不同类型的描述文件对应不同的使用场景



# 项目入口

UIKit: SceneDelegate

SwiftUI: WindowGroup



# UIViewController Life Cycle in iOS

1. init()
2. loadView()
3. viewDidLoad()
4. viewWillAppear()
5. viewIsAppearing()
6. viewWillLayoutSubviews()
7. viewDidLayoutSubviews()
8. viewDidAppear()
9. viewWillTransition()
10. viewWillDisappear()
11. viewDidDisappear()
12. deinit()
13. didReceiveMemoryWarning()

## init()

`init()`: This is the designated initializer for UIViewController. It's called when the view controller is being initialized. You typically override this method to perform custom initialization for your view controller. It's important to call the superclass's designated initializer (`super.init(nibName:bundle:)`) within your custom `init()` implementation.

## loadView()

`loadView()`: This method is responsible for creating or loading the view hierarchy of the view controller. You override this method when you want to create your view hierarchy programmatically without using a storyboard or a nib file. If you don't override this method, the view controller will automatically load its view from a nib or storyboard. If you want to perform any additional initialization of your views, do so in the `viewDidLoad()` method. You should never call this method directly. The view controller calls this method when its `view` property is requested but is currently `nil`. This method loads or creates a view and assigns it to the `view` property.

## viewDidLoad()

`viewDidLoad()`: This method is called after the view controller has loaded its view hierarchy into memory. This method is called regardless of whether the view hierarchy was loaded from a nib file or created programmatically in the `loadView()` method. You usually override this method to perform additional initialization on views that were loaded from nib files. It’s a good place to perform one-time setup tasks for your view controller, such as initializing data, setting up UI elements, or loading data from external sources. **This method is called only once during the view controller’s lifecycle.**

## viewWillAppear()

`viewDidLoad()`: This method is called before the view controller’s view is about to be added to a view hierarchy and before any animations are configured for showing the view. You can override this method to perform custom tasks associated with displaying the view. It’s often used to perform tasks that need to happen every time the view is shown, such as updating UI elements or refreshing data. If you override this method, you must call `super` at some point in your implementation.

## viewIsAppearing()

`viewIsAppearing()`: The system calls this method once each time a view controller’s view appears after the `viewWillAppear(_:)` call. In contrast to `viewWillAppear(_:)`, the system calls this method after it adds the view controller’s view to the view hierarchy, and the superview lays out the view controller’s view. By the time the system calls this method, both the view controller and its view have received updated trait collections and the view has accurate geometry.

You can override this method to perform custom tasks associated with displaying the view. For example, you might use this method to configure or update views based on the trait collections of the view or view controller. Or, because computing a scroll position relies on the view’s size and geometry, you might programmatically scroll a collection or table view to ensure a selected cell is visible when the view appears.

If you override this method, you need to call `super` at some point in your implementation. You can see the below image and figure out where it is called and so on.

## viewWillLayoutSubviews()

`viewWillLayoutSubviews()`: This method is called just before the view controller's view is about to update its layout to fit the current device's orientation or size. You can override this method to make adjustments to your view's layout before it occurs. For example, you can override this method to fix issues when the user changes orientation to landscape and gets back because often occurs issues when the users change application orientation.

## viewDidLayoutSubViews()

`viewDidLayoutSubviews()`: This method is called after the view controller's view has finished updating its layout. You can use this method to perform additional layout-related tasks or animations that depend on the final layout of your view.

## viewDidAppear()

`viewDidAppear()`: This method is called when the view has appeared on the screen. It's often used to start animations, load additional data, or perform other tasks that should happen after the view is fully visible.

## viewWillDisappear()

`viewWillDisappear()`: This method is called just before the view is about to disappear from the screen, typically when you navigate away from the current view controller. It's often used to perform cleanup tasks, save data, or pause ongoing processes that are specific to the current view. For example, you might stop media playback or invalidate timers when the view is about to disappear.

## viewDidDisappear()

`viewDidDisappear()`: This method is called after the view has disappeared from the screen. It's often used to perform additional cleanup tasks or to release resources that were allocated when the view was visible. For example, you might release memory-intensive resources, unregister from notifications, or stop observing changes.

## viewWillTransition()

`viewWillTransition()`: This method is called when the view controller’s trait collection is changed by its parent. UIKit calls this method before changing the size of a presented view controller’s view. You can override this method in your own objects and use it to perform additional tasks related to the size change. For example, a container view controller might use this method to override the traits of its embedded child view controllers. Use the provided `coordinator` object to animate any changes you make.

If you override this method, you should either call super to propagate the change to children or manually forward the change to children.

## deinit()

`deinit()`: This method is automatically called when an instance of a class is being deallocated or destroyed. It's the counterpart to the `init()` method, which is called when an instance is being initialized. You can use the `deinit()` method to perform cleanup tasks, release resources, or unregister from observers when the object is no longer needed.

For view controllers, `deinit()` can be useful for cleaning up any resources that were allocated during the view controller's lifecycle but are no longer needed. For example, you might use `deinit()` to release memory, close network connections, or perform any other necessary cleanup before the view controller is removed from memory.



## didReceiveMemoryWarning()

- iOS devices have a limited amount of memory and power. When the memory starts to fill up, iOS does not use its limited hard disk space to move data out of the memory like a computer does. For this reason you are responsible to keep the memory footprint of your app low. If your app starts using too much memory, iOS will notify it.
- Since view controllers perform resource management, these notifications are delivered to them through this method. In this way you can take actions to free some memory. Keep in mind that if you ignore memory warnings and the memory used by your app goes over a certain threshold, iOS will end your app. This will look like a crash to the user and should be avoided.



```swift
final class ViewController: UIViewController {
    
    deinit {
        print("deinit")
    }
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        print("didReceiveMemoryWarning")
    }
    
    override func viewWillTransition(to size: CGSize, with coordinator: UIViewControllerTransitionCoordinator) {
        super.viewWillTransition(to: size, with: coordinator)
        print("viewWillTransition")
    }
    
    override func viewIsAppearing(_ animated: Bool) {
        super.viewIsAppearing(animated)
        print("viewIsAppearing")
    }
    
    override func viewDidLayoutSubviews() {
        super.viewDidLayoutSubviews()
        print("viewDidLayoutSubviews")
    }
    
    override func viewWillLayoutSubviews() {
        super.viewWillLayoutSubviews()
        print("viewWillLayoutSubviews")
    }
    
    override func viewDidDisappear(_ animated: Bool) {
        super.viewDidDisappear(animated)
        print("viewDidDisappear")
    }
    
    override func viewWillDisappear(_ animated: Bool) {
        super.viewWillDisappear(animated)
        print("viewWillDisappear")
    }
    
    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)
        print("viewDidAppear")
    }
    
    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        print("viewWillAppear")
    }
    
    override func loadView() {
        super.loadView()
        print("loadView")
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        print("viewDidLoad")
    }
}
```



# SwiftUI State Management

让我详细解释 SwiftUI 的状态管理：

SwiftUI 的状态管理主要通过不同的属性包装器（Property Wrapper）来实现：

## @State

- 用于管理视图的本地状态
- 适用于简单的数据类型（如 Int, String 等）
- 当值改变时会自动触发视图刷新

```swift
struct ContentView: View {
    @State private var count = 0
    
    var body: some View {
        Button("Tap \(count)") {
            count += 1
        }
    }
}
```

## @Binding

- 用于在不同视图间共享状态
- 创建对其他状态的引用

```swift
// demo 1
struct ChildView: View {
    @Binding var count: Int
    
    var body: some View {
        Button("Increment") {
            count += 1
        }
    }
}

// demo 2
// 父视图
struct ParentView: View {
    @State private var isOn = false
    
    var body: some View {
        VStack {
            Text("Is On: \(isOn ? "Yes" : "No")")
            ChildView(isOn: $isOn)  // 注意这里使用 $
        }
    }
}

// 子视图
struct ChildView: View {
    @Binding var isOn: Bool  // 接收绑定
    
    var body: some View {
        // Toggle 内置开关组件 
        Toggle("Switch", isOn: $isOn)
    }
}
```

## @ObservedObject

- 用于观察外部引用类型（class）的状态变化
- 适用于遵循 ObservableObject 协议的对象

```swift
class UserData: ObservableObject {
    @Published var name = "John"
}

struct ProfileView: View {
    @ObservedObject var userData: UserData
    
    var body: some View {
        Text(userData.name)
    }
}
```

## @EnvironmentObject

- 用于在视图层次结构中注入依赖
- 可以在多个视图间共享数据

```swift
struct ContentView: View {
    var body: some View {
        ChildView()
            .environmentObject(UserData())
    }
}

struct ChildView: View {
    @EnvironmentObject var userData: UserData
    
    var body: some View {
        Text(userData.name)
    }
}
```

## @StateObject

- 类似于 @ObservedObject，但由 SwiftUI 管理生命周期
- 确保对象只被创建一次

```swift
struct ContentView: View {
    @StateObject private var userData = UserData()
    
    var body: some View {
        Text(userData.name)
    }
}
```



# Delegate

基本概念：

1. 定义协议（Protocol）声明方法和属性
2. 委托者（Delegator）持有代理对象
3. 代理（Delegate）实现协议定义的方法
4. 实现对象间松耦合的通信

基础示例：

```swift
// 1. 定义协议
protocol MyDelegate: AnyObject {
    func didTapButton()
    func didUpdateValue(_ value: String)
}

// 2. 委托者
class MyView {
    // 使用 weak 避免循环引用
    weak var delegate: MyDelegate?
    
    func buttonTapped() {
        delegate?.didTapButton()
    }
    
    func updateValue(_ value: String) {
        delegate?.didUpdateValue(value)
    }
}

// 3. 代理实现
class ViewController: UIViewController, MyDelegate {
    let myView = MyView()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        myView.delegate = self
    }
    
    // 实现代理方法
    func didTapButton() {
        print("Button tapped")
    }
    
    func didUpdateValue(_ value: String) {
        print("Value updated: \(value)")
    }
}
```



# Swift UI/UIKit 教程 + demo

https://goswiftui.com/uikit-equivalent-in-swiftui/

https://gitee.com/guangjie2021/SwiftUICn

https://developer.apple.com/tutorials/swiftui/creating-and-combining-views



# UIView vs UIViewController

UIView:

- 显示内容
- 处理触摸事件
- 管理子视图
- 渲染图形



UIViewController:

- 管理视图层次
- 处理视图生命周期
- 响应设备旋转
- 协调数据和视图
- 处理内存警告



```swift
// UIView：负责 UI 的显示和交互
class UIView {
    var frame: CGRect
    var bounds: CGRect
    var superview: UIView?
    var subviews: [UIView]
}

// UIViewController：负责视图的生命周期和业务逻辑
class UIViewController {
    var view: UIView
    var navigationController: UINavigationController?
    var presentedViewController: UIViewController?
}
```



UIViewRepresentable: 用于包装 UIView protocol UIViewRepresentable

UIViewControllerRepresentable: 用于包装 UIViewController protocol 

```swift
// UIViewRepresentable 示例
struct CustomViewRepresentable: UIViewRepresentable {
    // 1. 创建协调器
  	// makeCoordinator 方法会被调用。这个方法的目的是创建一个协调器（coordinator）对象，该对象通常用于处理 UIKit 视图中的事件或回调
  	// makeCoordinator 方法在 UIViewRepresentable 视图实例的生命周期中只会被调用一次（除非视图被重新创建，例如，由于内存回收和重新加载）
    // 一旦协调器对象被创建，它就会通过 Context 参数与 UIViewRepresentable 视图保持关联，直到视图被销毁
    func makeCoordinator() -> Coordinator {
        print("1. makeCoordinator")
        return Coordinator()
    }
    
    // 2. 创建视图，required
  	// 在 UIViewRepresentable 视图准备将其内部的 UIKit 视图添加到视图层次结构之前，makeUIView 方法会被调用。这个方法负责创建并返回一个 UIKit 视图实例
		// 在 makeUIView 中，你可能会设置 UIKit 视图的一些属性或事件监听器，这些监听器可以指向协调器中的方法。这样，当 UIKit 视图中的事件发生时（如按钮点击），相应的协调器方法就会被调用
    func makeUIView(context: Context) -> UIView {
        print("2. makeUIView")
        let view = UIView()
        return view
    }
    
    // 3. 更新视图，required
  	// 在 UIViewRepresentable 视图的生命周期中，如果其状态发生变化，并且这些变化需要反映在其内部的 UIKit 视图上，那么 updateUIView 方法可能会被调用。这个方法允许你根据 UIViewRepresentable 视图的状态更新其内部的 UIKit 视图
    func updateUIView(_ uiView: UIView, context: Context) {
        print("3. updateUIView")
    }
    
    // 4. 销毁视图
    func dismantleUIView(_ uiView: UIView, coordinator: Coordinator) {
        print("4. dismantleUIView")
    }
}

// UIViewControllerRepresentable 示例
struct CustomViewControllerRepresentable: UIViewControllerRepresentable {
    // 1. 创建协调器
    func makeCoordinator() -> Coordinator {
        print("1. makeCoordinator")
        return Coordinator()
    }
    
    // 2. 创建视图控制器
    func makeUIViewController(context: Context) -> UIViewController {
        print("2. makeUIViewController")
        return UIViewController()
    }
    
    // 3. 更新视图控制器
    func updateUIViewController(_ uiViewController: UIViewController, context: Context) {
        print("3. updateUIViewController")
    }
    
    // 4. 销毁视图控制器
    func dismantleUIViewController(_ uiViewController: UIViewController, coordinator: Coordinator) {
        print("4. dismantleUIViewController")
    }
}
```





## Coordinator

![img](https://www.sunyazhou.com/assets/images/20240726UIViewrepresentable/UIViewRepresentable.webp)

协调器（Coordinator）在 SwiftUI 中主要用于桥接 UIKit 和 SwiftUI，处理委托模式和回调。

1. 基本概念：

```swift
// 基本协调器结构
struct MyView: UIViewRepresentable {
    // 创建协调器
    func makeCoordinator() -> Coordinator {
        Coordinator(self)
    }
    
    // 协调器类定义
    class Coordinator: NSObject {
        var parent: MyView
        
        init(_ parent: MyView) {
            self.parent = parent
        }
    }
}
```

1. 主要用途：

a) 处理委托模式：

```swift
// 处理 UITextField 的委托
struct TextFieldView: UIViewRepresentable {
    @Binding var text: String
    
    func makeCoordinator() -> Coordinator {
        Coordinator(self)
    }
    
    class Coordinator: NSObject, UITextFieldDelegate {
        var parent: TextFieldView
        
        init(_ parent: TextFieldView) {
            self.parent = parent
        }
        
        func textFieldDidChangeSelection(_ textField: UITextField) {
            parent.text = textField.text ?? ""
        }
    }
    
    func makeUIView(context: Context) -> UITextField {
        let textField = UITextField()
        textField.delegate = context.coordinator // 设置委托
        return textField
    }
}
```

b) 处理回调：

```swift
// 处理图片选择器回调
struct ImagePicker: UIViewControllerRepresentable {
    @Binding var selectedImage: UIImage?
    @Environment(\.presentationMode) var presentationMode
    
    func makeCoordinator() -> Coordinator {
        Coordinator(self)
    }
    
    class Coordinator: NSObject, UIImagePickerControllerDelegate, UINavigationControllerDelegate {
        let parent: ImagePicker
        
        init(_ parent: ImagePicker) {
            self.parent = parent
        }
        
        // 处理选择图片回调
        func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {
            if let image = info[.originalImage] as? UIImage {
                parent.selectedImage = image
            }
            parent.presentationMode.wrappedValue.dismiss()
        }
        
        // 处理取消回调
        func imagePickerControllerDidCancel(_ picker: UIImagePickerController) {
            parent.presentationMode.wrappedValue.dismiss()
        }
    }
}
```

状态管理：

```swift
struct MapView: UIViewRepresentable {
    @Binding var region: MKCoordinateRegion
    
    class Coordinator: NSObject, MKMapViewDelegate {
        var parent: MapView
        
        init(_ parent: MapView) {
            self.parent = parent
        }
        
        // 更新地图区域
        func mapViewDidChangeVisibleRegion(_ mapView: MKMapView) {
            parent.region = mapView.region
        }
    }
}
```

事件处理：

```swift
struct GestureView: UIViewRepresentable {
    var onTap: () -> Void
    
    class Coordinator: NSObject {
        var parent: GestureView
        
        init(_ parent: GestureView) {
            self.parent = parent
        }
        
        @objc func handleTap() {
            parent.onTap()
        }
    }
    
    func makeUIView(context: Context) -> UIView {
        let view = UIView()
        let tapGesture = UITapGestureRecognizer(
            target: context.coordinator,
            action: #selector(Coordinator.handleTap)
        )
        view.addGestureRecognizer(tapGesture)
        return view
    }
}
```

数据源实现：

```swift
struct TableView: UIViewRepresentable {
    var items: [String]
    
    class Coordinator: NSObject, UITableViewDataSource {
        var parent: TableView
        
        init(_ parent: TableView) {
            self.parent = parent
        }
        
        func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
            return parent.items.count
        }
        
        func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
            let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath)
            cell.textLabel?.text = parent.items[indexPath.row]
            return cell
        }
    }
}
```

生命周期管理：

```swift
struct VideoPlayer: UIViewControllerRepresentable {
    class Coordinator: NSObject {
        var parent: VideoPlayer
        var player: AVPlayer?
        
        init(_ parent: VideoPlayer) {
            self.parent = parent
        }
        
        deinit {
            // 清理资源
            player?.pause()
            NotificationCenter.default.removeObserver(self)
        }
    }
}
```





# Binding

@Binding 是 SwiftUI 中用于在视图之间共享和同步数据的属性包装器。它允许一个视图修改另一个视图的状态。

**作用**

数据共享：在父视图和子视图之间共享数据。

双向绑定：子视图可以修改父视图的状态。

**最小示例**

以下是一个简单的示例，展示如何使用 @Binding 在父视图和子视图之间共享数据。

```swift
// 父视图
struct ContentView: View {
  @State private var isOn: Bool = false // 父视图的状态

  var body: some View {
    VStack {
      Toggle("父视图开关", isOn: $isOn) // 直接使用状态
      ChildView(isOn: $isOn) // 传递绑定到子视图
    }
  }
}
```



```swift
// 子视图

struct ChildView: View {
  @Binding var isOn: Bool // 接收父视图的绑定

  var body: some View {
    Toggle("子视图开关", isOn: $isOn) // 使用绑定
  }
}
```



```swift
import SwiftUI

struct ContentView_Previews: PreviewProvider {
  static var previews: some View {
    ContentView()
  }
}
```



**解释**

@State：用于在父视图中声明一个状态变量。

$ 符号：用于将 @State 变量转换为 @Binding，以便传递给子视图。

@Binding：在子视图中声明一个绑定变量，以便与父视图的状态同步。

在这个示例中，ContentView 中的 Toggle 和 ChildView 中的 Toggle 都会同步显示和修改 isOn 的值。无论在哪个视图中切换开关，另一个视图都会实时更新。



# 使用沙盒测试 App 内购买项目

https://developer.apple.com/cn/documentation/storekit/in-app_purchase/testing_in-app_purchases_with_sandbox/

https://developer.apple.com/cn/help/app-store-connect/test-in-app-purchases/create-a-sandbox-apple-account/



# 字体大小

```
// System Fonts
.font(.largeTitle)  // 34pt
.font(.title)       // 28pt
.font(.title2)      // 22pt
.font(.title3)      // 20pt
.font(.headline)    // 17pt (semi-bold)
.font(.body)        // 17pt
.font(.callout)     // 16pt
.font(.subheadline) // 15pt
.font(.footnote)    // 13pt
.font(.caption)     // 12pt
.font(.caption2)    // 11pt
```

# 设置不同圆角

## SwiftUI

```swift
import SwiftUI

extension View {
    func roundedCorner(_ radius: CGFloat, corners: UIRectCorner) -> some View {
        clipShape(RoundedCorner(radius: radius, corners: corners) )
    }
}


struct RoundedCorner: Shape {
    var radius: CGFloat = .infinity
    var corners: UIRectCorner = .allCorners

    func path(in rect: CGRect) -> Path {
        let path = UIBezierPath(roundedRect: rect, byRoundingCorners: corners, cornerRadii: CGSize(width: radius, height: radius))
        return Path(path.cgPath)
    }
}

// usage
struct ChatBubbleView: View {
    var body: some View {
        Color(red: 249/255, green: 244/255, blue: 241/255, opacity: 0.8)
            .roundedCorner(16, corners: [.topLeft, .topRight, .bottomRight])
            .roundedCorner(4, corners: .bottomLeft)
            .shadow(color: .black.opacity(0.05), radius: 10, x: 0, y: 0)
    }
}
```

## UIKit

```swift
import UIKit

class ChatFreeLimitCell: UITableViewCell {

	private let bubbleView: UIView = {
        let view = UIView()
 		view.backgroundColor = UIColor(red: 249/255, green: 244/255, blue: 241/255, alpha: 0.8)
        view.layer.shadowColor = UIColor.black.cgColor
        view.layer.shadowOpacity = 0.05
        view.layer.shadowOffset = .zero
        view.layer.shadowRadius = 10
    
        return view
  }()

	 override init(style: UITableViewCell.CellStyle, reuseIdentifier: String?) {
        super.init(style: style, reuseIdentifier: reuseIdentifier)
        setupViews()
    }

	required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
  
  private func setupViews() {
    contentView.backgroundColor = .clear
    backgroundColor = .clear
    selectionStyle = .none

    contentView.addSubview(bubbleView)

    bubbleView.snp.makeConstraints { make in
        make.width.equalTo(330)
        make.height.equalTo(100)
        make.left.equalToSuperview()
        make.top.equalToSuperview().offset(8)
        make.bottom.equalToSuperview().offset(-8)
    }
  }

	 override func layoutSubviews() {
        super.layoutSubviews()
        bubbleView.addRoundedCorners(topLeft: 16, topRight: 16, bottomLeft: 4, bottomRight: 16)
    }
}


extension UIView {
    func addRoundedCorners(topLeft: CGFloat = 0, topRight: CGFloat = 0, bottomLeft: CGFloat = 0, bottomRight: CGFloat = 0) {
        let path = UIBezierPath()
        let rect = bounds
        
        // Starting from top-left, going clockwise
        path.move(to: CGPoint(x: rect.minX + topLeft, y: rect.minY))
        
        // Top edge
        path.addLine(to: CGPoint(x: rect.maxX - topRight, y: rect.minY))
        
        // Top-right corner
        path.addArc(withCenter: CGPoint(x: rect.maxX - topRight, y: rect.minY + topRight),
                    radius: topRight,
                    startAngle: -CGFloat.pi/2,
                    endAngle: 0,
                    clockwise: true)
        
        // Right edge
        path.addLine(to: CGPoint(x: rect.maxX, y: rect.maxY - bottomRight))
        
        // Bottom-right corner
        path.addArc(withCenter: CGPoint(x: rect.maxX - bottomRight, y: rect.maxY - bottomRight),
                    radius: bottomRight,
                    startAngle: 0,
                    endAngle: CGFloat.pi/2,
                    clockwise: true)
        
        // Bottom edge
        path.addLine(to: CGPoint(x: rect.minX + bottomLeft, y: rect.maxY))
        
        // Bottom-left corner
        path.addArc(withCenter: CGPoint(x: rect.minX + bottomLeft, y: rect.maxY - bottomLeft),
                    radius: bottomLeft,
                    startAngle: CGFloat.pi/2,
                    endAngle: CGFloat.pi,
                    clockwise: true)
        
        // Left edge
        path.addLine(to: CGPoint(x: rect.minX, y: rect.minY + topLeft))
        
        // Top-left corner
        path.addArc(withCenter: CGPoint(x: rect.minX + topLeft, y: rect.minY + topLeft),
                    radius: topLeft,
                    startAngle: CGFloat.pi,
                    endAngle: -CGFloat.pi/2,
                    clockwise: true)
        
        path.close()
        
        let maskLayer = CAShapeLayer()
        maskLayer.path = path.cgPath
        layer.mask = maskLayer
    }
}

```

