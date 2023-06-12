## 一、项目名称

基于Pyspark和线性回归的股票数据分析预测

## 二、项目背景

+ 随着互联网和数字化技术的发展，金融行业正经历着前所未有的变革，机器学习和人工智能等技术被广泛应用于股票市场预测、风险控制和投资决策等领域。
+ 苹果公司作为全球知名的科技企业，其股票价格对整个股市有很大的影响，因此准确预测苹果公司股票价格趋势对投资者和分析师具有重要意义。
+ 苹果公司股票交易数据规模较大，传统的数据处理方式可能无法处理如此海量的数据。

## 三、项目介绍

**目的**

+ 分析苹果公司的股票数据
+ 预测苹果公司股票价格趋势、评估预测模型

**算法描述**

+ 在本次数据分析中，采用了基于回归的机器学习方法进行股票价格预测。

+ 使用了线性回归算法，该算法是一种基于最小二乘法的回归分析方法，通过寻找最佳拟合直线，将自变量和因变量之间的关系进行建模。

+ 使用了历史收盘价作为自变量，预测未来一个交易日的收盘价作为因变量。
+ 使用了均方误差（MSE）作为模型的性能评估指标，MSE越小，模型的预测准确度越高。

**预期结果**

+ 生成股票价格图
+ 和苹果公司相关性最高的五只股票
+ 和苹果公司相关性最差的五只股票
+ 评估预测能力，计算评估指标MSE

**主要工作流程**

+ 数据源：该数据集是纽约证券交易所（NYSE）和纳斯达克市场上所有股票在五年期间（2013年至2018年）交易的历史记录。数据集包括每个交易日的开盘价、收盘价、最高价、最低价、交易量和股票名称等信息。

  ![image-20230611203803310](../../../AppData/Roaming/Typora/typora-user-images/image-20230611203803310.png)

+ 数据探索和可视化：对比苹果公司与其他公司股票价格的相关性，使用pandas库计算苹果公司股票价格与其他公司股票价格的相关系数，并可视化展示结果；

  ![image-20230611202623845](../../../AppData/Roaming/Typora/typora-user-images/image-20230611202623845.png)



+ 相关性分析：使用Grid Search等方法对模型进行调优，提高准确性；然后计算多个公司股票价格的相关性并保存Top5正相关和Top5负相关的结果，将结果保存为CSV格式的文件。

  ![image-20230611202826514](../../../AppData/Roaming/Typora/typora-user-images/image-20230611202826514.png)

+ 数据获取和处理：使用pandas库读取苹果公司股票数据，并进行数据清洗和预处理，包括数据缺失值填充、异常值处理和数据类型转换等；

  ![image-20230611203509716](../../../AppData/Roaming/Typora/typora-user-images/image-20230611203509716.png)

+ 特征工程和模型训练：使用scikit-learn库对数据进行特征工程，然后基于前7天的苹果公司股票价格数据，使用多元线性回归模型预测未来股票价格，并使用MSE指标评估模型的预测能力；

  ![image-20230611203641660](../../../AppData/Roaming/Typora/typora-user-images/image-20230611203641660.png)

+ Spark分布式计算：使用pyspark这个强大的分布式计算框架来处理苹果公司股票交易数据、计算相关系数、优化模型等任务。

  ![image-20230611202745563](../../../AppData/Roaming/Typora/typora-user-images/image-20230611202745563.png)

## 四、总结和展望

+ 本项目通过使用pandas和pyspark等强大的数据处理工具，实现了经典的数据清洗、统计学方法、特征工程和多元线性回归建模等机器学习算法，有效提高了苹果公司股票价格的预测准确度。
+ 在未来，可以进一步扩展模型深度、优化算法和增加数据来源等手段，以提高模型的稳定性和预测精度，并针对投资者和分析师提供更具有实践意义的股票预测服务。
+ 此外，使用其他pyspark MLlib中的算法，如支持向量机（SVM）、随机森林（Random Forest）等，对苹果公司股票价格进行预测并进行对比分析，以得出更为准确和可靠的结论。
+ 最终，我们可以将项目集成到Web应用程序中，为投资者和分析师提供实时、准确的股票预测服务。















## 项目代码

##### 1、analysis.py

