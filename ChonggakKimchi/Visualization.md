from <a href='https://www.kaggle.com/alexisbcook/choosing-plot-types-and-custom-styles'>here</a>

## Trend

### sns.lineplot
- sns.lineplot은, 시간에 따른 경향을 보여주기에 적합하고, 여러개의 line은 여러 대상의 시간에 따른 경향을 보여주기에 적합하다.

## Relationship

### sns.barplot
- sns.barplot은, 다양한 집단의 '수량'을 비교할 때 유용하다.

### sns.heatmap
- sns.heatmap은, 숫자들의 table에서 색상이 있는 패턴들을 확인할 때 사용된다.

### sns.scatterplot
- sns.scatterplot은, 두개의 이어지는 변수의 관계를 보여준다. 색상을 나눌 경우 범주형 변수 하나가 추가된다.

### sns.regplot
- sns.regplot은, scatterplot에 회귀선을 추가하여 두 변수 사이의 관계를 선으로 나타내줘서 좀 더 보기 편하다.

### sns.lmplot
- sns.lmplot은, 여러 개의 회귀선을 그릴 때 유용하다. (scatterplot이 여러 개의 group으로 이뤄져 있을 때)

### sms.swarmplot
- sns.swarmplot은, 범주를 구분하는 변수와 연속변수 사이의 관계를 보여준다.

## Ditribution

### sns.distplot  
- sns.distplot은, 한 가지 수량 변수의 분포를 보여준다.

### sns.kdeplot
- sns.kdeplot은, 한 가지 수량 변수(또는 그 이상)의 분포를 부드럽게 보여준다.

### sns.jointplot
- sns.jointplot은, 각 개인 변수와 일치하는 kde plot을 2차원의 형태로 보여준다.