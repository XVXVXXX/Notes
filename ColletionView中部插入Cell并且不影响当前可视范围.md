### ColletionView中部插入Cell并且不影响当前可视范围.md  
以`IGListKit`为例，在`willDisplay`中请求异步数据并`reload`  
```swift
  func listAdapter(_ listAdapter: ListAdapter, willDisplay sectionController: ListSectionController, cell: UICollectionViewCell, at index: Int) {
    if !self.dataLoaded {
      // req data
      DispatchQueue.main.asyncAfter(deadline: DispatchTime.now() + 2) {
        // load data
        self.dataLoaded.toggle()
        // record current position
        let oldH : CGFloat = (listAdapter.collectionView?.contentSize.height)!
        let oldY : CGFloat = (listAdapter.collectionView?.contentOffset.y)!
        
        self.collectionContext?.performBatch(animated: false, updates: { batchContext in
          batchContext.reload(self)
          print("updating")
        }, completion: { finished in
          print("updated")
          let newH = (listAdapter.collectionView?.contentSize.height)!
          let diff = newH - oldH
          if diff > 0 {
            let view = listAdapter.collectionView
            view?.setContentOffset(CGPoint.init(x: 0, y: oldY+diff), animated: false)
          }
        })
      }
    }
  }
```