```python
from io import BytesIO
import matplotlib.pyplot as plt
import pandas as pd
from pyspark.sql import SparkSession
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error

from visualizer import Visualizer
from correlation_calculator import CorrelationCalculator
from stock_predictor import StockPredictor


class Analysis:
    """
    进行数据分析的类
    """

    def __init__(self):
        """
        初始化SparkSession和股票数据集
        """
        self.spark = SparkSession.builder.appName("Analyse").getOrCreate()
        self.sp500 = self.spark.read.format("csv") \
            .option("header", "true") \
            .option("inferSchema", "true") \
            .load("hdfs://namenode:9000/work/all_stocks_5yr.csv")

    def run(self):
        """
        运行数据分析任务
        """
        self.visualize_stock_prices()
        self.calculate_correlation_with_apple()
        self.predict_stock_price()
        
    def shutdown(self):
        """
        销毁该对象时需要关闭SparkSession
        """
        self.spark.stop()

    def visualize_stock_prices(self):
        """
        绘制苹果公司股票价格的图表，并将生成的图片保存到内存，将图片写入到HDFS中指定的路径
        """
        visualizer = Visualizer(self.sp500, self.spark)
        visualizer.visualize_stock_prices('AAPL', '2014-01-01', '2016-12-31', 'hdfs://namenode:9000/work/apple_stock_prices.png',
                                          'Apple Stock Prices')

    def calculate_correlation_with_apple(self):
        """
        计算苹果公司股票价格与其他公司股票价格的相关系数，选出正相关系数最高的5只股票和负相关系数最高的5只股票，
        并将结果保存到HDFS中指定的路径
        """
        calculator = CorrelationCalculator(self.sp500, self.spark)
        calculator.calculate_correlation_with_apple('AAPL', 'hdfs://namenode:9000/work/positive_correlation.csv',
                                                    'hdfs://namenode:9000/work/negative_correlation.csv')

    def predict_stock_price(self):
        """
        对苹果公司股票数据进行预测，训练线性回归模型，并在测试集上评估性能，将结果保存到HDFS中指定的路径
        """
        predictor = StockPredictor(self.sp500, self.spark)
        predictor.predict_stock_price('AAPL', 0.8, 'hdfs://namenode:9000/work/linear_regression_result.txt')
        

        
def main():
    """
    创建一个Analysis对象
    """
    analysis = Analysis()

    """
    调用run方法运行整个数据分析流程
    """
    analysis.run()

    """
    销毁该对象时需要关闭SparkSession
    """
    analysis.shutdown()


if __name__ == '__main__':
    main()
```

##### 2、visualizer.py

```python
from io import BytesIO
import matplotlib.pyplot as plt


class Visualizer:
    """
    可视化类，用来绘制股票价格图表
    """

    def __init__(self, sp500, spark):
        """
        初始化需要用到的数据集和SparkSession
        :param sp500: 股票数据集
        :param spark: SparkSession
        """
        self.sp500 = sp500
        self.spark = spark

    def visualize_stock_prices(self, name, start_date, end_date, output_path, title):
        """
        绘制股票价格图表，并将生成的图片保存到内存中，将图片写入到HDFS中指定的路径
        :param name: 股票名称
        :param start_date: 起始日期
        :param end_date: 结束日期
        :param output_path: 输出路径
        :param title: 图表标题
        """
        filtered_data = self.sp500.filter((self.sp500['Name'] == name) &
                                          (self.sp500['date'] >= start_date) &
                                          (self.sp500['date'] <= end_date))
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
        plt.title(title)
        plt.legend()
        plt.xticks(selected_dates, rotation=45)

        # 将生成的图片保存到内存中
        buf = BytesIO()
        plt.savefig(buf, format='png', dpi=600)
        buf.seek(0)

        # 将图片写入到HDFS中指定的路径
        fs = self.spark._jvm.org.apache.hadoop.fs.FileSystem.get(self.spark._jsc.hadoopConfiguration())
        output_stream = fs.create(self.spark._jvm.org.apache.hadoop.fs.Path(output_path))
        output_stream.write(bytearray(buf.read()))
        output_stream.close()

        plt.close()
```

##### 3、correlation_calculator.py

