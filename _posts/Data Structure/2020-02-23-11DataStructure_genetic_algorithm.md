---
layout: post
title: Data Structure, Genetic Algorithm
category: Data Structure
tags: [Data Structure]
comments: true
---

> 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.    
잘못된 부분이 있다면 댓글로 남겨주세요! :)

<hr>

## Genetic Algorithm

### Difficult Problem

#### 어떤 요소들이 문제를 어렵게 하는가?

1. 알고리즘을 구현하는 것이 어려운것인가?
2. 알고리즘을 이해하는 것이 어려운것인가?
3. 주어진 시간내에 문제를 해결하는 방법을 찾는 것이 어려운것인가?

**O(N!) 어려운 문제라고 판단할 수 있음**


### Search space and global optimum

어떻게 하면 도시를 최적 순회 할 수 있는가? → **n!** 중에 최적화된 루트를 찾아야한다

- BTS는 굉장히 안전하고 쉬운 알고리즘
- 미리 정해진 path가 없는 경우는 실현가능성이 떨어짐 > 이런 경우에는 패턴과 구조가 없다
- 자료구조는 데이터를 잘 정해서 넣기 때문에 쉽게 해결할 수 있음


### Heuristic based Optimization Techniques

- Heuristic
  - 알고리즘과 유사하지만 알고리즘은 정해진 규칙을 가지고 해답을 찾을 수 있음 → 문제푸는 기법에 대한 보장이 있다
  - 그러나 Heuristic은 해답이 있는 것이 아니라, **'이렇게 하면 달성할 수 있다'는 가정** 으로 문제를 품

- Hill climbing heuristic / gradient - ascent heuristics
  - 더이상 올라갈 수 없을때까지 높은 곳으로 올라감
  - 높은 곳으로만 가려고할때 낮은 곳에 둘러싸인 곳은 중간정도 높은 지점을 찾을 수 없음
  - 높은 곳을 계속 찾아서 가면 다행이지만, **local optimum** 이라는 문제가 발생할 수 있음

-> 특정 구역 속에서는 높으나 전체 맵을 생각했을 때 제일 높은 곳이 아닐 수 있음<br>
-> 혹은 높은 곳을 찾을 수 없음

- 그 중 다른 알고리즘 하나가 generic algotrithm이다


## genetic algorithm

- gene : 유전자 한조각
  - 염색체(chromosome)을 구성하는 하나의 유전 정보를 의미. 예를들어 A, B, C를 가지는 염색체가 있을때 A는 유전자(gene)
- genotype: 한 개인은 특정한 genotype 을 가지고 있음
  - 어떤 문제의 해답을 하나로 표현할 수 있거나 여러개의 집합을 하나로 표현 할 수 있는 것을 의미함 > 유전자의 조합
- phenotype:: 얼굴모양과 같이 관찰되는 성질
- Fittest phenotype: Optimal value > 주어진 염색체가 문제의 해결책에 얼마나 근접한가
- Driving genotype: Optimal solution

phenotype 을 최적화하기 위해 genotype 찾아내는 것을 genetic algorithm 의 기본 핵심**



## Terminology and Structure of Genetic Algorithm

### 용어

- 염색체(`Chromosome`): 생물학적으로 유전 물질을 담고 있는 하나의 집합을 의미. 염색체는 유전자를 포함
- 유전자(`Gene`): 염색체(chromosome)을 구성하는 하나의 유전 정보를 의미. 예를들어 A, B, C를 가지는 염색체가 있을때 A는 유전자(gene)
- 자손(`Offspring`): 특정 시간에 존재한 부모 염색체의 성질을 이어받은 다음세대 염색체들을 부모 염색체의 자손 염색체라 함(자손 염색체는 부모 염색체와 비슷한 성질을 가지게 됨)
- 적합도(`Fitness`): 각각의 염색체가 가지는 고유 값으로, 해당 문제의 해(목표값)에 얼마나 적합한지를 나타냄
- 교차율 또는 교배(`Crossover`): 2개의 염색체를 조합하여 새로운 염색체를 생성(보통 교차율은 0.7>70%로 정의)
- 돌연변이(`Mutation`): 생물학적 돌연변이와 같은 것으로 특정 확률로 염색체 또는 유전자를 변형. 변이 없이 알고리즘을 진행하다보면 여러 세대가 지나고 같은 데이터를 가진 염색체만 남는 경우가 있으며 변이를 통해 새로운 데이터에 대한 가능성을 열어둠(보통 변이율은 0.001>0.1%로 정의)


#### Structure of Genetic Algorithm

```python
class GeneticAlgorithm:
	def perform_evolution(self, num_iterations, num_off_strings):
		population = self.create_initial_population()

		for itr in range(num_iterations):
				off_string = {}

				for itr2 in range(num_off_strings):
						parent1, parent2 = self.select_parents()
						off_string[itr2] = self.cross_over_parents(parent1, parent2)
						off_string[itr2] = self.mutation(off_string[itr2])

				self.sub_stritute_population(population, offstring)

		most_fittest = self.find_best_solution(population)

		return most_fittest
```

1. `create_initial_population`(초기 염색체 집합 생성): 얼마나 진화를 진행해야하는 것인가에 대한 답은 없기 때문에 적절하게 실행
2. `select_parents`: 새로운 해답을 생성하기 위해서, 두개의 부모를 선택
3. `cross_over_parents`: 선택된 해답을 이용하여 새로운 해답을 생헝하고 부모를 만들어진 자식과 교체
4. `mutation`: 자손 염색체를 변이하여 새로운 해결책을 얻음
5. `sub_stritute_population`: 새로운 해결책을 집단에 넣어줌

- 자손을 만드는 것이 굉장히 중요하며 너무 많은 경우에는 원래의 목적을 잃어버릴 수 있기 때문에 처음에는 여러방법으로 찾아보고 어느정도 일정 해결책이 나오면 점차 자손의 수를 줄인다.
