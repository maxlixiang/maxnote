网红“菜场大妈”小市值策略，如何在本地 Python 环境下实现？（本地Python代码解析）

几乎每隔一段时间，网上都会刷到一个策略：**“菜场大妈”小市值策略** 。

策略逻辑非常朴素，但历史表现却很亮眼。因此，该策略成了各个账号反复提及的“流量担当”。

## ❌ 一个常被忽略的问题

这些推文写的都非常好，对策略分析极度到位。但是，对于一个对本地Python量化有强迫症的人来说，总觉得有点不足：

**这些推文的策略大多是在聚宽上实现的。有的做得多一点，在QMT上也实现了该策略。** 

因此，策略的运行和回测，得依赖这些第三方平台。

![](https://pic2.zhimg.com/v2-77ecd1656d0f0c59cd951d5c7fdb322d_r.jpg)

![](https://pica.zhimg.com/v2-d9fc9cca446ab505ad4a6559ba0f0b92_r.jpg)




我们是否可以在本地Python环境下实现该策略？这样，**我们就可以在自己的电脑上自由运行和修改策略.** 

答案是肯定的。

## 📌 策略核心逻辑

这个策略最早来自聚宽社区，本质并不复杂，可以概括为七个字：**质好价低市值小** 。

- **质好** ： 通常用 **股息率较高**  作为基本面筛选条件
- **价低** ： 只选择股价在一定区间内的股票，比如 **1–9 元**  （1 元以下容易触发退市风险，需剔除）
- **市值小** ： 在满足上述条件的股票中，优先选择小市值标的

此外，还需要一系列常规过滤条件：

- 剔除科创板、北交所、新三板股票
- 剔除 ST / *ST
- 剔除停牌、退市股票
- 剔除当日涨停、跌停股票

听起来条件不少，但好消息是：**这些数据在 Tushare 里基本都能拿到** ，我们完全可以在本地Python环境下实现这个策略。

```python3
# 根据股息率和市值选取今天该买的股票
# 传入前一天的基本面数据，因为今天的选股是根据昨天的基本面数据来的
def stk_select(day, pre_day, bnd_frac = 0.25):

    df = pro.daily_basic(ts_code = '', trade_date = pre_day)  
    stock_list = df['ts_code'].tolist()

    # 剔除一些不需要板块的股票： 创业板、科创板、北交所、三板
    stock_list = filter_stocks(stock_list)  
    df = df.set_index(['ts_code'])
    df = df.reindex(index=stock_list)
    
    # 将股票列表按照股息率由高到低的顺序排列
    df = df.sort_values(by='dv_ratio', ascending = False)
    # 取股息率最高的一部分股票用于选股
    df = df[:int(bnd_frac*len(df))]    
    # 将股票列表按照市值由低到高的顺序排列
    df = df.sort_values(by='total_mv', ascending = True)
    stock_list = df.index.tolist()    
    
    # 过滤ST、停牌、退市的股票
    stock_list = filter_st_suspend_delist_stocks(stock_list, day)   
    # 过滤今天涨停、跌停的股票  
    stock_list = filter_limitup_limitdown_stock(stock_list, day)
    # 过滤高股价股票
    stock_list = filter_highprice_stock(stock_list, day)    
    assert len(stock_list) != 0, print('stock list length is 0')
    
    # 保存今日选股结果文件
    file_name_stk = 'data/每日选股/' + day + '_stks.pkl'
    with open(file_name_stk, 'wb') as file:
        pickle.dump(stock_list, file)    
    return stock_list
```

## 📊  主要模块

### 1️⃣ 股息率 & 市值

可直接通过 Tushare 的`pro.daily_basic()` 接口获取：

```python3
df = pro.daily_basic(ts_code = "", trade_date = day)
```

### 2️⃣ 剔除科创 / 北交 / 三板 / 创业板

可以通过股票代码规则进行过滤：

```python3
# 过滤科创、北交、三板、创业板等板块股票
# [1] 三板：4开头
# [2] 北交：8开头
# [3] 科创：688开头
# [4] 创业板：300 / 301 开头
def filter_stocks(stock_list):
    for stock in stock_list[:]:
        if stock[0] == '4' or stock[0] == '8' or stock[:3] == '688' or \
           stock[:3] == '300' or stock[:3] == '301':
            stock_list.remove(stock)
    return stock_list
```

### 3️⃣ 剔除停牌股票

Tushare 可通过 `pro.suspend_d()` 获取某日停牌信息：

```python3
stk_suspend = pro.suspend_d(suspend_type='S',trade_date=day)
```

### 4️⃣ 剔除 ST / *ST 股票

Tushare 没有直接给ST状态，但可以通过**股票名称变更记录** 来反推，主要用到的是`pro.namechange()`接口：

```python3
# 获取每日的ST、*ST股票列表
def get_st(trade_date):
    # 获取三年前日期，因为ST最多3年
    start_date = (datetime.datetime.strptime(trade_date,'%Y%m%d') - \
                datetime.timedelta(days=365*3)).strftime('%Y%m%d')
    # 获取三年内所有更名记录
    df = pro.namechange(start_date=start_date,end_date=trade_date, \
                        fields='ts_code,name,start_date,end_date,ann_date,change_reason')
    # 筛选更名原因为ST或*ST的记录，去掉结束日期重复为None的
    df = df[(df['change_reason']=='ST') | (df['change_reason']=='*ST')] \
        .sort_values(by='end_date',ascending=True).drop_duplicates(subset=['ts_code','start_date'],keep='first')
    # 筛选目标日期在ST时间段的记录
    result = df[(df.start_date<=trade_date)&((df.end_date>=trade_date)|(df.end_date.isna()))]
    return result
```

### 5️⃣ 剔除退市股票

这里有两种实现思路。

**方式一：股票名称中是否包含“退”字** 

```python3
def filter_delist(stock_list, day):
    stock_names = get_stock_chinese_name(stock_list, day)
    stock_list = [
        code for code, name in zip(stock_list, stock_names)
        if '退' not in name
    ]
    return stock_list
```

**方式二：成交量为 0（简单粗暴，但不一定是退市导致）** 

```python3
def filter_delist(stock_list, day):
    temp = pro.daily(trade_date=day)
    temp = temp.set_index('ts_code').reindex(stock_list)
    temp = temp[temp['vol'] == 0]
    return [stk for stk in stock_list if stk not in temp.index]
```

### 6️⃣ 剔除涨停 / 跌停股票

通过 `pro.stk_limit()` 获取涨跌停价，对比当日收盘价即可：

```python3
def filter_limitup_limitdown_stock(stock_list, trade_date):
    df_price = pro.daily(trade_date=trade_date).set_index('ts_code')
    df_limit = pro.stk_limit(trade_date=trade_date).set_index('ts_code')
    df_limit = df_limit.reindex(df_price.index)

    limit_stocks = [
        stk for stk in df_price.index
        if df_price.loc[stk, 'close'] == df_limit.loc[stk, 'up_limit']
        or df_price.loc[stk, 'close'] == df_limit.loc[stk, 'down_limit']
    ]

    return [stk for stk in stock_list if stk not in limit_stocks]
```

## 🚀 本地回测

到这里，**选股逻辑已经完整闭环** 。接下来要做的，就是：

- 将上述筛选流程模块化
- 生成每日选股结果
- 基于本地数据
- 使用 **Backtrader**  对策略进行回测

![](https://pic4.zhimg.com/v2-db4787e542c2e0a9d782e9176476a43d_r.jpg)

这样，我们就可以：完全脱离第三方平台，自由修改策略逻辑，任意组合因子，构建属于自己的本地量化体系。



---

## 评论

共 7 条评论，已存 7 条

### 秋水12345

数据用哪个呢？数据要收费吧

2026-01-19 上海 1 赞

> #### Python本地量化
> 
> Tushare 
> 
> 2026-01-19 广东 0 赞

### 晨旭宝宝

写的真不错加油加油[赞同]

2026-01-19 河南 0 赞

> #### Python本地量化
> 
> 谢谢！
> 
> 2026-01-19 广东 0 赞

### 秋水12345

是需要收费版的吗？免费版的提供不了这么全面的数据吧

2026-01-19 上海 0 赞

> #### Python本地量化
> 
> 2000积分的就可以，稳妥点的话最好5000积分以上。搞量化，还是得花点钱吧
> 
> 2026-01-19 广东 0 赞
> 
> #### 秋水12345 › Python本地量化
> 
> 了解了，多谢不吝赐教[握手]
> 
> 2026-01-19 上海 0 赞
