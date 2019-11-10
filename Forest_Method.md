# Forest Method
* 숲같이 되어있는 방법
* Classification에 많이 쓰임

## Decision Tree
* Root Node (라벨, 클레스, dependent variable, 종속변수 등등) > Intermediate Node (root에서 True/False에따라 나눈다) > Leaf (순도가 매우 오름)
* Leaf를 더 많은 단위로 쪼갤 수 있으나, 시각화에도 어렵고 overfitting할 가능성이 있음
    * hyperparameter: 머신러닝에서 기계가 못 찾고 인간이 정해줘야하는 구간

sklearn Code
```py
tree = DecisionTreeClassifier(random_state=42, max_depth=2, max_leaf_nodes=4)

tree = tree.fit(train_x, train_y)
tree
```
```py
forest = RandomForestClassifier(random_state=42, n_estimators=100, max_features='auto', n_jobs=2, verbose=True)

forest = forest.fit(train_x, train_y)
forest
```