```python
import pandas as pd


class CorrelationCalculator:
    """
    相关系数计算类，用于计算股票的相关系数并保存到文件中
    """

    def __init__(self, sp500, spark):
        """
        初始化需要用到的数据集和SparkSession
        :param sp500: 股票数据集
        :param spark: SparkSession
        """
        self.sp500 = sp500
        self.spark = spark

    def calculate_correlation_with_apple(self, apple_name, positive_output_path, negative_output_path):
        """
        计算苹果公司股票价格与其他公司股票价格的相关系数，选出正相关系数最高的5只股票和负相关系数最高的5只股票，
        并将结果保存到HDFS中指定的路径
        :param apple_name: 苹果公司股票名称
        :param positive_output_path: 正相关系数结果输出路径
        :param negative_output_path: 负相关系数结果输出路径
        """
        sp500_cleaned = self.sp500.dropna()
        apple_data = sp500_cleaned.filter(sp500_cleaned['Name'] == apple_name)
        apple_close = apple_data.select('open', 'high', 'low', 'close').toPandas().reset_index(drop=True)

        correlation_df = pd.DataFrame(columns=['Stock', 'Correlation'])
        for stock in sp500_cleaned.select('Name').distinct().rdd.flatMap(lambda x: x).collect():
            if stock != apple_name:
                stock_data = sp500_cleaned.filter(sp500_cleaned['Name'] == stock)
                stock_close = stock_data.select('open', 'high', 'low', 'close').toPandas().reset_index(drop=True)
                correlation = apple_close.corrwith(stock_close).mean()
                correlation_df = pd.concat([correlation_df, pd.DataFrame({'Stock': [stock], 'Correlation': [correlation]})],
                                           ignore_index=True)

        top_positive_correlation = correlation_df.sort_values(by='Correlation', ascending=False).head(5)
        top_negative_correlation = correlation_df.sort_values(by='Correlation', ascending=True).head(5)

        # 将结果保存到HDFS中指定的路径
        fs = self.spark._jvm.org.apache.hadoop.fs.FileSystem.get(self.spark._jsc.hadoopConfiguration())

        # 在HDFS中创建输出文件
        output_stream = fs.create(self.spark._jvm.org.apache.hadoop.fs.Path(positive_output_path))
        output_stream.write(bytearray(top_positive_correlation.to_csv(index=False).encode()))
        output_stream.close()

        output_stream = fs.create(self.spark._jvm.org.apache.hadoop.fs.Path(negative_output_path))
        output_stream.write(bytearray(top_negative_correlation.to_csv(index=False).encode()))
        output_stream.close()
```

##### 4、stock_predictor.py

```python
from pyspark.sql import functions as F
from pyspark.sql.window import Window
import pandas as pd
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error


class StockPredictor:
    """
    股票预测类，使用线性回归模型进行预测并保存结果到文件中
    """

    def __init__(self, sp500, spark):
        """
        初始化需要用到的数据集和SparkSession
        :param sp500: 股票数据集
        :param spark: SparkSession
        """
        self.sp500 = sp500
        self.spark = spark

    def predict_stock_price(self, name, train_ratio, output_path):
        """
        对股票价格进行预测，训练线性回归模型，并在测试集上评估性能，将结果保存到HDFS中指定的路径
        :param name: 股票名称
        :param train_ratio: 训练集比例
        :param output_path: 输出路径
        """
        sp500_filled = self.sp500.fillna({'Close': 0})

        # 筛选出苹果公司（AAPL）股票数据并添加滞后特征
        stock_data = sp500_filled.filter(sp500_filled['Name'] == name)
        stock_data = stock_data.withColumn('Close_shifted', F.lag(stock_data['Close']).over(
            Window.partitionBy('Name').orderBy('date')))
        for i in range(1, 8):
            stock_data = stock_data.withColumn(f'Close_{i}', F.lag(stock_data['Close'], i).over(
                Window.partitionBy('Name').orderBy('date')))
        stock_data = stock_data.dropna()

        # 将DataFrame转换为Pandas DataFrame，并提取特征和标签数据
        pdf = stock_data.select('Close_1', 'Close_2', 'Close_3', 'Close_4', 'Close_5', 'Close_6', 'Close_7',
                                'Close_shifted').toPandas()
        X = pdf.iloc[:, :-1]
        y = pdf.iloc[:, -1]

        # 划分数据集为训练集和测试集
        train_size = int(len(pdf) * train_ratio)
        X_train, X_test = X[:train_size], X[train_size:]
        y_train, y_test = y[:train_size], y[train_size:]

        # 训练线性回归模型并进行预测
        lr = LinearRegression().fit(X_train, y_train)
        y_pred_test = lr.predict(X_test)

        # 在测试集上计算均方误差并将结果保存到HDFS中指定的路径
        mse_test = mean_squared_error(y_test, y_pred_test)

        # 将结果保存到 HDFS 中指定的路径
        fs = self.spark._jvm.org.apache.hadoop.fs.FileSystem.get(self.spark._jsc.hadoopConfiguration())
        output_stream = fs.create(self.spark._jvm.org.apache.hadoop.fs.Path(output_path))
        output_stream.write(bytearray(f"mean squared error: {mse_test}".encode()))
        output_stream.close()
```

