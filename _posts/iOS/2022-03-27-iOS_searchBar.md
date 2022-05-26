---
layout: post
title: iOS SearchBar 사용해보기
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## UISearchBar 사용해보기

서치 컨트롤러를 사용하다가 이래저래.. 서치바로 넘어왔습니다.

서치바는 아래와 같이 사용했습니다. 

```swift 
class SearchViewController: UIViewController {
    @IBOutlet weak var searchBar: UISearchBar!
    var isFiltering: Bool = false

    var arr = ["zehye", "hi", "hello", "nice", "to", "meet", "you"]
    var filterredArr: [String] = []

    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.initUI()
        self.setSearchControllerUI()
    }

    func initUI() {
        self.tableView.delegate = self
        self.tableView.dataSource = self
    }

    func setSearchControllerUI() {
        self.searchBar.delegate = self
        self.searchBar.showsCancelButton = false
    }
}

extension SearchViewController: UITableViewDelegate, UITableViewDataSource {
    public func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return self.isFilterting ? self.filterredArr.count : self.arr.count
    }

    public func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "SeachListCell") as! SearchListTableViewCell
        if self.isFiltering {
            cell.textLabel?.text = self.filterredArr[indexPath.row]
        } else {
            cell.textLabel?.text = self.arr[indexPath.row]
        }

        return cell
    }
}

extension SearchViewController: UISearchBarDelegate {
    // 서치바에서 검색을 시작할 때 호출 
    func searchBarTextDidBeginEditing(_ searchBar: UISearchBar) {
        self.isFiltering = true
        self.searchBar.showsCancelButton = true
        self.tableView.reloadData()
    }
    
    func searchBar(_ searchBar: UISearchBar, textDidChange searchText: String) {
        guard let text = searchController.searchBar.text?.lowercased() else { return }
        self.filterredArr = self.arr.filter { $0.localizedCaseInsensitiveContains(text) }
       
        self.tableView.reloadData()
    }
    
    // 서치바에서 검색버튼을 눌렀을 때 호출 
    func searchBarSearchButtonClicked(_ searchBar: UISearchBar) {
        dismissKeyboard()

        guard let text = searchController.searchBar.text?.lowercased() else { return }
        self.filterredArr = self.arr.filter { $0.localizedCaseInsensitiveContains(text) }
       
        self.tableView.reloadData()
    }
    
    // 서치바에서 취소 버튼을 눌렀을 때 호출 
    func searchBarCancelButtonClicked(_ searchBar: UISearchBar) {
        self.searchBar.text = ""
        self.searchBar.resignFirstResponder()
        self.isFiltering = false
        self.tableView.reloadData()
    }
    
    // 서치바 검색이 끝났을 때 호출
    func searchBarTextDidEndEditing(_ searchBar: UISearchBar) {
        self.tableView.reloadData()
    }
    
    // 서치바 키보드 내리기 
    func dismissKeyboard() {
        searchBar.resignFirstResponder()
    }
}
```