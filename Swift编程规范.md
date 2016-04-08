# Swift编程规范

## 一. 规范的目的
1. 增进精确，减少程序员犯错的可能
2. 明确意图  
3. 减少冗余  
4. 使代码尽量优美，方便自己或其他人查看

## 二. 规范整理
### 1. 命名
#### 类／结构体／变量／常量
类／结构体／方法／变量／常量等定义时，均采用驼峰式命名规则，类／结构体首字母大写，其余首字母小写。

	private let maximumWidgetCount = 100	// 不要用 MAX_WIDGET_COUNT
	
	class WidgetContainer {
	
		var widgetButton: UIButton
		let widgetHeightPercentage = 0.85
	}

#### 方法
对于方法和init方法，为所有参数合理命名，必要的时候使用`外部参数名`增强可读性。

	func dateFromString(dateString: String) -> NSDate
	func convertPointAt(column column: Int, row: Int) -> CGPoint
	func timedAction(afterDelay delay: NSTimeInterval, perform action: SKAction) -> SKAction!
	
	// would be called like this:
	dateFromString("2014-03-14")
	convertPointAt(column: 42, row: 13)
	timedAction(afterDelay: 1.0, perform: someOtherAction)

#### 枚举
枚举类型定义时，采用`大驼峰`方式命名，包括其属性。

	enum Shape {
		case Rectangle
		case Square
		case Triangle
		case Circle
	}

#### 类前缀
Swift 支持命名空间，所以不需要再加类前缀，如果来自不同模块的两个同名类发生冲突，你可以通过模块名调用的方式消除歧义。

	import SomeModule
	
	let myClass = MyModule.UsefulClass()

### 2. 留白
* 用 `Tabs`，而非`" "`(空格)
* 文件结束时留一空行
* 用足够的空行把代码分割成合理的块
* 不要在一行结尾留下空白
* 千万别在空行留下缩进

#### 缩进格式
所有成对出现的`{}`，起始`{`在同一行，结尾`}`另起一行

例如:

	class Person {
		...
	}
		
	if user.isHappy {
		// Do something
	} else {
		// Do something else
	}

### 3. 注释
注释采用中文书写。

* 关键代码一定要有注释说明
* 废弃的代码要及时清理，不要注释后不管
* 需要保留的停用代码，注释后需要加以说明

### 4. 协议
为一个类添加遵循的协议时，可以用扩展的方式写，同时不要忘记添加`// MARK: -`，使代码组织的更合理。

	class MyViewcontroller: UIViewController {
		// class stuff here
	}
	
	// MARK: - UITableViewDataSource
	extension MyViewcontroller: UITableViewDataSource {
		// table view data source methods
	}
	
	// MARK: - UIScrollViewDelegate
	extension MyViewcontroller: UIScrollViewDelegate {
		// scroll view delegate methods
	}

### 5.函数声明
较短的函数声明，控制在一行搞定。

	func reticulateSplines(spline: [Double]) -> Bool {
		// reticulate code goes here
	}

较长的函数声明（参数较多时），合理的增加换行，并在换行后增加额外的缩进。

	func reticulateSplines(spline: [Double],
			adjustmentFactor: Double,
    		translateConstant: Int,
    		comment: String) -> Bool {
    	// reticulate code goes here
    }

### 6. 类型
使用 Swift 的数据类型，除非非用不可，才去用 OC 的类型，因为 Swift 提供了对 OC 数据类型的桥接及自动转换。

要用：

	let width = 120.0                                    // Double
	let widthString = (width as NSNumber).stringValue    // String

不要用：

	let width: NSNumber = 120.0                          // NSNumber
	let widthString: NSString = width.stringValue        // NSString

### 7. Struct 初始化
用 Swift 的 struct 初始化方式，不要用 CGGeometry 的构造方式。

要用：

	let bounds = CGRect(x: 40, y: 20, width: 120, height: 80)
	let centerPoint = CGPoint(x: 96, y: 42)

