# React 中 key 的作用為何 ?

key 就是作為 DOM 的識別，當資料發生變化時 react 會依據新的資料生成新的 virtual DOM，
然後 react 會將新舊的 virtual DOM 做 diff 比對

1. 如果新舊 VDOM 中找到相同的 key 且內容也相同的話就會直接使用舊 VDOM
2. 如果新舊 VDOM 中找到相同的 key 但內容不同則會產生新的 RDOM 然後更新畫面
3. 如果新舊 VDOM 中沒有相同的 key 則生成新的 RDOM


## 問題來了 如果用 index 做為 key 的話可能會有什麼問題出現 ?

簡單的結論，如果當資料的順序性遭到破壞時，所有的新舊 key 跟內容會比對出差異，進而全部的內容都重新 render，
例如 : 
我有 2000 筆資料已經 render 出來並使用 index 作為 key，他們的 key 則為 0 1 2 3 開始，
今天我在最前面新增第 2001 筆的資料，這時候 react 將產生出新的 VDOM 然後去跟舊的 VDOM 做比對，
發現到原先 key 0 1 2 3 都變為 1 2 3 4，全部往後推移順序遭到改變，對於 react 來說就是雖有相同的 key，
但是內容卻不同了，所以全部 2001 筆資料全部重新 render，
如果我是用獨一無二的值作為 key，那麼就算我的順序遭到破壞，但是我的 key 仍然可以作為 render 的判斷基準，
所以我可以繼續使用原先已經 render 的內容，只需要重新 render 那一筆新的內容就好

條列一下
1. 當資料出現逆序或是破壞順序的操作實，可能引發不必要的重新渲染
2. 結構中有輸入欄位時，可能造成畫面錯誤顯示

所以，盡可能避免使用 index 作為 key 值，除非已保證不會出現破壞順序的簡單資料顯示