# Swift

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
