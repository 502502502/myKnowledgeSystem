

#### 一、初始代码整理

```python
#############################################################################
from hdfs import InsecureClient
import pandas as pd

# 创建 HDFS 客户端
client = InsecureClient('http://namenode:9870', user='hadoop')

# 从 HDFS 中读取 CSV 文件
with client.read('/work/all_stocks_5yr.csv', encoding='utf-8') as reader:
    # 将 CSV 数据加载到 Pandas 数据框中
    sp500 = pd.read_csv(reader)
    

print(sp500['Name'].unique())

print(sp500['date'].min(), sp500['date'].max())



#############################################################################
import matplotlib.pyplot as plt

filtered_data = sp500[(sp500['Name'] == 'AAPL') & (sp500['date'] >= '2014-01-01') & (sp500['date'] <= '2016-12-31')]

date = filtered_data['date']
ope = filtered_data['open']
high = filtered_data['high']
low = filtered_data['low']
close = filtered_data['close']

plt.plot(date, ope, label='Open')
plt.plot(date, high, label='High')
plt.plot(date, low, label='Low')
plt.plot(date, close, label='Close')

dates = filtered_data['date'].unique()
step = len(dates) // 10
selected_dates = dates[::step]

plt.title('Apple Stock Prices')
plt.legend()
plt.xticks(selected_dates, rotation=45)

plt.show()



#############################################################################
sp500 = pd.read_csv("all_stocks_5yr.csv")

sp500_cleaned = sp500.dropna()

aapl_data = sp500_cleaned[sp500_cleaned['Name'] == 'AAPL']
aapl_close = aapl_data[['open', 'high', 'low', 'close']].reset_index(drop=True)

correlation_df = pd.DataFrame(columns=['Stock', 'Correlation'])

for stock in sp500_cleaned['Name'].unique():
    if stock != 'AAPL':
        stock_data = sp500_cleaned[sp500_cleaned['Name'] == stock]
        stock_close = stock_data[['open', 'high', 'low', 'close']].reset_index(drop=True)
        correlation = aapl_close.corrwith(stock_close).mean()
        correlation_df = pd.concat([correlation_df, pd.DataFrame({'Stock': [stock], 'Correlation': [correlation]})], ignore_index=True)

top_positive_correlation = correlation_df.sort_values(by='Correlation', ascending=False).head(5)
top_negative_correlation = correlation_df.sort_values(by='Correlation', ascending=True).head(5)

print("Top 5 stocks that have the highest positive correlation with Apple:")
print(top_positive_correlation)

print("\nTop 5 stocks that have the highest negative correlation with Apple:")
print(top_negative_correlation)



#############################################################################
sp500=pd.read_csv("all_stocks_5yr.csv")
sp500_filled = sp500.fillna(method='ffill')

apple = sp500_filled[sp500_filled['Name'] == 'AAPL']

apple = apple.copy()
apple.loc[:, 'Close_shifted'] = apple.loc[:, 'close'].shift(-1)
for i in range(1, 8):
    apple.loc[:, f'Close_{i}'] = apple.loc[:, 'close'].shift(i)
apple.dropna(inplace=True)



#############################################################################
from sklearn.model_selection import train_test_split

X = apple[['Close_1', 'Close_2', 'Close_3', 'Close_4', 'Close_5', 'Close_6', 'Close_7']]
y = apple['Close_shifted']

train_size = int(len(X) * 0.8)
X_train, X_test, y_train, y_test = X[:train_size], X[train_size:], y[:train_size], y[train_size:]



#############################################################################
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error

model = LinearRegression()
model.fit(X_train, y_train)
predictions = model.predict(X_test)

mse = mean_squared_error(y_test, predictions)
print("mean squared error: ", mse)
```



#### 二、Pyspark代码转换

