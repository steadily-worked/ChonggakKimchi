from https://www.kaggle.com/dansbecker/your-first-machine-learning-model

column만 출력하려면 -> (data).columns

dropna -> missing values를 없애주는 기능.(na = 'not available')

(data) = (data).dropna(axis=0)

'dot notation' -> 우리가 예측하고자 하는 column을 선택하기 위함.

ex) y = (data(melbourne_data)).(column(Price))

특정 데이터만 나타내려면 -> (나타낼 데이터 호칭) = ['column A', 'column B' .... ] 를 하고 난 후에

X = (data(melbourne_data))[(나타낼 데이터 호칭)]

## Building My Model

scikit-learn(싸이킷 런)을 이용해서, 모델을 만들 수 있다.

- Define : 어떤 종류의 모델이 될지..!

- Fit : 주어진 데이터에서 패턴을 뽑아내는 과정. 모델링의 핵심

- Predict : 어떻게 보이는지? Just what it sounds like

- Evaluate : 모델에 대한 예측이 얼마나 정확한지 결정하는 과정.

* from sklearn.tree import DecisionTreeRegressor


# 모델을 정해라. 매번 돌릴 때마다 같은 결과를 만들기 위해 random_state를 위한 숫자를 정해라. Specifying a number for random_state eusures you get the same results in each run.

melbourne_model = DecisionTreeRegressor(random_state=1)

# Fit model하는 과정.

melbourne_model.fit(X, y)

## Model Validation

1. prediction error for each house is:<br> error = actual - predicted

<mean absolute error를 계산하는 과정>

2. from sklearn.metrics import mean_absolute_error로 import하기

3. predicted_home_prices = melbourne_model.predict(X)<br>mean_absolute_error(y, predicted_home_prices)

<데이터를 두 개로 나누는 과정(training and validation data)>

1. from sklearn.model_selection import train_test_split

2. train_X, val_X, train_y, val_y = train_test_split(X, y, random_state = 0)

3. <Define model> melbourne_model = DecisionTreeRegressor()

4. <Fit model> melbourne_model.fit(train_X, train_y)

5. <get predicted prices on validation data> val_predictions = melbourne_model.predict(val_X)<br>print(mean_absolute_error(val_y, val_predictions))

... 즉 val_X에 대한 예측을 만들고(val_predictions) val_y와 val_predictions 사이의 error의 정도를 출력하는 과정인 것임.


## Underfitting and Overfitting

1. Overfitting : 각 가지에 대해 많은 잎들이 있을 때 -> 각 잎마다는 그에 해당하는 데이터의 양이 적을 것임. 이러한 상황에서의 예측은, 실제 데이터의 값과 거의 비슷하겠지만, 새로운 데이터에 대한 예측은 매우 부정확할 것임 (왜냐하면 각 에측이 얼마 없는 데이터들에 기반하기 때문)

2. Underfitting : 각 가지에 대해 잎이 조금밖에 없는 경우 -> 각 잎마다에는 매우 많은 양의 데이터가 저장되어 있을 것임. 그래서, 심지어 훈련 데이터에 대한 예측 결과는 대부분의 데이터에서 멀리 떨어져 있을 수 있다.(그래서 validation도 bad)

3. max_leaf_nodes를 통해, overfitting vs underfitting을 통제할 수 있음. 모델이 더 많은 잎을 만들 수 있게 함으로써, 데이터를 underfitting 영역에서 overfitting 영역으로 이동하게 할 수 있다. 함수를 통해 각자 다른 max_leaf_nodes의 값으로부터 MAE 점수를 비교할 수 있다.

4. sklearn.metrics import mean_absolute_error<br>sklearn.tree import DecisionTreeRegressor

def get_mae(max_leaf_nodes, train_X, val_X, train_y, val_y):

    model = DecisionTreeRegressor(max_leaf_nodes = max_leaf_nodes, random_state = 0)

    model.fit(train_X, train_y)

    preds_val = model.predict(val_X)

    mae = mean_absolute_error(val_y, preds_val)

    return(mae)

데이터는 전부터 써왔던 코드로 인해 train_X, val_X, train_y, val_y로 로드되었음.

5. #compare MAE with differing values of max_leaf_nodes

for max_leaf_nodes in [5, 50, 500, 5000]:
    
    my_mae = get_mae(max_leaf_nodes, train_X, val_X, train_y, val_y)

    print("Max leaf nodes: %d \t\t Mean Absolute Error: %d" % (max_leaf_nodes, my_mae))

    하면..


Max leaf nodes: 5  		 Mean Absolute Error:  347380

Max leaf nodes: 50  		 Mean Absolute Error:  258171

Max leaf nodes: 500  		 Mean Absolute Error:  243495

Max leaf nodes: 5000  		 Mean Absolute Error:  254983

가 나온다. 여기서는 Max leaf nodes가 500일 때 MAE의 값이 최저이므로 500이 최적의 잎의 양이다.

# Conclusion of Underfitting and Overfitting

1. Overfitting : 미래에 다시 발생하지 않을 가짜 패턴들을 capture함으로써, 덜 정확한 예측을 만들게 된다.

2. Underfitting : 관련있는 패턴들에 대한 capture를 실패함으로써, 또다시 덜 정확한 예측을 만들게 된다.

## Random Forests

1. 잎이 많은 나무는, 각 예측이 각 잎에 있는 '몇개 없는 데이터에서' 추출되기 때문에 overfit에 해당한다. 반대로 잎이 몇개 없는 나무 또한, 날것의 데이터에서 많은 구별을 가져올 수 없기 때문에 제대로 수행할 수 없다.

2. 이러한 문제들로 인해 perform better할 수 없는데, 이를 가능하게 해 줄 여러가지 idea가 있다. 그 중 하나가 random forest.

3. random forest는 많은 나무를 이용하고, 각 나무들에 대한 예측을 평균을 내서 예측을 만든다. 이 예측은 single decision tree에 비해 훨씬 더 나은 예측 정확도를 갖고 있다.

4. random forest는, DecisionTreeRegressor 대신에 RandomForestRegressor를 사용한다.

5. from sklearn.ensemble import RandomForestRegressor<br>from sklearn.metrics import mean_absolute_error

forest_model = RandomForestRegressor(random_state = 1)

forest_model.fit(train_X, train_y)

melb_preds = forest_model.predict(val_X)

print(mean_absolute_error(val_y, melb_preds))


<val_X에 random forest 모델을 이용한 예측값과, fit을 통한 val_y의 값의 MAE를 구하는 과정인 것>