---
layout: post
title: Data Structure, Assembly Line Scheduling
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## Assembly Line Scheduling

실생활에서 Dynamic Programming이 쓰일때는 언제일까? >> 생산공정을 생각해보자

<center>
<figure>
<img src="/assets/post-img/DataStructure/16.jpeg" alt="" width="80%">
</figure>
</center>


<center>
<figure>
<img src="/assets/post-img/DataStructure/17.jpeg" alt="" width="80%">
</figure>
</center>

- Memoization을 통해 minimum tracel time을 확인해 볼 수 있다.
- retrace table을 통해 어떤 선택지를 하였는지 저장을 하여 공정 루트를 만들어낼 수 있었다.



### Assembly Line Scheduling in Recursion

```python
class AssemblyLines:
  timeStation = [[7,9,3,4,8,4],[8,5,6,4,5,7]]
  timeBelt = [[2,2,3,1,3,4,3],[4,2,1,2,2,1,2]]
  intCount = 0

  def scheduling(self, idxLine, idxStation):
    print("Calculate scheduling: Line, station:", idxLine, idxStation, "()", self.intCount, "recursion calls")
    self.intCount = self.intCount + 1
    if idxStation == 0:  # excape
      if idxLine == 1:
        return self.timeBelt[0][0] + self.timeStation[0][0]
      elif idxLine == 2:
        return self.timeBelt[1][0] + self.timeStation[1][0]

    if idxLine == 1:  # Recursive
      costLine1 = self.Scheduling(1, idxStation-1) + self.timeStation[0][idxStation]
      costLine2 = self.Scheduling(2, idxStation-1) + self.timeStation[0][idxStation] + self.timeBelt[1][idxStation]
    elif idxLine == 2:
      costLine1 = self.Scheduling(1, idxStation-1) + self.timeStation[1][idxStation] + self.timeBelt[0][idxStation]
      costLine2 = self.Scheduling(2, idxStation-1) + self.timeStation[1][idxStation]

    if costLine1 > costLine2:
      return costLine2
    else:
      return costLine1


  def startScheduling(self):
    numStation = len(self.timeStation[0])
    costLine1 = self.Scheduling(1, numStation -1) + self.timeBelt[0][numStation]
    costLine2 = self.Scheduling(2, numStation -1) + self.timeBelt[1][numStation]
    if costLine1 > costLine2:
      return costLine2
    else:
      return costLine1

lines = AssemblyLines()
time = lines.startScheduling()
print("Fastest production time: ", time)
```



### Assembly Line Scheduling in DP

```python
class AssemblyLines:
  timeStation = [[7,9,3,4,8,4],[8,5,6,4,5,7]]
  timeBelt = [[2,2,3,1,3,4,3],[4,2,1,2,2,1,2]]

  timeScheduling = [range(6), range(6)]
  stationTracing = [range(6), range(6)]

  def startSchedulingDP(self):
    numStation = len(self.timeStation[6])
    self.timeScheduling[0][0] = self.timeStation[0][0] + self.timeBelt[0][0]
    self.timeScheduling[1][0] = self.timeStation[1][0] + self.timeBelt[1][0]

    for itr in range(1, numStation):
      if self.timeScheduling[0][itr-1] > self.timeScheduling[1][itr-1] + self.timeBelt[1][itr]:
         self.timeScheduling[0][itr] = self.timeStation[0][itr] + self.timeScheduling[1][itr-1] + self.timeBelt[1][itr]
         self.stationTracing[0][itr] = 1
      else:
        self.timeScheduling[0][itr] = self.timeStation[0][itr] + self.timeBelt[0][itr]
        self.stationTracing = 0

      if self.timeScheduling[1][itr-1] > self.timeScheduling[0][itr-1] + self.timeBelt[0][itr]:
         self.timeScheduling[1][itr] = self.timeStation[1][itr] + self.timeScheduling[0][itr-1] + self.timeBelt[0][itr]
         self.stationTracing[1][itr] = 0
      else:
        self.timeScheduling[1][itr] = self.timeStation[1][itr] + self.timeBelt[1][itr-1]
        self.stationTracing = 1

    costLine1 = self.timeScheduling[0][numStation-1] + self.timeBelt[0][numStation]
    costLine2 = self.timeScheduling[1][numStation-1] + self.timeBelt[0][numStation]
    if costLine1 > costLine2:
      return costLine2
    else:
      return costLine2

  def printTracing(self, lineTracing):
    numStation = len(self.timeStation[0])
    print("Line : ", lineTracing, ", Station : ", numStation)
    for itr in range(numStation-1, 0, -1):
      lineTracing = self.stationTracing[lineTracing][itr]
      print("line : ", lineTracing, ", Station : ", numStation)


lines = AssemblyLines()
time, lineTracing = lines.startSchedulingDP()
print("Fastest production time: ", time)
lines.printTracing(lineTracing)
```