```python
# 导入必要的模块
from io import BytesIO
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error
from pyspark.sql import SparkSession
from pyspark import SparkConf, SparkContext, SQLContext, HiveContext
from pyspark.sql import functions as F
from pyspark.sql.window import Window





#************************************************************************************
#1.
# 创建 SparkSession
spark = SparkSession.builder.appName("Analyse").getOrCreate()

# 从 HDFS 中读取 CSV 文件
sp500 = spark.read.format("csv")\
    .option("header", "true")\
    .option("inferSchema", "true")\
    .load("hdfs://namenode:9000/work/all_stocks_5yr.csv")

#************************************************************************************
#2.
# 绘制苹果公司股票价格的图表
filtered_data = sp500.filter((sp500['Name'] == 'AAPL') & (sp500['date'] >= '2014-01-01') & (sp500['date'] <= '2016-12-31'))
date = filtered_data.select('date').rdd.flatMap(lambda x: x).collect()
ope = filtered_data.select('open').rdd.flatMap(lambda x: x).collect()
high = filtered_data.select('high').rdd.flatMap(lambda x: x).collect()
low = filtered_data.select('low').rdd.flatMap(lambda x: x).collect()
close = filtered_data.select('close').rdd.flatMap(lambda x: x).collect()
plt.plot(date, ope, label='Open')
plt.plot(date, high, label='High')
plt.plot(date, low, label='Low')
plt.plot(date, close, label='Close')
dates = filtered_data.select('date').distinct().rdd.flatMap(lambda x: x).collect()
step = len(dates) // 10
selected_dates = dates[::step]
plt.title('Apple Stock Prices')
plt.legend()
plt.xticks(selected_dates, rotation=45)

# 将生成的图片保存到内存中
buf = BytesIO()
plt.savefig(buf, format='png', dpi=600)
buf.seek(0)

# 将图片写入到 HDFS 中指定的路径
fs = spark._jvm.org.apache.hadoop.fs.FileSystem.get(spark._jsc.hadoopConfiguration())
output_path = "hdfs://namenode:9000/work/apple_stock_prices.png"
output_stream = fs.create(spark._jvm.org.apache.hadoop.fs.Path(output_path))
output_stream.write(bytearray(buf.read()))
output_stream.close()

plt.close()

#************************************************************************************
#3.
# 计算苹果公司股票价格与其他公司股票价格的相关系数
sp500_cleaned = sp500.dropna()
aapl_data = sp500_cleaned.filter(sp500_cleaned['Name'] == 'AAPL')
aapl_close = aapl_data.select('open', 'high', 'low', 'close').toPandas().reset_index(drop=True)

correlation_df = pd.DataFrame(columns=['Stock', 'Correlation'])
for stock in sp500_cleaned.select('Name').distinct().rdd.flatMap(lambda x: x).collect():
    if stock != 'AAPL':
        stock_data = sp500_cleaned.filter(sp500_cleaned['Name'] == stock)
        stock_close = stock_data.select('open', 'high', 'low', 'close').toPandas().reset_index(drop=True)
        correlation = aapl_close.corrwith(stock_close).mean()
        correlation_df = pd.concat([correlation_df, pd.DataFrame({'Stock': [stock], 'Correlation': [correlation]})], ignore_index=True)

top_positive_correlation = correlation_df.sort_values(by='Correlation', ascending=False).head(5)
top_negative_correlation = correlation_df.sort_values(by='Correlation', ascending=True).head(5)

# 将结果保存到 HDFS 中指定的路径
fs = spark._jvm.org.apache.hadoop.fs.FileSystem.get(spark._jsc.hadoopConfiguration())
positive_output_path = 'hdfs://namenode:9000/work/positive_correlation.csv'
negative_output_path = 'hdfs://namenode:9000/work/negative_correlation.csv'

# 在 HDFS 中创建输出文件
output_stream = fs.create(spark._jvm.org.apache.hadoop.fs.Path(positive_output_path))
output_stream.write(bytearray(top_positive_correlation.to_csv(index=False).encode()))
output_stream.close()

output_stream = fs.create(spark._jvm.org.apache.hadoop.fs.Path(negative_output_path))
output_stream.write(bytearray(top_negative_correlation.to_csv(index=False).encode()))
output_stream.close()


#************************************************************************************
#4.
sp500_filled = sp500.fillna({'Close': 0})

# 筛选出苹果公司（AAPL）股票数据并添加滞后特征
apple = sp500_filled.filter(sp500_filled['Name'] == 'AAPL')
apple = apple.withColumn('Close_shifted', F.lag(apple['Close']).over(Window.partitionBy('Name').orderBy('date')))
for i in range(1, 8):
    apple = apple.withColumn(f'Close_{i}', F.lag(apple['Close'], i).over(Window.partitionBy('Name').orderBy('date')))
apple = apple.dropna()

# 将 DataFrame 转换为 Pandas DataFrame，并提取特征和标签数据
pdf = apple.select('Close_1', 'Close_2', 'Close_3', 'Close_4', 'Close_5', 'Close_6', 'Close_7', 'Close_shifted').toPandas()
X = pdf.iloc[:, :-1]
y = pdf.iloc[:, -1]

# 划分训练集和测试集
train_size = int(len(X) * 0.8)
X_train, X_test, y_train, y_test = X.iloc[:train_size, :], X.iloc[train_size:, :], y.iloc[:train_size], y.iloc[train_size:]

# 训练线性回归模型，并在测试集上评估性能
model = LinearRegression()
model.fit(X_train, y_train)
predictions = model.predict(X_test)
mse = mean_squared_error(y_test, predictions)

# 将结果保存到 HDFS 中指定的路径
fs = spark._jvm.org.apache.hadoop.fs.FileSystem.get(spark._jsc.hadoopConfiguration())
output_path = "hdfs://namenode:9000/work/linear_regression_result.txt"
output_stream = fs.create(spark._jvm.org.apache.hadoop.fs.Path(output_path))
output_stream.write(bytearray(f"mean squared error: {mse}".encode()))
output_stream.close()
```



