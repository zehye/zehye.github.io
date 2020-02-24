---
layout: post
title: 뷰의 재사용(연습)
category: iOS
tags: [iOS]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    

<hr>

## 뷰의 재사용 실습

이전까지의 코드를 살펴보면 `dequeueReusableCell`라는 걸 사용하는 것을 볼 수 있다.

```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {

    if indexPath.section < 2 {
        let cell: UITableViewCell = tableView.dequeueReusableCell(withIdentifier: self.cellIdentifier, for: indexPath)
        let text: String = indexPath.section == 0 ? korean[indexPath.row] : english[indexPath.row]
        cell.textLabel?.text = text
        return cell

    } else {
        let cell: CustomTableViewCell = tableView.dequeueReusableCell(withIdentifier: self.customCellIdentifier, for: indexPath) as! CustomTableViewCell
        cell.leftLabel.text = self.dateFormatter.string(from: self.dates[indexPath.row])
        cell.righLable.text = self.timeFormatter.string(from: self.dates[indexPath.row])
        return cell
    }
}
```

이는 큐에 쌓여있던 재사용가능한 셀을 꺼내와 사용한다는 것을 의미한다.

<center>
<figure>
<img src="/assets/post-img/iOS/50.png" alt="" width="80%">
</figure>
</center>

위 그림을 보면 테이블뷰에는 여러 셀이 존재하고 있음을 볼 수 있다. 처음 왼쪽 그림을 보면 첫번째 셀이 맨 위에 있을 것이고 사용자가 더 아래의 셀을 보고싶다면 스크롤을 해서 화면을 올릴 것이고 그렇게 된다면 첫번째 셀은 화면에서 사라지고 왼쪽의 화면이 보이게 될 것이다. 이떄 화면에서 사라지게 되면 해당 셀은 **Queue** 에 가게 된다. (재사용 큐-재사용을 대기) 그러다가 밑에서 셀이 더 필요하게 되면 재사용 대기중인 셀이 그 뒤에 붙게된다. 즉, 화면 바깥으로 나간 셀은 다시 재사용이 필요해서 아래로 붙게 되기에 우리가 위에서 사용했던 **dequeueReusableCell** 는 이를 의미한다. (큐에 들어갔다가 다시 큐에서 나오는 것)

이를 확인해보기 위해 간단하게 코드를 작성해봅시다.

```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {

    if indexPath.section < 2 {
        let cell: UITableViewCell = tableView.dequeueReusableCell(withIdentifier: self.cellIdentifier, for: indexPath)
        let text: String = indexPath.section == 0 ? korean[indexPath.row] : english[indexPath.row]
        cell.textLabel?.text = text

        if indexPath.row == 1 {
            cell.backgroundColor = UIColor.red
        } else {  // 해당 코드가 없으면 재사용되는 코드 모두가 빨간색이 되어버린다
            cell.backgroundColor = UIColor.white  
        }
        return cell

    } else {
        let cell: CustomTableViewCell = tableView.dequeueReusableCell(withIdentifier: self.customCellIdentifier, for: indexPath) as! CustomTableViewCell
        cell.leftLabel.text = self.dateFormatter.string(from: self.dates[indexPath.row])
        cell.righLable.text = self.timeFormatter.string(from: self.dates[indexPath.row])
        return cell
    }
}
```

그런데 우리가 셀을 재사용하지 않는다면 어떻게 될까? > 코드를 작성해보자.

```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {

    if indexPath.section < 2 {
        let cell: UITableViewCell = UITableViewCell()  // 셀을 하나 생성
        let text: String = indexPath.section == 0 ? korean[indexPath.row] : english[indexPath.row]
        cell.textLabel?.text = text

        if indexPath.row == 1 {
            cell.backgroundColor = UIColor.red
        } else {
            cell.backgroundColor = UIColor.white
        }
        return cell

    } else {
        let cell: CustomTableViewCell = tableView.dequeueReusableCell(withIdentifier: self.customCellIdentifier, for: indexPath) as! CustomTableViewCell
        cell.leftLabel.text = self.dateFormatter.string(from: self.dates[indexPath.row])
        cell.righLable.text = self.timeFormatter.string(from: self.dates[indexPath.row])
        return cell
    }
}
```

이렇게 되면 테이블 뷰의 로우가 정말 많은 숫자가 된다면 셀을 그에 맞게 만들어줘야 하기에 메모리의 낭비가 굉장히 심해질 것이다. 즉 화면에서 벗어난 셀은 큐에 담아두고 쓰고 하는 방식을 채택하는 것이 옳다. 