不要用：

	let bounds = CGRectMake(40, 20, 120, 80)
	let centerPoint = CGPointMake(96, 42)
	
### 8. 类型推断
为了代码简洁，不需要显示声明变量／常量类型，让编译器推断，`CGFloat/Int16`这种特殊类型，编译器根据默认值无法推断时除外。

要用：

	let message = "Click the button"
	let currentBounds = computeViewBounds()
	var names = [String]()
	let maximumWidth: CGFloat = 106.5

不要用：

	let message: String = "Click the button"
	let currentBounds: CGRect = computeViewBounds()
	var names: [String] = []

### 9. 语法糖
对于数组和字典，采用简洁版本的声明方式。

要用：

	var deviceModels: [String]
	var employees: [Int: String]
	var faxNumber: Int?

不要用：

	var deviceModels: Array<String>
	var employees: Dictionary<Int, String>
	var faxNumber: Optional<Int>

### 10. For 循环
采用 `for-in` 的方式，而不是传统的 `for-condition-increment` 的方式。

要用：

	for _ in 0..<3 {
		println("Hello three times")
	}
	
	for (index, person) in attendeeList.enumerate() {
		println("\(person) is at position #\(index)")
	}
	
不要用：

	for var i = 0; i < 3; i++ {
		println("Hello three times")
	}
	
	for var i = 0; i < attendeeList.count; i++ {
		let person = attendeeList[i]
		println("\(person) is at position #\(i)")
	}

### 11. 分号
* 不要在每行的结束加 `;`
* 只在合并变量声明的时候用 `;`

### 12. 控制流中的括号
`if-else/for/while/switch-case` 等控制流语句中，不需要在 condition 部分添加 `()` 。


	for _ in 0..<3 {
		...
	}
	
	if value > 3 {
		...
	} else {
		...
	}

### 13. 常量和变量
能用 `let` 尽量不用 `var` 。

尽可能的用 `let foo = ...` 而不是 `var foo = ...` （并且包括你疑惑的时候）。万不得已的时候，再用 `var` （就是说：你知道这个值会改变，比如：有 `weak` 修饰的存储变量）。

理由：这俩关键字无论意图还是意义都很清楚了，但是 `let` 可以产生安全清晰的代码。

`let`-有保障，并且它的值的永远不会变对程序猿也是个清晰的标记，对于它的用法，之后的代码可以做个强而有力的推断。

猜测代码更容易了。不然一旦你用了 `var`，还要去推测值会不会变，这时候你就不得不人肉去检查。

这样，无论何时你看到 `var`，就假设它会变，并问自己为啥。

### 14. 尽早地 Return 或者 break
当你遇到某些操作需要通过条件判断去执行，应当尽早地退出判断条件。
要用：

	guard n.isNumber else {
		return
	}

不要用：

	if n.isNumber {
		// Use n here
    } else {
    	return
    }

理由： 你一但声明 `guard` 编译器会强制要求你和 `return`, `break` 或者 `continue` 一起搭配使用，否则会产生一个编译时的错误。

### 15. 避免对可选类型强解包
如果你有个 `FooType?` 或 `FooType!` 的 `foo`，尽量不要强行展开它以得到基本类型（`foo!`）。

	if let foo = foo {
		// Use unwrapped `foo` value in here
    } else {
    	// If appropriate, handle the case where the optional is nil
    }

理由： `if let` 绑定可选类型产生了更安全的代码，强行展开很可能导致运行时崩溃。

### 16. 避免毫无保留地展开可选类型
如果 `foo` 可能为 `nil` ，尽可能的用 `let foo: FooType?` 代替 `let foo: FooType!`（注意：一般情况下，`?`可以代替`!`）

理由: 明确的可选类型产生了更安全的代码。无保留地展开可选类型也会挂。

