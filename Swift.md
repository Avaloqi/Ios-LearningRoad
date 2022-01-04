# Swift

##Learn

<details>
  <summary>00</summary>
</details>

### 0x003

swift

### 0x007
swift常用的集合类型Array，Dictionary，Set都遵循协议 Collection，译为集合，是一个元素可以反复遍历并且可以通过索引的下标访问的有限集合
协议Collection继承于Sequence，在Sequence的基础上扩展了下标访问、元素个数能特性。

<details>
  <summary>protocol Collection</summary>
  
    public protocol Collection : Sequence {
        associatedtype Index : Comparable
        var startIndex: Index { get }
        var endIndex: Index { get }
        var isEmpty: Bool { get }
        var count: Int { get }
  
        subscript(position: Index) -> Element { get }
        subscript(bounds: Range<Index>) -> SubSequence { get }
    }
</details>
Sequence 即序列，该协议目的是一系列相同类型值的集合，并且可以进行迭代（可以看做遍历，使用for-in，序列可以是无限的
  
<details>
  <summary>protocol Sequence</summary>
  
    protocol Sequence {
        associatedtype Iterator: IteratorProtocol
        func makeIterator() -> Iterator
    }
  
    //其引入的协议IteratorProtocol是为序列提供迭代能力
    public protocol IteratorProtocol {
        associatedtype Element
        public mutating func next() -> Self.Element?
    }
</details>

  
  
### async/await
  
    func taskkkk() async -> [string] {
    }

使用这种写法可将函数（方法）定义为异步函数（方法），调用异步函数时，要在调用位置使用await标识此处为可能的悬点，
与此相应地，执行会被挂起直到异步方法返回， 所有的悬点都需要被标记为await
以文档代码为例
    
    let photoNames = await listPhotos(inGallery: "Summer Vacation")
    let sortedNames = photoNames.sorted()
    let name = sortedNames[0]
    let photo = await downloadPhoto(named: name)
    show(photo)
使用await标识可以看作当下面的程序依赖当前程序的返回时，程序可以在此处阻塞式地运行等到方法返回结果，与此同时其他并行代码继续在执行
比如listPhotos函数必须返回执行才能继续往下走

相比这种实现，async/await 的使用还在于异步序列方式以及并行的调用异步方法

    for try await line in handle.bytes.lines {
        print(line)
        //do staff
    }
    
与普通的 for-in 循环相比，上面的列子在 for 之后添加了 await 关键字。就像在调用异步函数或方法时一样，await 表明代码中有一个可能的悬点。for-await-in 循环会在每次循环开始的时候因为有可能需要等待下一个元素而挂起当前代码的执行。
想让自己创建的类型使用 for-in 循环需要遵循 Sequence 协议，这里也同理，如果想让自己创建的类型使用 for-await-in 循环，就需要遵循 AsyncSequence 协议。
如果do staff部分依赖于上一次的结果，则可以使用这种方式等待每一次的调用完成

并行的调用异步方法
调用有 await 标记的异步函数在同一时间只能执行一段代码。在异步代码执行的过程中，调用方需要等待异步代码执行完后才能继续执行下一行代码。比如，当你想从图库中拉取前三张图片，可以像下面这样，等三次调用完后再执行 downloadPhoto(named:) 函数：
    
    let firstPhoto = await downloadPhoto(named: photoNames[0])
    let secondPhoto = await downloadPhoto(named: photoNames[1])
    let thirdPhoto = await downloadPhoto(named: photoNames[2])

    let photos = [firstPhoto, secondPhoto, thirdPhoto]
    show(photos)
这种方式有一个非常严重的缺陷：虽然下载过程是异步的，并且在等待过程中可以执行其他任务，但每次只能执行一个 downloadPhoto(named:)。每一张图片只能在上一张图片下载结束了才开始下载。然而，并没有必要让这些操作等待，每张图片可以独立甚至同时下载。
为了在调用异步函数的时候让它附近的代码并发执行，定义一个常量时，在 let 前添加 async 关键字，然后在每次使用这个常量时添加 await 标记。

    async let firstPhoto = downloadPhoto(named: photoNames[0])
    async let secondPhoto = downloadPhoto(named: photoNames[1])
    async let thirdPhoto = downloadPhoto(named: photoNames[2])

    let photos = await [firstPhoto, secondPhoto, thirdPhoto]
    show(photos)
在上面的例子中，三次调用 downloadPhoto(named:) 都不需要等待前一次调用结束。如果系统有足够的资源，这三次调用甚至都可以同时执行。这三次调用都没有没标记为 await，因为代码不需要被挂起等待函数的结果。程序会继续执行直到 photos 被定义，与上面不同的是，在这个时间点由于程序需要上面几次异步调用的结果，所以你需要添加 await 关键字来挂起当前代码的执行直到所有图片下载完成。
下面是关于两种不同方法的一些说法：

  代码中接下来的几行需要依赖异步函数的结果时，需要使用 await 来调用异步函数。这样产生的结果是有序的。
  短时间内并不需要异步函数的结果时，需要使用 async-let 来调用异步函数。这样产生的任务是并发的。
  await 和 async-let 都允许其他任务在他们被挂起的时候执行。
  
在两种情况下，都需要用 await 标记可能的悬点，以表明代码在这些点在需要的情况下会被挂起，直到异步函数执行结束。
也可以在同一段代码中混用两种方式。


此外，使用await标记悬点也使用于外部访问actor的可变属性情况时，actor在同一时间只允许一个任务访问它的可变状态，这使得多个任务中的代码与一个 actor 交互时更加安全
与此相应的是，当某任务要修改actor的属性时，该属性可能正在被访问，于是需要添加await等到属性的访问结束，以此来保证安全性



。。。
##Tips

###关于collectionview

![image](https://user-images.githubusercontent.com/51845254/147029225-faca1ded-e05d-4615-a714-82b3653acffd.png)

collectionview默认为Flow的Layout和Vertical的scollDirection格式，即流布局方式横向流进行布局，可以切换为流满行或流满列模式（滚动方向控制）

集合视图的大小检查器有一个名为 Estimate Size 的选项，设置为Automatic的时候，集合视图将计算单元格的实际大小，这取决于图像的大小和布局约束。
同样，这也是为什么某些行包含的项目多于其他行的原因(即每个单元格的大小可以不一样，对于cell的大小指定会被忽略)。
要固定单元格的大小，一个简单的技巧是将估计大小选项从自动更改为None。集合视图将不再计算单元格的实际大小，而是使用您在故事板中指定的大小。
也可以在代码中进行设置
<details>
  <summary>code</summary>
  
    if let layout = collectionViewLayout as? UICollectionViewFlowLayout {
      layout.itemSize = CGSize(width: 100, height: 150)
      layout.estimatedItemSize = .zero
    }
  
  </details>