#### 三、Hadoop部署

###### 1、数据集上传hdfs

```shell
#创建目录
hdfs dfs -mkdir /work
hdfs dfs -mkdir /work/out
hdfs dfs -ls /
#上传文件
hdfs dfs -put all_stocks_5yr.csv /work
```

###### 2、集群机器安装anaconda，创建虚拟环境

```shell
#下载
wget https://repo.continuum.io/archive/Anaconda3-2022.10-Linux-x86_64.sh
bash Anaconda3-2022.10-Linux-x86_64.sh -p anaconda/ -u
conda -version
#环境变量配置
vi ~/.bashrc
export PATH=$PATH:/home/hadoop/anaconda/anaconda/envs/work/lib/python3.10/site-packages
export PATH=/home/hadoop/anaconda/anaconda/bin:$PATH
source ~/.bashrc
#创建虚拟环境
conda create --name work python=3.10 scikit-learn pandas matplotlib pyspark=3.1.2
conda activate work
```

###### 3、打开jupeter，打开.ipynb文件，选工作核心

```shell
#启动集群
$SPARK_HOME/sbin/start-all.sh
start-dfs.sh

#安装核心
pip install ipykernel
python -m ipykernel install --user --name=work

#打开jupyter
jupyter notebook

#清除文件 
hdfs dfs -rm /work/apple_stock_prices.png
hdfs dfs -rm /work/linear_regression_result.txt
hdfs dfs -rm /work/positive_correlation.csv
hdfs dfs -rm /work/negative_correlation.csv
```

增加了pyspark修改后的代码

###### 4、从hdfs下载结果，查看

```shell
#下载文件
hadoop fs -get /work/apple_stock_prices.png /home/hadoop/data/work/out/apple_stock_prices.png
hadoop fs -get /work/linear_regression_result.txt /home/hadoop/data/work/out/inear_regression_result.txt
hadoop fs -get /work/positive_correlation.csv /home/hadoop/data/work/out/positive_correlation.csv
hadoop fs -get /work/negative_correlation.csv /home/hadoop/data/work/out/negative_correlation.csv

#关闭集群
stop-dfs.sh
$SPARK_HOME/sbin/stop-all.sh
```



#### 四、演示操作

```shell
#激活环境
conda activate work
#启动集群
$SPARK_HOME/sbin/start-all.sh
start-dfs.sh
#清除hdfs文件
hdfs dfs -rm /work/apple_stock_prices.png
hdfs dfs -rm /work/linear_regression_result.txt
hdfs dfs -rm /work/positive_correlation.csv
hdfs dfs -rm /work/negative_correlation.csv

#打开hadoop-web
namenode:9870
#打开jupyter
jupyter notebook
anaconda/work/**.ipynb
#运行程序
5min
#从hdfs下载文件
hadoop fs -get /work/apple_stock_prices.png /home/hadoop/data/work/out/apple_stock_prices.png
hadoop fs -get /work/linear_regression_result.txt /home/hadoop/data/work/out/inear_regression_result.txt
hadoop fs -get /work/positive_correlation.csv /home/hadoop/data/work/out/positive_correlation.csv
hadoop fs -get /work/negative_correlation.csv /home/hadoop/data/work/out/negative_correlation.csv
#查看
data/work/out/*
#关闭集群
stop-dfs.sh
$SPARK_HOME/sbin/stop-all.sh
```

