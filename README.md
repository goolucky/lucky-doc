# Lucky 
lucky 是一个开源的加密货币的量化交易软件，对接币安交易所，简单易用。当前版本只支持动态平衡策略，后期会逐步增加策略。

## 什么是动态平衡策略
可以参考这几篇文章  
http://blog.sina.com.cn/s/blog_dbbf2b3b0102ywlm.html  
https://zhuanlan.zhihu.com/p/28397113  

## 下载
Windows版本  
https://github.com/goolucky/lucky-doc/raw/main/files/lucky_windows.zip

Linux版本  
https://github.com/goolucky/lucky-doc/raw/main/files/lucky_linux.zip

## 获取Api key和 Sercret key

从币安APP获取

![](https://raw.githubusercontent.com/goolucky/lucky-doc/main/files/01.png)


## 运行
运行程序前，需要修改配置文件。解压上面下载的文件后，修改文件`lucky.yaml`（注意yaml文件的格式，每个冒号后面有空格）
```
key:
  apikey: aaaaaaaa     // 这里改成你币安账号的 Api key
  secretkey: bbbbbbbb  // 这里改成你币安账号的 Sercret key

markets:
  - symbol: BTCUSDT     // 这里是一个交易对
    quote_asset: USDT   // 这里是交易对的锚定币
    base_asset: BTC     // 这里是交易对的另一个币
    quote_borrow: 0     // 可以不改
    percent: 0.5        // 达到平衡后，base_asset价值占比。这个例子中是BTC的占比
    trigger: 0.02       // 平衡阈值，0.02表示当两个币种的价值变化超过2%是触发交易，重新回到平衡状态。
```
只需在交易所中持有相应的币种，然后修改好配置文件，在双击运行run.bat即可。比如上面的例子，用户最开始有100 USDT。运行上面的程序后，程序自动买入50 USDT的BTC。 当币价涨超过2%或者跌超过2%后，会自动卖出或买入一定数量的BTC，重新达到两个币种的价值一样。如果你的USDT不多，又想买更多的BTC，可以设置quote_borrow为一个正整数。比如你的币安账号只有100 USDT，quote_borrow设置为40，这时候程序认为你有140 USDT，达到平衡后你的账号有70 USDT的BTC和30 UDST，相当于放大了杠杆，当币价跌得太猛，USDT不够时，再充值就可以了。


### 设置多个交易对
你可以设置多个交易对，当要保证任意两个交易对的币种不能有重复。比如下面这个配置是可行的。

```
markets:
  - symbol: BTCUSDT     
    quote_asset: USDT   
    base_asset: BTC     
    quote_borrow: 0    
    percent: 0.5       
    trigger: 0.02    
    
  - symbol: ETHBUSD
    quote_asset: BUSD
    base_asset: ETH
    quote_borrow: 0
    percent: 0.5
    trigger: 0.04
```

但是下面这个就是错误的，英文USDT出现了2次。如果你需要跑很多交易对，建议申请多个币安账号。
```
markets:
  - symbol: BTCUSDT     
    quote_asset: USDT   
    base_asset: BTC     
    quote_borrow: 0    
    percent: 0.5       
    trigger: 0.02    
    
  - symbol: ETHUSDT
    quote_asset: USDT
    base_asset: ETH
    quote_borrow: 0
    percent: 0.5
    trigger: 0.04
```

你还可以设置更多交易对, 比如
```
markets:
  - symbol: BTCUSDT     
    quote_asset: USDT   
    base_asset: BTC     
    quote_borrow: 0    
    percent: 0.5       
    trigger: 0.02    
    
  - symbol: ETHBUSD
    quote_asset: BUSD
    base_asset: ETH
    quote_borrow: 0
    percent: 0.2
    trigger: 0.04
    
  - symbol: DOTBNB
    quote_asset: BNB
    base_asset: DOT
    quote_borrow: 0
    percent: 0.8
    trigger: 0.06
    
```

## 可能遇到的问题
- 网络不通。可以在系统host添加域名解析。（修改host文件请自行百度）
```
52.84.150.39 api.binance.com
```
- 配置文件解错误。注意yaml文件格式是否正确。

- 交易失败。 币安限制每笔交易最低10U， 所以每个交易对最好至少配置1000U， 平衡阈值大于0.04（或者每个交易对2000U,平衡阈值大于0.02）。

## 联系方式

电报群： https://t.me/Luckycoin8