### 17. 只读属性的 properties 和 subscripts
对于只读属性的 `properties` 和 `subscripts`，选用隐式的 `getters` 方法，省略只读属性的 `properties` 和 `subscripts` 的 `get` 关键字。

要用：

	var myGreatProperty: Int {
		return 4
	}
	
	subscript(index: Int) -> T {
		return objects[index]
	}

不要用：

	var myGreatProperty: Int {
    	get {
        	return 4
    	}
	}
	
	subscript(index: Int) -> T {
    	get {
        	return objects[index]
    	}
	}

理由: 第一个版本的代码意图已经很清楚了，并且用了更少的代码。

### 18. 对于顶级定义，永远明确的列出权限控制
顶级函数，类型和变量，永远应该有着详尽的权限控制说明符。

	public var whoopsGlobalState: Int
	internal struct TheFez {}
	private func doTheThings(things: [Thing]) {}

### 19. 当指定一个类型时，把冒号和标识符连在一起
当指定标示符的类型时，冒号要紧跟着标示符，然后空一格再写类型。

	class SmallBatchSustainableFairtrade: Coffee { ... }

	let timeToCoffee: NSTimeInterval = 2

	func makeCoffee(type: CoffeeType) -> Coffee { ... }

理由: 类型区分号是对于 identifier 来说的，所以要跟它连在一起。

### 20. 需要时才写上 self
当调用 `self` 的 `properties` 或 `methods` 时，`self` 用默认的隐式引用：

	private class History {
	
	    var events: [Event]

    	func rewrite() {
        	events = []
    	}
	}

必要的时候再加上self, 比如在闭包里，或者 参数名冲突了：

	extension History {
	
	    init(events: [Event]) {
    	    self.events = events
    	}

	    var whenVictorious: () -> () {
    	    return {
        	    self.rewrite()
        	}
    	}
	}

原因: 在闭包里用self更加凸显它的语义，并且避免了别处的冗长。

### 21. 首选 structs 而非 classes
除非你需要 `class` 才能提供的功能（比如 `identity` 或 `deinitializers`），不然就用 `struct`

要注意到继承通常不是用类的好理由，因为多态可以通过协议实现，重用可以通过组合实现。

比如，这个类的分级:

	class Vehicle {
	
    	let numberOfWheels: Int

	    init(numberOfWheels: Int) {
    	    self.numberOfWheels = numberOfWheels
    	}

	    func maximumTotalTirePressure(pressurePerWheel: Float) -> Float {
    	    return pressurePerWheel * Float(numberOfWheels)
    	}
	}

	class Bicycle: Vehicle {
	
    	init() {
        	super.init(numberOfWheels: 2)
    	}
	}

	class Car: Vehicle 
	{
    	init() {
        	super.init(numberOfWheels: 4)
    	}
	}

可以重构成：

	protocol Vehicle {
	
		var numberOfWheels: Int { get }
	}

	func maximumTotalTirePressure(vehicle: Vehicle, pressurePerWheel: Float) -> Float {
    	return pressurePerWheel * Float(vehicle.numberOfWheels)
	}

	struct Bicycle: Vehicle {
	
    	let numberOfWheels = 2
	}

	struct Car: Vehicle {
	
    	let numberOfWheels = 4
	}

理由: 值的类型更简单，容易辨别，并且通过`let`关键字可猜测行为，而`class`为引用类型。

> **另外，尽量不要用 `继承`，用 `协议` 和 `组合` 来实现需要的功能。**

### 22. 操作符 两边留空格
操作符无论是实用时，还是在重载定义时，均需要在两边留空格，增强代码清晰度。

	func <| (lhs: Int, rhs: Int) -> Int
	
	let aString = "hello " + "world"

## 三. 参考
本规范参考了：

* [github/swift-style-guide](https://github.com/github/swift-style-guide)
* [raywenderlich/swift-style-guide](https://github.com/raywenderlich/swift-style-guide)

> **规范需要不断的优化、完善，在 coding 过程中有了新的总结要及时补充进来。**