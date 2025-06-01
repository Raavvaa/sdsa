//@version=5
// WRITE ME ON TELEGRAMM >> t.me/aaathename
var string VERSION                  =  'v1'

var string strategyName = '+DYADYA VOVA Ai🤖 Øptimized V2' + '\n' + 'Telegram @aaathename'
var string shortTitle = 'DYADYA VOVA'
strategy(title= strategyName, shorttitle= shortTitle , overlay=true, max_lines_count = 500, max_labels_count = 500, max_bars_back = 3000, initial_capital = 1000)


Watermark = table.new(position.bottom_right, 1, 4, border_width=5)

table.cell(Watermark, 0, 0, text= strategyName, text_color=color.rgb(252, 0, 189), text_size=size.normal)


label_tp = input.string('Disable', options=['Enable', 'Disable'], title='Show label Tps, Entry, SL') 
panel = input.string(title = "Dashboard", defval = "Dashboard 1", options=["Dashboard 1", "Dashboard 2", "Off"])


// GLOBAL VARIABLES

// ------------------------------------------
asset = input.string(title = "Strategies", defval = " ### Dev Mode (FREE SETTING sensitivity AND offset) ### ", options=[" ### Dev Mode (FREE SETTING sensitivity AND offset) ### ",
     "     ",
     "BTC/D - 15m | Mid-Term", 
     "ETH/USDT - 15m | Short-Term",
     "BTC/USDT - 30m | Mid-Term",
     "BTC/USDT - 1h | Long-Term", 
     "BTC/USDT - 15m | Short-Term",
     "QTUM/USDT - 15m | Short-Term",
     "ZIL/USDT - 30m | Mid-Term",
     "ZIL/USDT - 15m | Short-Term",
     "AUDIO/USDT - 15m | Short-Term",
     "AXS/USDT - 15m | Short-Term",
     "BAND/USDT - 30m | Mid-Term",
     "BAND/USDT - 15m | Short-Term",
     "TRX/USDT - 30m | Mid-Term" ,
     "OP/USDT - 15m | Short-Term",
     "OP/USDT - 30m | Mid-Term",
     "APE/USDT - 15m | Short-Term",
     "APE/USDT - 30m | Mid-Term",
     "ENS/USDT - 15m | Short-Term",
     "BAKE/USDT - 30m | Mid-Term", 
     "CELR/USDT - 30m | Mid-Term",
     "1000PEPE/USDT - 15m | Short-Term",
     "1000SHIB/USDT - 45m | Mid-Term", 
     "CHR/USDT - 15m | Short-Term",
     "CHR/USDT - 45m | Mid-Term",
     "HNT/USDT - 15m | Short-Term",
     "BLZ/USDT - 15m | Short-Term",
     "EGLD/USDT - 15m | Short-Term",
     "KAVA/USDT - 15m | Short-Term", 
     "ZEN/USDT - 15m | Short-Term",
     "FLOW/USDT - 30m | Mid-Term",
     "YFI/USDT - 15m | Short-Term",
     "LDO/USDT - 15m | Short-Term",
     "MATIC/USDT - 15m | Short-Term",
     "ANKR/USDT - 15m | Mid-Term",
     "TOMO/USDT - 15m | Short-Term",
     "DOT/USDT - 15m | Short-Term",
     "DASH/USDT - 30m | Mid-Term",
     "JASMY/USDT - 15m | Short-Term",
     "WOO/USDT - 15m | Short-Term",
     "SUSHI/USDT - 30m | Mid-Term",
     "SUSHI/USDT - 15m | Short-Term",
     "NEAR/USDT - 30m | Mid-Term",
     "SKL/USDT - 15m | Short-Term",
     "UNFI/USDT - 15m | Short-Term",
     "FTM/USDT - 15m | Short-Term",
     "SFP/USDT - 15m | Short-Term",
     'BNB/USDT - 30m | Mid-Term',
     'AUD/EUR - 15m | Short-Term',
     'CAD/JPY - 15m | Short-Term',
     'TSLA/TESLA - 15m | Short-Term',
     'AAPL/APLE - 15m | Short-Term',
     'AAPL/APLE - 5m | Short-Term',
     'TSLA/TESLA - 5m | Short-Term',
     'GOOG/GOOGLE - 15m | Short-Term',
     'SILVER/XAGUSD - 15m | Short-Term',
     'PLATINUM - 15m | Short-Term',
     'XAU/USD - 15m | Mid-Term',
     'AUD/EUR - 30m | Mid-Term',
     'XAU/USD - 30m | Mid-Term',
     'CHF/JPY - 15m | Short-Term',
     'USD/JPY - 15m | Short-Term',
     'USD/CAD - 15m | Short-Term',
     'GBP/USD - 15m | Short-Term',
     'EUR/USD - 15m | Short-Term',
     'AUD/USD - 15m | Short-Term',
     'GBP/JPY - 15m | Short-Term'
     ])
 



//Supertrend Indicator
amplitude = input(title='Amplitude', defval=45,                         group = "Trendline")
channelDeviation = input(title='Channel Deviation', defval=2,           group = "Trendline")
showArrows = input(title='Show Arrows', defval=true,                    group = "Trendline")
showChannels = input(title='Show Channels', defval=false,               group = "Trendline")
tpLevel = input.int(title='Take Profit Level', defval=1, minval=1, maxval=4,    group = "Trendline")
slLevel = input.int(title='Stop Loss Level', defval=1, minval=1, maxval=4,      group = "Trendline")

var int trend2 = 0
var int nextTrend = 0
var float maxLowPrice = nz(low[1], low)
var float minHighPrice = nz(high[1], high)

var float up2 = 0.0
var float down = 0.0
float atrHigh = 0.0
float atrLow = 0.0
float arrowUp = na
float arrowDown = na

atr3 = ta.atr(100) / 2
dev = channelDeviation * atr3

highPrice = high[math.abs(ta.highestbars(amplitude))]
lowPrice = low[math.abs(ta.lowestbars(amplitude))]
highma = ta.sma(high, amplitude)
lowma = ta.sma(low, amplitude)

if nextTrend == 1
    maxLowPrice := math.max(lowPrice, maxLowPrice)
    if highma < maxLowPrice and close < nz(low[1], low)
        trend2 := 1
        nextTrend := 0
        minHighPrice := highPrice
        minHighPrice
else
    minHighPrice := math.min(highPrice, minHighPrice)
    if lowma > minHighPrice and close > nz(high[1], high)
        trend2 := 0
        nextTrend := 1
        maxLowPrice := lowPrice
        maxLowPrice

if trend2 == 0
    if not na(trend2[1]) and trend2[1] != 0
        up2 := na(down[1]) ? down : down[1]
        arrowUp := up2 - atr3
        arrowUp
    else
        up2 := na(up2[1]) ? maxLowPrice : math.max(maxLowPrice, up2[1])
        up2
    atrHigh := up2 + dev
    atrLow := up2 - dev
    atrLow
else
    if not na(trend2[1]) and trend2[1] != 1
        down := na(up2[1]) ? up2 : up2[1]
        arrowDown := down + atr3
        arrowDown
    else
        down := na(down[1]) ? minHighPrice : math.min(minHighPrice, down[1])
        down
    atrHigh := down + dev
    atrLow := down - dev
    atrLow

ht = trend2 == 0 ? up2 : down

var color long_final1 = color.green
var color short_final1 = color.red

htColor = trend2 == 0 ? long_final1 : short_final1

atrHighPlot = plot(showChannels ? atrHigh : na, title='ATR High',   style=plot.style_circles, color=color.new(short_final1, 0))
atrLowPlot  = plot(showChannels ? atrLow : na,   title='ATR Low',    style=plot.style_circles, color=color.new(long_final1, 0))


buySignal   = not na(arrowUp) and trend2 == 0 and trend2[1] == 1
sellSignal  = not na(arrowDown) and trend2 == 1 and trend2[1] == 0

//.plotshape(showArrows and buySignal ? atrLow : na, title='Trend UP', style=shape.triangleup, location=location.belowbar, size=size.tiny, color=color.new(buyColor, 0))
//plotshape(showArrows and sellSignal ? atrHigh : na, title='Trend Down', style=shape.triangledown, location=location.abovebar, size=size.tiny, color=color.new(sellColor, 0))

















notes = input('',title='Your_notes ',inline='set00')
//tf = input(200, title='DC Period')
//mult15 = input.float(1.0, minval=0.001, maxval=10, step=0.2, title='DC Mult')

//Truncate Function
truncate(number, decimals) =>
    factor = math.pow(10, decimals)
    int(number * factor) / factor

//=======================================================================================
// Trade Time
daysback = input(30, title='Backtest Days, For Defining Best TF, Max: 60 Days')
millisecondinxdays = 1000 * 60 * 60 * 24 * daysback
leftbar = timenow - time < millisecondinxdays
backtest = leftbar
dpa = 8
//=======================================================================================
et1 = 'Supertrend'
longside = input.bool(true, title='Long Side', group='Strategy Option')
shortside = input.bool(true, title='Short Side', group='Strategy Option')
filter1 = 'Filter with Atr'
filter2 = 'Filter with RSI'
filter3 = 'Atr or RSI'
filter4 = 'Atr and RSI'
filter5 = 'No Filtering'
filter6 = 'Entry Only in flat market(By ATR or RSI)'
filter7 = 'Entry Only in flat market(By ATR and RSI)'
typefilter = input.string(filter5, title='Flat Filtering Input', options=[filter1, filter2, filter3, filter4, filter5, filter6, filter7], group='Strategy Options')
withsl = input.bool(false, title='Use Dynamic SL?', group='Strategy Options')

RSI = truncate(ta.rsi(close, input.int(7, group='set signal control #select dev mode#')), 2)
BBperiod   = input.int(100,minval=1,title='Sensitivity',inline='set1',             group='RSI Filterring')

BBdeviations = input.float(1.5,step=0.01,minval=0.01,title='Offset',inline='set1', group='RSI Filterring')

if asset            == 'BTC/D - 15m | Mid-Term'
    BBperiod        := 200
    BBdeviations    := 0.3

if asset            == 'ETH/USDT - 15m | Short-Term'
    BBperiod        := 150
    BBdeviations    := 0.5

if asset            == 'BTC/USDT - 30m | Mid-Term'
    BBperiod        := 160
    BBdeviations    := 1.5
    
if asset            == 'BTC/USDT - 1h | Long-Term'
    BBperiod        := 180
    BBdeviations    := 1
    
if asset            == 'BTC/USDT - 15m | Short-Term'
    BBperiod        := 150
    BBdeviations    := 3.1
    
if asset            == 'QTUM/USDT - 30m |Short-Term'
    BBperiod        := 196
    BBdeviations    := 1
    
if asset            == 'ZIL/USDT - 30m | Mid-Term'
    BBperiod        := 140
    BBdeviations    := 1.5

if asset            == 'ZIL/USDT - 15m | Short-Term'
    BBperiod        := 150
    BBdeviations    := 3.5
    
if asset            == 'AUDIO/USDT - 15m | Short-Term'
    BBperiod        := 320
    BBdeviations    := 2
    
if asset            == 'AXS/USDT - 15m | Short-Term'
    BBperiod        := 300
    BBdeviations    := 3
    
if asset            == 'BAND/USDT - 30m | Mid-Term'
    BBperiod        := 300
    BBdeviations    := 2
    
if asset            == 'BAND/USDT - 15m | Short-Term'
    BBperiod        := 300
    BBdeviations    := 2    
    
if asset            == 'TRX/USDT - 30m | Mid-Term'
    BBperiod        := 200
    BBdeviations    := 2
    
if asset            == 'OP/USDT - 15m | Short-Term'
    BBperiod        := 170
    BBdeviations    := 2.8
    
if asset            == 'OP/USDT - 30m | Mid-Term'
    BBperiod        := 170
    BBdeviations    := 2.3    
    
if asset            == 'APE/USDT - 15m | Short-Term'
    BBperiod        := 120
    BBdeviations    := 1.8
    
if asset            == 'APE/USDT - 30m | Mid-Term'
    BBperiod        := 170
    BBdeviations    := 2.3 
    
if asset            == 'ENS/USDT - 15m | Short-Term'
    BBperiod        := 170
    BBdeviations    := 3.3
    
if asset            == 'BAKE/USDT - 30m | Mid-Term'
    BBperiod        := 100
    BBdeviations    := 2.6
    
if asset            == 'CELR/USDT - 30m | Mid-Term'
    BBperiod        := 200
    BBdeviations    := 0.8
    
if asset            == '1000PEPE/USDT - 15m | Short-Term'
    BBperiod        := 60
    BBdeviations    := 2
    
if asset            == '1000SHIB/USDT - 45m | Mid-Term'
    BBperiod        := 50
    BBdeviations    := 3
    
if asset            == 'CHR/USDT - 15m | Short-Term'
    BBperiod        := 50
    BBdeviations    := 3
    
if asset            == 'CHR/USDT - 45m | Mid-Term'
    BBperiod        := 50
    BBdeviations    := 3    
    
if asset            == 'HNT/USDT - 15m | Short-Term'
    BBperiod        := 300
    BBdeviations    := 2
    
if asset            == 'BLZ/USDT - 15m | Short-Term'
    BBperiod        := 190
    BBdeviations    := 1
    
if asset            == 'EGLD/USDT - 15m | Short-Term'
    BBperiod        := 200
    BBdeviations    := 3
    
if asset            == 'KAVA/USDT - 15m | Short-Term'
    BBperiod        := 200
    BBdeviations    := 2
    
if asset            == 'ZEN/USDT - 15m | Short-Term'
    BBperiod        := 200
    BBdeviations    := 2
    
if asset            == 'FLOW/USDT - 30m | Mid-Term'
    BBperiod        := 20
    BBdeviations    := 3
    
if asset            == 'YFI/USDT - 15m | Short-Term'
    BBperiod        := 40
    BBdeviations    := 3
    
if asset            == 'LDO/USDT - 15m | Short-Term'
    BBperiod        := 200
    BBdeviations    := 2
    
if asset            == 'MATIC/USDT - 15m | Short-Term'
    BBperiod        := 200
    BBdeviations    := 2.5
    
if asset            == 'ANKR/USDT - 15m | Short-Term'
    BBperiod        := 20
    BBdeviations    := 3
    
if asset            == 'TOMO/USDT - 15m | Short-Term'
    BBperiod        := 20
    BBdeviations    := 3
    
if asset            == 'DOT/USDT - 15m | Short-Term'
    BBperiod        := 20
    BBdeviations    := 2.5
        
if asset            == 'DASH/USDT - 30m | Mid-Term'
    BBperiod        := 200
    BBdeviations    := 1
        
if asset            == 'JASMY/USDT - 15m | Short-Term'
    BBperiod        := 60
    BBdeviations    := 2
        
if asset            == 'WOO/USDT - 15m | Short-Term'
    BBperiod        := 40
    BBdeviations    := 3
        
if asset            == 'SUSHI/USDT - 30m | Mid-Term'
    BBperiod        := 40
    BBdeviations    := 2

if asset            == 'SUSHI/USDT - 15m | Short-Term'
    BBperiod        := 40
    BBdeviations    := 2    
        
if asset            == 'NEAR/USDT - 30m | Mid-Term'
    BBperiod        := 60
    BBdeviations    := 2
        
if asset            == 'SKL/USDT - 15m | Short-Term'
    BBperiod        := 200
    BBdeviations    := 2
        
if asset            == 'UNFI/USDT - 15m | Short-Term'
    BBperiod        := 180
    BBdeviations    := 2
        
if asset            == 'FTM/USDT - 15m | Short-Term'
    BBperiod        := 80
    BBdeviations    := 2.2
        
if asset            == 'SFP/USDT - 15m | Short-Term'
    BBperiod        := 150
    BBdeviations    := 2

if asset            == 'BNB/USDT - 30m | Mid-Term'
    BBperiod        := 90
    BBdeviations    := 2.4

if asset            == 'AUD/EUR - 15m | Short-Term'
    BBperiod        := 90
    BBdeviations    := 2.4

if asset            == 'CAD/JPY - 15m | Short-Term'
    BBperiod        := 110
    BBdeviations    := 2

if asset            == 'TSLA/TESLA - 15m | Short-Term'
    BBperiod        := 60
    BBdeviations    := 0.8

if asset            == 'AAPL/APLE - 15m | Short-Term'
    BBperiod        := 70
    BBdeviations    := 0.3

if asset            == 'AAPL/APLE - 5m | Short-Term'
    BBperiod        := 210
    BBdeviations    := 1

if asset            == 'TSLA/TESLA - 5m | Short-Term'
    BBperiod        := 35
    BBdeviations    := 2.5

if asset            == 'GOOG/GOOGLE - 15m | Short-Term'
    BBperiod        := 90
    BBdeviations    := 1.5

if asset            == 'SILVER/XAGUSD - 15m | Short-Term'
    BBperiod        := 90
    BBdeviations    := 1.5

if asset            == 'PLATINUM - 15m | Short-Term'
    BBperiod        := 90
    BBdeviations    := 1.5


if asset            == 'XAU/USD - 15m | Mid-Term'
    BBperiod        := 20
    BBdeviations    := 3

if asset            == 'AUD/EUR - 30m | Mid-Term'
    BBperiod        := 120
    BBdeviations    := 1.8
            
if asset            == 'XAU/USD - 30m | Mid-Term'
    BBperiod        := 250
    BBdeviations    := 2

if asset            == 'CHF/JPY - 15m | Short-Term'
    BBperiod        := 300
    BBdeviations    := 1.5
            
if asset            == 'USD/JPY - 15m | Short-Term'
    BBperiod        := 200
    BBdeviations    := 0.8
            
if asset            == 'USD/CAD - 15m | Short-Term'
    BBperiod        := 100
    BBdeviations    := 2.6
            
if asset            == 'GBP/USD - 15m | Short-Term'
    BBperiod        := 80
    BBdeviations    := 3
                
if asset            == 'EUR/USD - 15m | Short-Term'
    BBperiod        := 200
    BBdeviations    := 2
                
if asset            == 'AUD/USD - 15m | Short-Term'
    BBperiod        := 400
    BBdeviations    := 1.5
                
if asset            == 'GBP/JPY - 15m | Short-Term'
    BBperiod        := 100
    BBdeviations    := 2



toplimitrsi = input.int(45, title='TOP Limit', group='RSI Filterring')
botlimitrsi = input.int(10, title='BOT Limit', group='RSI Filterring')






// TP SL Option
ex1 = 'TP & SL by Percentage Price'
ex2 = 'TP & SL by atr Value'
et = input.string(ex2, title='TP SL Type', options=[ex1, ex2], group='Strategy Options')
ST = false
period = 15
mult = 5
atrLen = input.int(5, minval=1, title='atr Length', group='Sideways Filtering Input')
atrMaType = input.string('EMA', options=['SMA', 'EMA'], group='Sideways Filtering Input', title='atr Moving Average Type')
atrMaLen = input.int(5, minval=1, title='atr MA Length', group='Sideways Filtering Input')
adxLen = input.float(5, minval = 1, maxval = 50, title = "ADX SMOOTHing", group='Sideways Filtering Input')
diLen = input.float(14, minval = 1, title = "DI Length", group='Sideways Filtering Input')
adxLim = input.float(22, minval = 1, title = "ADX Limit", group='Sideways Filtering Input')
SMOOTH = input.float(3, minval = 1, maxval = 5, title = "SMOOTHing Factor", group='Sideways Filtering Input')
lag = input.float(8, minval = 0, maxval = 15, title = "Lag", group='Sideways Filtering Input')



//Supertrend Indicator
src = hl2
atr2 = ta.sma(ta.tr, period)
atr = request.security(syminfo.ticker, '', ta.atr(period))
up = src - mult * atr
up1 = nz(up[1], up)
up := close[1] > up1 ? math.max(up, up1) : up
dn = src + mult * atr
dn1 = nz(dn[1], dn)
dn := close[1] < dn1 ? math.min(dn, dn1) : dn
trend = 1
trend := nz(trend[1], trend)
trend := trend == -1 and close > dn1 ? 1 : trend == 1 and close < up1 ? -1 : trend
st = trend == 1 ? up : dn

//Buy-Sell Indicator

BBUpper = ta.sma(close,BBperiod)+ta.stdev(close,BBperiod)*BBdeviations
BBLower = ta.sma(close,BBperiod)-ta.stdev(close,BBperiod)*BBdeviations

oc2 = (close+open)/2
UP_trend = close>BBUpper and oc2>BBUpper
DN_trend = close<BBLower and oc2<BBLower
FLAT = close<BBUpper and close>BBLower

//TrendLine = ta.sma(ohlc4,BBperiod)

UseATRfilter = false

ATR1period = 5

hl = false

TrendLine = 0.0


iTrend = 0.0

long_final = 0.0

short_final = 0.0

BBSignal = close > BBUpper ? 1 : close < BBLower ? -1 : 0


if BBSignal == 1 and UseATRfilter == 1

    TrendLine := low - ta.atr(ATR1period)



    if TrendLine < TrendLine[1]



        TrendLine := TrendLine[1]
        TrendLine

if BBSignal == -1 and UseATRfilter == 1

    TrendLine := high + ta.atr(ATR1period)



    if TrendLine > TrendLine[1]



        TrendLine := TrendLine[1]
        TrendLine

if BBSignal == 0 and UseATRfilter == 1



    TrendLine := TrendLine[1]
    TrendLine

if BBSignal == 1 and UseATRfilter == 0



    TrendLine := low



    if TrendLine < TrendLine[1]



        TrendLine := TrendLine[1]
        TrendLine

if BBSignal == -1 and UseATRfilter == 0

    TrendLine := high


    if TrendLine > TrendLine[1]

        TrendLine := TrendLine[1]
        TrendLine

if BBSignal == 0 and UseATRfilter == 0

    TrendLine := TrendLine[1]
    TrendLine

iTrend := iTrend[1]


if TrendLine > TrendLine[1]


    iTrend := 1
    iTrend

if TrendLine < TrendLine[1]


    iTrend := -1
    iTrend


long_final := iTrend[1] == -1 and iTrend == 1 ? 1 : na


short_final := iTrend[1] == 1 and iTrend == -1 ? 1 : na


i_entryPriceSrc = input.source(title='Entry Price Calculation Src', defval=ohlc4, group='?? Risk Reward Ratio ??')

//filtering
atra = request.security(syminfo.tickerid, '', ta.atr(atrLen))
atrMa = atrMaType == 'EM' ? ta.ema(atra, atrMaLen) : ta.sma(atra, atrMaLen)
updm = ta.change(high)
downdm = -ta.change(low)
plusdm = na(updm) ? na : updm > downdm and updm > 0 ? updm : 0
minusdm = na(downdm) ? na : downdm > updm and downdm > 0 ? downdm : 0
//trur = rma(tr, diLen)                                    

cndSidwayss1 = atra >= atrMa
cndSidwayss2 = RSI > toplimitrsi or RSI < botlimitrsi
cndSidways = cndSidwayss1 or cndSidwayss2
cndSidways1 = cndSidwayss1 and cndSidwayss2
Sidwayss1 = atra <= atrMa
Sidwayss2 = RSI < toplimitrsi and RSI > botlimitrsi
Sidways = Sidwayss1 or Sidwayss2
Sidways1 = Sidwayss1 and Sidwayss2

trendType = typefilter == filter1 ? cndSidwayss1 : typefilter == filter2 ? cndSidwayss2 : typefilter == filter3 ? cndSidways : typefilter == filter4 ? cndSidways1 : typefilter == filter5 ? RSI > 0 : typefilter == filter6 ? Sidways : typefilter == filter7 ? Sidways1 : na


buy = long_final and longside and backtest and trendType
sell = short_final and shortside and backtest and trendType










// ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░

var bool is_long_trend_started = false

var bool is_short_trend_started = false

var bool is_trend_change = na

var bool is_long_trend = false

var bool is_short_trend = false



var bool is_long_trend_start = false

var bool is_short_trend_start = false




var bool redy_long = false

var bool can_long = false

var bool redy_short = false

var bool can_short = false

var bool is_long_go = false

var bool is_short_go = false



var float imba_entry_long_line = na

var float imba_entry_short_line = na



var float STLLong = na 

var float STLShort = na



// Inputs

// ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░



// ————  Fix Settings

asset2 = input.string(title = "Strategies", defval = "Dev Mode (FREE SETTING)", options=["Dev Mode (FREE SETTING)",

     "UNIVERSAL(1,3,5,10,15,30) | UNV"

     ])



// ——— MAIN

sensitivity = input.float(200, title = 'Sensitive', step = 1,group = "Main", inline = "1")



start_date_input = input.time(defval = timestamp("1 Jan 2024"), title = "Start calculating date",group = "Main", inline = "2")



// ——— Filter

filterAct = input.bool(true,title = "Atr and RSI",group = 'Sideways Filtering Input', inline = "1")


// ——— Backraound Zone

showZones = input(false, title='Show Bullish/Bearish Zones')



// ——— Fib LvL

_236 = input.float(0.236, title = 'Final Long', step = 0.001, group='FIB LVL')

_382 = input.float(0.382, title = 'Sensitive', step = 0.001, group='FIB LVL')

_5 = input.float(0.5, title = 'trend line', step = 0.001, group='FIB LVL')

_456 = input.float(-0.456, title = 'SL line', step = 0.001, group='FIB LVL')

_618 = input.float(0.618, title = 'Sensitive', step = 0.001, group='FIB LVL')

_786 = input.float(0.786, title = 'finel short', step = 0.001, group='FIB LVL')



// ———— Strategie TP SL

useSession  = input.bool        (defval = false,        title = 'Sessione ', inline = 'Sessione', group = " ⚽INPUT settings  ")

session     = input.session     (defval = '0000-0000',  title = '',         inline = 'Sessione', group = "   INPUT settings  ") + ':'

   + (input.bool                (defval = true,         title = 'Lu',      inline = 'Days',    group = "   INPUT settings  ") ? str.tostring(dayofweek.monday) : '')

   + (input.bool                (defval = true,         title = 'Ma',      inline = 'Days',    group = "   INPUT settings  ") ? str.tostring(dayofweek.tuesday) : '')

   + (input.bool                (defval = true,         title = 'Me',      inline = 'Days',    group = "   INPUT settings  ") ? str.tostring(dayofweek.wednesday) : '')

   + (input.bool                (defval = true,         title = 'Gi',      inline = 'Days',    group = "   INPUT settings  ") ? str.tostring(dayofweek.thursday) : '')

   + (input.bool                (defval = true,         title = 'Ve',      inline = 'Days',    group = "   INPUT settings  ") ? str.tostring(dayofweek.friday) : '')

   + (input.bool                (defval = true,        title = 'Sa',      inline = 'Days',    group = "   INPUT settings  ") ? str.tostring(dayofweek.saturday) : '')

   + (input.bool                (defval = true,        title = 'Do',      inline = 'Days',    group = "   INPUT settings  ") ? str.tostring(dayofweek.sunday) : '')

closeAtSessionEnd = input.bool  (defval = false,        title = 'Close all sessions at the end',     group = "   INPUT settings  ", tooltip = 'Close all positions at the market price at the end of each session')



multiprofit     = input.bool(true, title="Use Multi Profit" , group = " 💰 OUTPUT settings 💰 ")

dcaAct          = input.bool(true, title="Use Dollar Cost Averaging :" , group = " 💰 OUTPUT settings 💰 ", inline='DCA1')

dacValue        = input.string("5", title="Number of entry", options=["2","3","5"], group = " 💰 OUTPUT settings 💰 ", inline='DCA1')



TP1     =   input.float(0.5,    title="TP1 %",  step=0.1, group=" 💰 OUTPUT settings 💰 ", inline='1')

TP2     =   input.float(1.1,    title="TP2 %",  step=0.1, group=" 💰 OUTPUT settings 💰 ", inline='2')

TP3     =   input.float(2.5,    title="TP3 %",  step=0.1, group=" 💰 OUTPUT settings 💰 ", inline='3')

TP4     =   input.float(4.2,      title="TP4 %",  step=0.1, group=" 💰 OUTPUT settings 💰 ", inline='4')



qty1    =   input.float(30,     title="% EXIT", step=1,   group=" 💰 OUTPUT settings 💰 ", inline='1')

qty2    =   input.float(30,     title="% EXIT", step=1,   group=" 💰 OUTPUT settings 💰 ", inline='2')

qty3    =   input.float(15,     title="% EXIT", step=1,   group=" 💰 OUTPUT settings 💰 ", inline='3')

qty4    =   input.float(15,     title="% EXIT", step=1,   group=" 💰 OUTPUT settings 💰 ", inline='4')



SL      =   input.float(1.5,      title="Stoploss %", step=0.1, group=" 💰 OUTPUT settings 💰 ", inline='6')



movestoploss    = input.bool(title="If TP1 is reached: Move Stoploss to entry price",   group=" 💰 OUTPUT settings 💰 ",  defval = false)

active_trailing = input.bool(title="If TP1 is reached: Activate Trailing Stoploss ",     group=" 💰 OUTPUT settings 💰 ",  defval = false)



sl_type         = input.string('ATR',   title='Tipo di Trailing', options=['ATR', '%', 'FIB LVL'],                    inline='Trailing1')

atrLength       = input(14,             title='Durata ATR',                                             inline='Trailing1')

stopLoss        = input.int(            title='% Trailing',                 defval=5, minval=1,         inline='Trailing2')

atrMultiplier   = input(5,              title='ATR Multi Trailing',                                      inline='Trailing2')





// Daschbord ans Alert Setting

var bool                    plotDashboard                   = input.bool(true, group="groupBacktest", title="Plot Dashboard",inline="UX Backtest")

var int                     DashLabel                       = input.int(defval=500, title='Label X Offset', minval=0, maxval=500, group="groupBacktest",inline="UX Backtest")

var bool                    tradeIdeeAct                    = input.bool(true, group="groupBacktest", title="Show Trade Idee",inline="UX Backtest")

var bool                    oppositeSignalACT               = input.bool(true, group="groupBacktest", title="Stop or opposite Signal",inline="UX Backtest")

var bool                    trailingConfigurationACT        = input.bool(true, "Trailing Configuration ON/OFF"  ,inline = "trailing", group = "groupBacktest")

var string                  trailingConfigurationType       = input.string("Breakeven","Trailing Type", options=["Breakeven","Moving Target","Moving 2-Target", "Percent Below Triggers", "Percent Below Highest"], inline = "trailing", group = "groupBacktest")

var int                     movingTarget                    = input.int(1, title="Moving Target (BE)", minval=1,group="groupBacktest",inline="SignalG")

// Asset Settings

// ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░



if asset2            == "UNIVERSAL(1,3,5,10,15,30) | UNV"

    sensitivity     := 350

    filterAct       := true

    typefilter      := 'Atr or RSI'

    TP1             := 0.5

    TP2             := 1.1

    TP3             := 2.1

    TP4             := 4.5

    SL              := 1.5

    dcaAct          := true

    dacValue        := "2"

// Main


// STRATEGY

//sensitivity *= sensitivity2

//sensitivity *= 10



high_line = ta.highest(high, int(sensitivity))

low_line = ta.lowest(low, int(sensitivity))

channel_range = high_line - low_line

fib_236 = high_line - channel_range * (_236)

fib_382 = high_line - channel_range * _382

fib_5 = high_line - channel_range * _5

fib_456 = high_line - channel_range * _456

fib_618 = high_line - channel_range * _618

fib_786 = high_line - channel_range * (_786)

imba_trend_line = fib_5

SL_Line = fib_456




//filtering



RSI2 = truncate(ta.rsi(close, input.int(14, group='RSI Filterring')), 2)



atra2 = request.security(syminfo.tickerid, '', ta.atr(atrLen))

atrMa2 = atrMaType == 'EM' ? ta.ema(atra2, atrMaLen) : ta.sma(atra2, atrMaLen)

updm2 = ta.change(high)

downdm2 = -ta.change(low)

plusdm2 = na(updm2) ? na : updm2 > downdm2 and updm2 > 0 ? updm2 : 0

minusdm2 = na(downdm2) ? na : downdm2 > updm2 and downdm2 > 0 ? downdm2 : 0

//trur = rma(tr, diLen)                                    

//plus = fixnan(100 * rma(plusdm, diLen) / trur)    

//minus = fixnan(100 * rma(minusdm, diLen) / trur)

//sum = plus + minus

//adx = 100 * rma(abs(plus - minus) / (sum == 0 ? 1 : sum), adxLen)






if time >= start_date_input

    can_long := close >= imba_trend_line and close >= fib_236 and not is_long_trend and trendType

    can_short := close <= imba_trend_line and close <= fib_786 and not is_short_trend and trendType



// CAN LONG/SHORT

//
if can_long

    is_long_trend := true

    is_short_trend := false

    is_long_trend_started := is_long_trend_started ? false : true

else if can_short

    is_short_trend := true

    is_long_trend := false

    is_short_trend_started := is_short_trend_started ? false : true

else

    is_trend_change := false

    can_long := false

    can_short := false

    is_short_trend_started := false

    is_long_trend_started := false

plot(imba_trend_line, color = is_long_trend[1] ? #14caa3: is_short_trend ? color.new(#ff0000, 0) : color.new(#000000, 100), linewidth = 3)


//filtering



// Khai báo biến đếm
var int long_count = 0
var int short_count = 0
var int opened_trade = 0

// Strategy

if buy

    strategy.entry("Long", strategy.long)

    imba_entry_long_line := imba_trend_line
    long_count := long_count + 1
    opened_trade := opened_trade + 1

if sell

    strategy.entry("Short", strategy.short)

    imba_entry_short_line := imba_trend_line
    short_count := short_count + 1
    opened_trade := opened_trade + 1


// Add labels for TP1, TP2, TP3

labelTpSl(_use, _y, _text, _color) =>

    if (_use)

        label.new(bar_index, _y, text=_text, color=_color, textalign=text.align_left)



bool sessionFilter = useSession ? not (na(time(timeframe.period, session)) or na(time_close(timeframe.period, session))) : true





percentTPSL(x) =>

    strategy.position_size != 0 ? math.round(x / 100 * strategy.position_avg_price / syminfo.mintick) : float(na)



sl_val = sl_type == 'ATR' ? atrMultiplier * ta.atr(atrLength) : sl_type == 'FIB LVL' ? SL_Line : sl_type == 'PERCENTAGE' ? close * stopLoss / 100 : 0.00 



trailing_sl_long = 0.0

trailing_sl_long := strategy.position_size > 0 ? imba_trend_line  : na



trailing_sl_short = 0.0

trailing_sl_short := strategy.position_size < 0 and strategy.position_size[1] >= 0 ? high + sl_val :

         strategy.position_size < 0 and strategy.position_size[1] < 0 ? math.min(high + sl_val, nz(trailing_sl_short[1])) : na





// Logic SL TP ----------------------------------------

//

bool status_long_hit_tp1 = na

status_long_hit_tp1 := (strategy.position_size[1] > 0 and strategy.position_size > 0 and strategy.position_size[1] > strategy.position_size) ? true : strategy.position_size <= 0 ? false : status_long_hit_tp1[1]



bool status_short_hit_tp1 = na

status_short_hit_tp1 := (strategy.position_size[1] < 0 and strategy.position_size < 0 and strategy.position_size[1] < strategy.position_size) ? true : strategy.position_size >= 0 ? false : status_short_hit_tp1[1]



//

STLLong         := (status_long_hit_tp1 == true  and movestoploss == true) ? strategy.position_avg_price : imba_entry_long_line - percentTPSL(SL)*syminfo.mintick



STLShort        := (status_short_hit_tp1 == true and movestoploss == true) ? strategy.position_avg_price : imba_entry_short_line + percentTPSL(SL)*syminfo.mintick





//Calculate levels

long_sl_lv      = (status_long_hit_tp1 == true      and active_trailing == true) ? trailing_sl_long     : STLLong

short_sl_lv     = (status_short_hit_tp1 == true     and active_trailing == true) ? trailing_sl_short    : STLShort



long_tp1_lv     = strategy.position_avg_price + percentTPSL(TP1)*syminfo.mintick

long_tp2_lv     = strategy.position_avg_price + percentTPSL(TP2)*syminfo.mintick

long_tp3_lv     = strategy.position_avg_price + percentTPSL(TP3)*syminfo.mintick

long_tp4_lv     = strategy.position_avg_price + percentTPSL(TP4)*syminfo.mintick





short_tp1_lv    = strategy.position_avg_price - percentTPSL(TP1)*syminfo.mintick

short_tp2_lv    = strategy.position_avg_price - percentTPSL(TP2)*syminfo.mintick

short_tp3_lv    = strategy.position_avg_price - percentTPSL(TP3)*syminfo.mintick

short_tp4_lv    = strategy.position_avg_price - percentTPSL(TP4)*syminfo.mintick




////////////////////////////////////////////////////////////////////////////////SETUP EXIT

if multiprofit == true

    strategy.exit("TP1", "Long", qty_percent = qty1,    profit = percentTPSL(TP1),  comment = "TP1")

    strategy.exit("TP2", "Long", qty_percent = qty2,    profit = percentTPSL(TP2),  comment = "TP2")

    strategy.exit("TP3", "Long", qty_percent = qty3,    profit = percentTPSL(TP3),  comment = "TP3")

    strategy.exit("TP4", "Long", qty_percent = qty4,    profit = percentTPSL(TP4),  comment = "TP4")


    strategy.close_all(when = strategy.position_size > 0 and low < long_sl_lv,          comment = "Closed")

    strategy.exit("TP1", "Short", qty_percent = qty1,   profit = percentTPSL(TP1), comment = "TP1")

    strategy.exit("TP2", "Short", qty_percent = qty2,   profit = percentTPSL(TP2), comment = "TP2")

    strategy.exit("TP3", "Short", qty_percent = qty3,   profit = percentTPSL(TP3), comment = "TP3")

    strategy.exit("TP4", "Short", qty_percent = qty4,   profit = percentTPSL(TP4), comment = "TP4")



    strategy.close_all(when = strategy.position_size < 0 and high > short_sl_lv,            comment = "Closed")



//Close all positions at the end of the session

strategy.close_all(when = closeAtSessionEnd and not sessionFilter, comment = 'Fine Sessione')





//SETUP exit sipmle

if multiprofit == false

    strategy.exit("SL",     "Long",     loss = STLLong,     comment     = "SL")

    strategy.exit("SL",     "Short",    loss = STLShort,    comment     = "SL")



//////////////////////////////////////////////////////////////////////////////// Plot TP & SL

var float entryLongTwo = 0.00

var float entryLongThree = 0.00

var float entryLongFour = 0.00



var float entryShortTwo = 0.00

var float entryShortThree = 0.00

var float entryShortFour = 0.00



entryLongThree := math.round_to_mintick(math.avg(strategy.position_avg_price,imba_entry_long_line))

entryLongTwo := math.round_to_mintick(math.avg(strategy.position_avg_price,entryLongThree))

entryLongFour := math.round_to_mintick(math.avg(entryLongThree,imba_entry_long_line))



entryShortThree := math.round_to_mintick(math.avg(strategy.position_avg_price,imba_entry_short_line))

entryShortTwo := math.round_to_mintick(math.avg(strategy.position_avg_price,entryShortThree))

entryShortFour := math.round_to_mintick(math.avg(entryShortThree,imba_entry_short_line))





plot(strategy.position_size > 0 ? long_sl_lv : na,      color=color.new(color.blue, 20), style=plot.style_linebr, title="SL Long", linewidth =2)

plot(strategy.position_size < 0 ? short_sl_lv : na,     color=color.new(color.blue, 20), style=plot.style_linebr, title="SL Short", linewidth =2)






plot(multiprofit == true and strategy.position_size > 0 ? strategy.position_avg_price + percentTPSL(TP1)*syminfo.mintick : na,     color=color.new(#14ca3b, 30), style=plot.style_linebr, title="Long TP1", linewidth =2)

plot(multiprofit == true and strategy.position_size > 0 ? strategy.position_avg_price + percentTPSL(TP2)*syminfo.mintick : na,     color=color.new(#14ca3b, 30), style=plot.style_linebr, title="Long TP2", linewidth =2)

plot(multiprofit == true and strategy.position_size > 0 ? strategy.position_avg_price + percentTPSL(TP3)*syminfo.mintick : na,     color=color.new(#14ca3b, 30), style=plot.style_linebr, title="Long TP3", linewidth =2)

plot(multiprofit == true and strategy.position_size > 0 ? strategy.position_avg_price + percentTPSL(TP4)*syminfo.mintick : na,     color=color.new(#14ca3b, 30), style=plot.style_linebr, title="Long TP4", linewidth =2)

plot(multiprofit == true and strategy.position_size > 0 ? strategy.position_avg_price + percentTPSL(TP4)*syminfo.mintick : na,     color=color.new(#14ca3b, 30), style=plot.style_linebr, title="Long TP5", linewidth =2)



plot(multiprofit == true and strategy.position_size < 0 ? strategy.position_avg_price - percentTPSL(TP1)*syminfo.mintick : na,    color=color.new(color.red, 30), style=plot.style_linebr, title="Short TP1", linewidth =2)

plot(multiprofit == true and strategy.position_size < 0 ? strategy.position_avg_price - percentTPSL(TP2)*syminfo.mintick : na,    color=color.new(color.red, 30), style=plot.style_linebr, title="Short TP2", linewidth =2)

plot(multiprofit == true and strategy.position_size < 0 ? strategy.position_avg_price - percentTPSL(TP3)*syminfo.mintick : na,    color=color.new(color.red, 30), style=plot.style_linebr, title="Short TP3", linewidth =2)

plot(multiprofit == true and strategy.position_size < 0 ? strategy.position_avg_price - percentTPSL(TP4)*syminfo.mintick : na,    color=color.new(color.red, 30), style=plot.style_linebr, title="Short TP4", linewidth =2)

plot(multiprofit == true and strategy.position_size < 0 ? strategy.position_avg_price - percentTPSL(TP4)*syminfo.mintick : na,    color=color.new(color.red, 30), style=plot.style_linebr, title="Short TP5", linewidth =2)



//Label TP/SL

_x = timenow + math.round(ta.change(time) * 2)



draw_label(y1, y2, label1, label2, percent1, percent2, _textcolor) =>

    var label Label = na

    label.delete(Label)

    Label := label.new(_x, strategy.position_size > 0 ? y1 : y2, strategy.position_size > 0 ? '' + str.tostring(label1) + ': ' + str.tostring(math.round(y1,4)) + ' (' + str.tostring(math.round(percent1,2)) + '%)' : '' + str.tostring(label2) + ': ' + str.tostring(math.round(y2,4)) + ' (' + str.tostring(math.round(percent2,2)) + '%)'  , color=color.new(color.white, 100), textcolor=_textcolor, style=label.style_label_left, yloc=yloc.price, xloc=xloc.bar_time, size=size.small, textalign=text.align_left)

    Label

draw_label_entry(y1, y2, label1, label2, entry1, entry2, _textcolor) =>

    var label Label = na

    label.delete(Label)

    Label := label.new(_x, strategy.position_size > 0 ? y1 : y2, strategy.position_size > 0 ? '' + str.tostring(label1) + ': ' + str.tostring(math.round(y1,4)) + ' (' + str.tostring(entry1) + ')' : '' + str.tostring(label2) + ': ' + str.tostring(math.round(y2,4)) + ' (' + str.tostring(entry2) + ')' , color=color.new(color.white, 100), textcolor=_textcolor, style=label.style_label_left, yloc=yloc.price, xloc=xloc.bar_time, size=size.small, textalign=text.align_left)

    Label





if multiprofit == true and label_tp =='Enable'

    if dcaAct == true

        // 2 Rebuy

        if dacValue == "2"

            draw_label_entry(strategy.position_avg_price, strategy.position_avg_price,   " Entry Zone Long ",   "  Entry Zone Short", "Entry 1", "Entry 1",    color.new(#004cff, 30))

            draw_label_entry(imba_entry_long_line, imba_entry_short_line,   "  Entry Zone Long ",   "  Entry Zone Short", "Entry 2", "Entry 2",   color.new(#004cff, 30))

        // 3 Rebuy

        if dacValue == "3"

            draw_label_entry(strategy.position_avg_price, strategy.position_avg_price,   "  Entry Zone Long ",   "  Entry Zone Short", "Entry 1", "Entry 1",    color.new(#004cff, 30))

            draw_label_entry(entryLongThree, entryShortThree,   "",   "", "Entry 2", "Entry 2",   color.new(#004cff, 30))

            draw_label_entry(imba_entry_long_line, imba_entry_short_line,   "  Entry Zone Long ",   "  Entry Zone Short", "Entry 3", "Entry 3",   color.new(#004cff, 30))                                                                                                                           

        // 5 Rebuy

        if dacValue == "5"

            draw_label_entry(strategy.position_avg_price, strategy.position_avg_price,   "  Entry Zone Long ",   "  Entry Zone Short", "Entry 1", "Entry 1",    color.new(#004cff, 30))

            draw_label_entry(entryLongTwo, entryShortTwo,   "",   "", "Entry 2", "Entry 2",   color.new(#004cff, 30))

            draw_label_entry(entryLongThree, entryShortThree,   "",   "", "Entry 3", "Entry 3",   color.new(#004cff, 30))

            draw_label_entry(entryLongFour, entryShortFour,   "",   "", "Entry 4", "Entry 4",   color.new(#004cff, 30))

            draw_label_entry(imba_entry_long_line, imba_entry_short_line,   "  Entry Zone Long ",   "  Entry Zone Short", "Entry 5", "Entry 5",   color.new(#004cff, 30))

    else

        draw_label_entry(strategy.position_avg_price, strategy.position_avg_price,   "Entry Long ",   "Entry Short", "Entry 1", "Entry 1",    color.new(#004cff, 30))



    draw_label(strategy.position_avg_price + percentTPSL(TP1)*syminfo.mintick,      strategy.position_avg_price - percentTPSL(TP1)*syminfo.mintick,   "✔️ Long TP1",   "✔️ Short TP1", TP1, TP1, color.new(#14ca3b, 30))

    draw_label(strategy.position_avg_price + percentTPSL(TP2)*syminfo.mintick,      strategy.position_avg_price - percentTPSL(TP2)*syminfo.mintick,   "✔️ Long TP2",   "✔️ Short TP2", TP2, TP2, color.new(#14ca3b, 30))

    draw_label(strategy.position_avg_price + percentTPSL(TP3)*syminfo.mintick,      strategy.position_avg_price - percentTPSL(TP3)*syminfo.mintick,   "✔️ Long TP3",   "✔️ Short TP3", TP3, TP3, color.new(#14ca3b, 30))

    draw_label(strategy.position_avg_price + percentTPSL(TP4)*syminfo.mintick,      strategy.position_avg_price - percentTPSL(TP4)*syminfo.mintick,   "💎 Long TP4",   "💎 Short TP4", TP4, TP4, color.new(#14ca3b, 30))




    draw_label(long_sl_lv,short_sl_lv,"Long SL","Short SL",  (imba_entry_long_line-long_sl_lv)*100/strategy.position_avg_price, (-imba_entry_short_line+short_sl_lv)*100/strategy.position_avg_price, color.new(#ca14af, 20))

    

DashLabelxpos = DashLabel * (time - time[1])



closedTrades = strategy.closedtrades

winTrades = strategy.wintrades

lossTrades = strategy.losstrades



newWin  = (strategy.wintrades  > strategy.wintrades[1]) and (strategy.losstrades == strategy.losstrades[1]) and (strategy.eventrades == strategy.eventrades[1])

newLoss = (strategy.wintrades == strategy.wintrades[1]) and (strategy.losstrades  > strategy.losstrades[1]) and (strategy.eventrades == strategy.eventrades[1])



varip int winRow     = 0

varip int lossRow    = 0

varip int maxWinRow  = 0

varip int maxLossRow = 0



if newWin

    lossRow := 0

    winRow := winRow + 1

    if winRow > maxWinRow

        maxWinRow := winRow

        

if newLoss

    winRow := 0

    lossRow := lossRow + 1

    if lossRow > maxLossRow

        maxLossRow := lossRow



var int MyDate = na

if strategy.opentrades == 1 and na(MyDate)

    MyDate := time

// Tính ATR
atr_length = 14
atr_value = ta.atr(atr_length)

// Chuyển ATR thành % so với giá đóng cửa
atr_percent = (atr_value / close) * 100

// timeframe
name_tf = timeframe.period == '1' ? '1m' : timeframe.period == '3' ? '3m' : timeframe.period == '5' ? '5m' : timeframe.period == '15' ? '15m' : timeframe.period == '30' ? '30m' : timeframe.period == '60' ? '1H' : timeframe.period == '120' ? '2H' : timeframe.period == '240' ? '4H' : 'D'


// Lấy thời gian hiện tại (UNIX timestamp)
current_time = time / 1000  // Chuyển từ milliseconds -> seconds

// Lấy giờ UTC
utc_hour = hour(time, "UTC")

// Lấy giờ theo múi giờ của biểu đồ
local_hour = hour(time)
///////////////////////////////////

showtime=true
var show_newyork_session =  true and showtime
var newyork_color = color.rgb(10, 141, 242, 20)
var newyork_text_color =color.rgb(10, 141, 242, 0)
var newyork_position = 'Top'

var show_london_session =  true and showtime
var london_color = color.rgb(80, 204, 27, 20)
var london_text_color = color.rgb(80, 204, 27, 20)
var london_position = 'Bottom'

var show_tokyo_session = true and showtime
var tokyo_color = color.rgb(252, 223, 3, 20)
var tokyo_text_color = color.rgb(252, 223, 3, 0)
var tokyo_position = 'Top'

var show_sydney_session = true and showtime
var sydney_color = color.rgb(247, 72, 238, 20)
var sydney_text_color = color.rgb(253, 75, 215)
var sydney_position = 'Bottom'

// sessions
newyork_session = time(timeframe.period, '0800-1600:1234567', 'America/New_York')
newyork_session_start = time(timeframe.period, '0800-0801:1234567', 'America/New_York')


london_session = time(timeframe.period, '0300-1100:1234567', 'America/New_York')
london_session_start = time(timeframe.period, '0300-0301:1234567', 'America/New_York')

tokyo_session = time(timeframe.period, '1900-0300:1234567', 'America/New_York')
tokyo_session_start = time(timeframe.period, '1900-1901:1234567', 'America/New_York')

sydney_session = time(timeframe.period, '1700-0100:1234567', 'America/New_York')
sydney_session_start = time(timeframe.period, '1700-1701:1234567', 'America/New_York')


markets = table.new(position.top_right, 4, 9, border_width=1)



time_utc = newyork_session ? 'NewYork ' + str.tostring(hour(timenow,"America/New_York"), "00:") + str.tostring(minute(timenow,"America/New_York"), "00:") + str.tostring(second(timenow,"America/New_York"), "00") + ""  : london_session ? 'London  '+ str.tostring(hour(timenow,"Europe/London"), "00:") + str.tostring(minute(timenow,"Europe/London"), "00:") + str.tostring(second(timenow,"Europe/London"), "00") + "" : tokyo_session ? 'Tokyo     '+ str.tostring(hour(timenow,"Asia/Tokyo"), "00:") + str.tostring(minute(timenow,"Asia/Tokyo"), "00:") + str.tostring(second(timenow,"Asia/Tokyo"), "00") + "" : 'Sydney   '+ str.tostring(hour(timenow,"Australia/Sydney"), "00:") + str.tostring(minute(timenow,"Australia/Sydney"), "00:") + str.tostring(second(timenow,"Australia/Sydney"), "00") + ""






////////////////////////////////////
////////////////////////////////////
////////////////////////////////////

cb = ta.valuewhen(buy, close, 0)
cs = ta.valuewhen(sell, close, 0)


Ptpb1 = cb * (1 + TP1/100)
Ptpb2 = cb * (1 + TP2/100)
Ptpb3 = cb * (1 + TP3/100)
Ptpb4 =  cb * (1 + TP4/100)
Pslb = cb * (1 - SL/100)

Ptps1 = cs * (1 - TP1/100)
Ptps2 = cs * (1 - TP2/100)
Ptps3 = cs * (1 - TP3/100)
Ptps4 =  cs * (1 - TP4/100) 
Psls = cs * (1 + SL/100)


//Variable Fix TP SL
tpb1t =  Ptpb1 
tpb2t =  Ptpb2 
tpb3t =  Ptpb3 
tpb4t =  Ptpb4
slbt =   Pslb 
tps1t =  Ptps1 
tps2t =  Ptps2 
tps3t =  Ptps3 
tps4t =  Ptps4
slst =   Psls 


///////////
sinceentryb = int(math.max(1, nz(ta.barssince(buy))))
highestbarb = ta.highest(high[1], sinceentryb)
lowestbarb = ta.lowest(low[1], sinceentryb)
sinceentrys = int(math.max(1, nz(ta.barssince(sell))))
highestbars = ta.highest(high[1], sinceentrys)
lowestbars = ta.lowest(low[1], sinceentrys)


hittp1 = strategy.position_size > 0 ? highestbars > tpb1t : strategy.position_size < 0 ? lowestbars < tps1t : na
hittp1t = hittp1 ? ' ✔️' : na
hittp2 = strategy.position_size > 0 ? highestbars > tpb2t : strategy.position_size < 0 ? lowestbars < tps2t : na
hittp2t = hittp2 ? ' ✔️' : na
hittp3 = strategy.position_size > 0 ? highestbars > tpb3t : strategy.position_size < 0 ? lowestbars < tps3t : na
hittp3t = hittp3 ? ' ✔️' : na
hittp4 = strategy.position_size > 0 ? highestbars > tpb4t : strategy.position_size < 0 ? lowestbars < tps4t : na
hittp4t = hittp4 ? ' ✔️' : na


///////////////
openedb = ta.barssince(buy) >= 0

stillopenb1 = ta.barssince(buy) == 1 ? true : highestbarb[1] < tpb1t and lowestbarb[1] > slbt
stillopenb2 = ta.barssince(buy) == 1 ? true : highestbarb[1] < tpb2t and lowestbarb[1] > slbt
stillopenb3 = ta.barssince(buy) == 1 ? true : highestbarb[1] < tpb3t and lowestbarb[1] > slbt
stillopenb4 = ta.barssince(buy) == 1 ? true : highestbarb[1] < tpb4t and lowestbarb[1] > slbt

hittpb1 = ta.cross(high, tpb1t) and stillopenb1 and openedb
hittpb2 = ta.cross(high, tpb2t) and stillopenb2 and openedb
hittpb3 = ta.cross(high, tpb3t) and stillopenb3 and openedb
hittpb4 = ta.cross(high, tpb4t) and stillopenb4 and openedb
hitslb = low < slbt and stillopenb4 and openedb


openeds = ta.barssince(sell) >= 0 


stillopens1 = ta.barssince(sell) == 1 ? true : highestbars[1] < slst and lowestbars[1] > tps1t
stillopens2 = ta.barssince(sell) == 1 ? true : highestbars[1] < slst and lowestbars[1] > tps2t
stillopens3 = ta.barssince(sell) == 1 ? true : highestbars[1] < slst and lowestbars[1] > tps3t
stillopens4 = ta.barssince(sell) == 1 ? true : highestbars[1] < slst and lowestbars[1] > tps4t

hittps1 = ta.cross(low, tps1t) and stillopens1 and openeds
hittps2 = ta.cross(low, tps2t) and stillopens2 and openeds
hittps3 = ta.cross(low, tps3t) and stillopens3 and openeds
hittps4 = ta.cross(low, tps4t) and stillopens4 and openeds
hitsls = high > slst and stillopens4 and openeds

//============================================
cumbuy = ta.cum(buy ? 1 : 0)
cumsell = ta.cum(sell ? 1 : 0)
cumsignal = cumbuy + cumsell
///winrate
wintpb1 = ta.cum(hittpb1 ? 1 : 0)
wintpb2 = ta.cum(hittpb2 ? 1 : 0)
wintpb3 = ta.cum(hittpb3 ? 1 : 0)
wintpb4 = ta.cum(hittpb4 ? 1 : 0)
wrslb = ta.cum(hitslb ? 1 : 0)

wintps1 = ta.cum(hittps1 ? 1 : 0)
wintps2 = ta.cum(hittps2 ? 1 : 0)
wintps3 = ta.cum(hittps3 ? 1 : 0)
wintps4 = ta.cum(hittps4 ? 1 : 0)
wrsls = ta.cum(hitsls ? 1 : 0)

cumtp1 = wintpb1 + wintps1
cumtp2 = wintpb2 + wintps2
cumtp3 = wintpb3 + wintps3
cumtp4 = wintpb4 + wintps4
cumsl = wrslb + wrsls
cumposition = cumtp1 + cumtp2 + cumtp3 + cumtp4 + cumsl  //+cumwinreverse//+cumlosereverse

wrtp1 = truncate(cumtp1 * 100 / cumsignal, 0)
wrtp2 = truncate(cumtp2 * 100 / cumsignal, 0)
wrtp3 = truncate(cumtp3 * 100 / cumsignal, 0)
wrtp4 = truncate(cumtp4 * 100 / cumsignal, 0)
wrsl = truncate(cumsl * 100 / cumsignal, 0)

winall = cumtp1 + cumtp2 + cumtp3 + cumtp4  //+cumwinreverse
wrall = winall * 100 / cumposition








//////////////////////////////
//////////////////////////////
//////////////////////////////
MTLabel(shortTitle, tradeIdeeAct, MyDate, dcaAct, dacValue, imba_entry_long_line, imba_entry_short_line, entry_Long_Three, entry_Short_Three, entry_Long_Two, entry_Short_Two, entry_Long_Four, entry_Short_Four, multiprofit, TP1, TP2, TP3, TP4, qty1, qty2, qty3, qty4, oppositeSignalACT, trailingConfigurationACT, trailingConfigurationType, movingTarget, start_date_input, asset2) =>

    string _text = '💹 ' + '' + shortTitle + ' 🤖 Øptimized v2 💹' + '\n'
    _text += '——————————————————————' + '\n'

    _text += '✔ Current position: '
    _text += strategy.position_size > 0 ? '🟩 LØNG 🟩'+ '\n' : strategy.position_size < 0 ? '🟥 SHØRT 🟥' + '\n' : 'Closed All' + '\n'
    _text += '————————————————————————' + '\n'

    _text += 'COIN: '  + str.tostring(syminfo.tickerid)                 + '\n'
    _text += '———————————————————————————' + '\n'

    _text += '⚙️ STRATEGY: ' + str.tostring(syminfo.ticker) + ' ' + name_tf + ' | Long-Term v.2'+ '\n'
    _text += '———————————————————————————' + '\n'

    _text += '📊 Total Trades: '  + str.tostring(opened_trade, '##.##') + ' 🟩 LONG: '  + str.tostring(long_count, '##.##') + ' 🟥 SHORT: '  + str.tostring(short_count, '##.##') +'\n'
    _text += '-- 📅 First Trade : ' + str.tostring(dayofmonth(MyDate)) + "-" + str.tostring(month(MyDate)) + "-" + str.tostring(year(MyDate)) + ' --'+ '\n'
    _text += '———————————————————————————' + '\n'
    _text += '🏌 >>> ACCURACY >>> ⛳️' + '\n'
    _text += '———————————————————————————' + '\n'
    _text += hittp1t + ' TP1 Hits          | ' + str.tostring(wrtp1, '##.##') +  '%'+    '  |  ' + str.tostring(cumtp1) + '/' + str.tostring(cumsignal) + '\n'
    _text += hittp2t + ' TP2 Hits          | ' + str.tostring(wrtp2, '##.##') +  '%'+    '  |  ' + str.tostring(cumtp2) + '/' + str.tostring(cumsignal) + '\n'
    _text += hittp3t + ' TP3 Hits          | ' + str.tostring(wrtp3, '##.##') +  '%'+    '  |  ' + str.tostring(cumtp3) + '/' + str.tostring(cumsignal) + '\n'
    _text += hittp4t + ' TP4 Hits          | ' + str.tostring(wrtp4, '##.##') +  '%'+    '  |  ' + str.tostring(cumtp4) + '/' + str.tostring(cumsignal) + '\n'
    _text += '———————————————————————————' + '\n'

    _text += ' 🏆️ Max Wins In A Row:  ' + str.tostring(maxWinRow, '######') + '\n'

    _text += ' ❌️ Max Losses In A Row:  ' + str.tostring(maxLossRow, '######') + '\n'


    if tradeIdeeAct == true

        _text += '———————————————————————————' + '\n'
        
    else 

        _text += na 
    _text += '🤑 Net Profit (Lev 10x): ' + str.tostring(strategy.netprofit_percent *10, '##') + '%' + '\n'
    _text += '💵 Initial Capital: 💲 ' + str.tostring(strategy.initial_capital)+ '\n'
    _text += '💵 Balance: 💲 ' + str.tostring(strategy.netprofit_percent *0.1 *strategy.initial_capital + strategy.initial_capital, '##') + '\n'
    _text += '———————————————————————————' + '\n'
    _text += '🚦 MARKET INFORMATION 🚦' + '\n'
    _text += '———————————————————————————' + '\n'
    _text += '🟣 RSI       | ' + str.tostring(ta.rsi(close,14), '##')+ '\n'
    _text += '📊 Volatility  | ' + str.tostring(atr_percent, '##') + '%' + '\n'
    _text += 'Volume  | ' + str.tostring(volume, '##')+ '\n'
    _text += can_long ? '❓️ Curent Sentiment  | Bullish' + '\n' : '❓️ Curent Sentiment  | Bearish' + '\n'
    _text += '🕘︎ Current Session (UTC)   |' + str.tostring(time_utc) +                  '\n'              
    _text += '———————————————————————————' + '\n'





label la = na

label.delete(la[1])

txt = MTLabel(shortTitle, tradeIdeeAct, MyDate, dcaAct, dacValue, imba_entry_long_line, imba_entry_short_line, entryLongThree, entryShortThree, entryLongTwo, entryShortTwo, entryLongFour, entryShortFour, multiprofit, TP1, TP2, TP3, TP4, qty1, qty2, qty3, qty4, oppositeSignalACT, trailingConfigurationACT, trailingConfigurationType, movingTarget, start_date_input, asset2)

xval = timeframe.period == "1" ? timenow + 300000 : timeframe.period == "3" ? timenow + (15*60000) : timeframe.period == "5" ? timenow + (25*60000) : timeframe.period == "15" ? timenow + (75*60000) : timeframe.period == "30" ? timenow + (150*60000) : timeframe.period == "45" ? timenow + (225*60000) : timeframe.period == "60" ? timenow + (300*60000) : timeframe.period == "180" ? timenow + (600*60000) : timeframe.period == "240" ? timenow + (1200*60000) : timeframe.period == "D" ? timenow + (7200*60000) : timenow + (50400*60000)



la := plotDashboard and panel ==  "Dashboard 1"? label.new(x=time + DashLabelxpos, xloc=xloc.bar_time, yloc=yloc.price, y=close, text=txt, color=color.new(color.blue, 80), style=label.style_label_center, textcolor=color.white, size=size.normal, textalign=text.align_center) : na








////////////////////////////

leverage11 = input.int(title="Leverage", defval=20, minval=1, maxval=125, group='Strategy Option')
var label pre_entry_label_long = na
var label positionMaxLabel_long = na
var firstOrderPlaced_long = false
var ath = high
var peak_profit_long = 0.0
var string peak_profit_string_long = na

var drawdown_long = close
var peak_drawdown_long = 0.0
if (ta.crossover(strategy.position_size, 0))
    ath := high
    peak_profit_long := (((ath - strategy.position_avg_price) / strategy.position_avg_price) * 100) * leverage11
    peak_profit_string_long := str.tostring(math.round(peak_profit_long, 2))
    positionMaxLabel_long := label.new(bar_index , ath, text ="Long Max Profit: " +"\n"+ peak_profit_string_long + "%", textcolor = color.white, color = color.green, style = label.style_label_down)
    firstOrderPlaced_long := true
    
    drawdown_long := close
    peak_drawdown_long := (((drawdown_long - strategy.position_avg_price) / strategy.position_avg_price) * 100) * leverage11

else if strategy.position_size > 0
    if (ath < high)
        ath := high
        peak_profit_long := (((ath - strategy.position_avg_price) / strategy.position_avg_price) * 100) * leverage11 // The last value is the leverage
        peak_profit_string_long := str.tostring(math.round(peak_profit_long, 2))
        if (firstOrderPlaced_long)
            label.set_x(positionMaxLabel_long, bar_index)
            label.set_y(positionMaxLabel_long, ath)
            label.set_text(positionMaxLabel_long, "Long Max Profit: " +"\n"+ peak_profit_string_long + "%")
        
    if drawdown_long > low
        drawdown_long := low
        peak_drawdown_long := (((drawdown_long - strategy.position_avg_price) / strategy.position_avg_price) * 100) * leverage11

var label pre_entry_label_short = na
var label positionMaxLabel_short = na
var firstOrderPlaced_short = false
var atl = low
var peak_profit_short = 0.0
var string peak_profit_string_short = na

var drawdown_short = close
var peak_drawdown_short = 0.0

if (ta.crossunder(strategy.position_size, 0))
    atl := low
    peak_profit_short := -(((atl - strategy.position_avg_price) / strategy.position_avg_price) * 100) * leverage11
    peak_profit_string_short := str.tostring(math.round(peak_profit_short, 2))
    positionMaxLabel_short := label.new(bar_index , atl, text ="Short Max Profit: " +"\n"+ peak_profit_string_short + "%", textcolor = color.white, color = color.red, style = label.style_label_up)
    firstOrderPlaced_short := true
    
    drawdown_short := close
    peak_drawdown_short := -(((drawdown_short - strategy.position_avg_price) / strategy.position_avg_price) * 100) * leverage11
    
else if strategy.position_size < 0
    if (atl > low)
        atl := low
        peak_profit_short := -(((atl - strategy.position_avg_price) / strategy.position_avg_price) * 100) * leverage11
        peak_profit_string_short := str.tostring(math.round(peak_profit_short, 2))
        if (firstOrderPlaced_short)
            label.set_x(positionMaxLabel_short, bar_index)
            label.set_y(positionMaxLabel_short, atl)
            label.set_text(positionMaxLabel_short, "Short Max Profit: " +"\n"+ peak_profit_string_short + "%")
        
    if drawdown_short < high
        drawdown_short := high
        peak_drawdown_short := -(((drawdown_short - strategy.position_avg_price) / strategy.position_avg_price) * 100) * leverage11





/////////////////////////////////////////////

use_atr = input.bool(true, title='ATR OR STDEV', group='TP/SL by ATR')
sl_trendline = input.bool(false, title='SL = TRENDLINE', group='TP/SL by ATR')
stdev = input.int(28, minval = 1, maxval = 50, title = "STDEV TP PERIOD", group='TP/SL by ATR')
atrper = input.int(14, minval = 1, maxval = 50, title = "ATR TP AND SL PERIOD", group='TP/SL by ATR')
//Tp & Sl by atr Mult
ratr = use_atr ? ta.valuewhen(buy, ta.atr(atrper), 0) : ta.valuewhen(buy, ta.stdev(close,stdev), 0)
ratrs = use_atr ? ta.valuewhen(buy, ta.atr(atrper), 0) : ta.valuewhen(buy, ta.stdev(close,stdev), 0)

TP5 = input.float(title='TP5 Multiplier', minval=0.0, step=0.1, defval=13, group='TP/SL by ATR')
TP6 = input.float(title='TP6 Multiplier', minval=0.0, step=0.1, defval=15, group='TP/SL by ATR')


ptp5 = input.float(1.3, title='Take Profit 5(%)', minval=0.0, step=0.1, defval=8, group='TP/SL Percentage Price') * 0.01
ptp6 = input.float(1.5, title='Take Profit 6 (%)', minval=0.0, step=0.1, defval=8, group='TP/SL Percentage Price') * 0.01

tpb5 = cb * (1 + ptp5)
tpb6 = cb * (1 + ptp6)

tps5 = cs * (1 - ptp5)
tps6 = cs * (1 - ptp6)

Ptpb5 = cb * (1 + ptp5)
Ptpb6 = cb * (1 + ptp6)

Ptps5 = cs * (1 - ptp5)
Ptps6 = cs * (1 - ptp6)

tpb5t = et == ex1 ? Ptpb5 : tpb5
tpb6t = et == ex1 ? Ptpb6 : tpb6

tps5t = et == ex1 ? Ptps5 : tps5
tps6t = et == ex1 ? Ptps6 : tps6



asset22 = input.string(title="Strategies", defval = " ### Dev Mode ### ", options=[
     " ### Dev Mode ### ",
     "_____ CRYPTO_____",
     "BTCUSDTPERP 30m/15m | Binance - v1.2",
     "ETHUSDTPERP 30m/15m | Binance - v1.2",
     " ===== FOREX ===== ",
     "GOLD 15m | MID-TERM",
     "GOLD 15m | LONG-TERM",
     "GBPAUD 15m | LONG-TERM" 

    
     ])
 
BTC_trend_days = input.int(title='Days BTC Trend', defval=3, minval=1, maxval=90, group = "Statistics Data")
XAU_trend_days = input.int(title='Days XAU Trend', defval=3, minval=1, maxval=90, group = "Statistics Data")
GBPAUD_trend_days = input.int(title='Days GBP Trend', defval=3, minval=1, maxval=90, group = "Statistics Data")

ma_choice = input.string(title="MA Choice", defval = "EMA", options=["EMA", "HMA", "RMA", "SMA", "VWAP", "VWMA", "WMA"])
ma_period = input.int(title='MA Period', defval=300, minval=0, maxval=10000)
leverage = input.int(title="Leverage", defval=20, minval=0, maxval=125)
renorm_coeff_comm = input.float(title="Renormalization", defval=1.0, minval=0.1, maxval=20.0)

//PROBABILITY TABLE
number_trades_statistics = input.int(title='Statistik Perdagangan', defval=3, minval=1, maxval=50, group = "Long/Short Input")
num_cum1 = 10// minval=1, maxval=49, group = "Cumulative Input")
num_cum2 = 20// minval=26, maxval=99, group = "Cumulative Input")
num_cum3 = 30// minval=51, maxval=124, group = "Cumulative Input")
num_cum4 = 100// minval=76, maxval=150, group = "Cumulative Input")

// ------------------------------------------
// LONG INPUTS
// ------------------------------------------
std_deviation_period_long =14//minval=1, maxval=1000, group="Long")
std_deviation_coefficient_long =0.5//minval=0, maxval=100, group="Long")
atr_period_long =14// minval=1, maxval=1000, group="Long")
atr_parameter_long = 0.5// minval=0.0, maxval=10, group = "Long")
long_entry_ma_distance_perc = 0.5// minval=0.0, maxval=100.0, group="Long")
long_stop_price_perc = input.float(title="Stop Loss LONG %", defval=4.5, minval=0, maxval=100, group="Long")
 
// ------------------------------------------
// SHORT INPUTS
// ------------------------------------------
std_deviation_period_short = 14// minval=1, maxval=1000, group="Short")
std_deviation_coefficient_short = 0.5// minval=0, maxval=100, group="Short")
atr_period_short = 14// minval=0, maxval=1000, group="Short")
atr_parameter_short = 0.5// minval=0.0, maxval=10, group = "Short")
short_entry_ma_distance_perc = 0.5// minval=0.0, maxval=100.0, group="Short")
short_stop_price_perc = input.float(title="Stop Loss SHORT %", defval=4.5, minval=0, maxval=100, group="Short")

// ---------------------------------------------------
// CREAT STRATEGI
// ---------------------------------------------------
// CRYPTO
// ---------------------------------------------------
if asset22 == "BTCUSDTPERP 30m/15m | Binance - v1.2" 
    ma_period := 300
    ma_choice := "VWMA"
    renorm_coeff_comm := 1

    std_deviation_period_long := 17
    std_deviation_coefficient_long := 1.6
    atr_period_long := 11
    atr_parameter_long := 2
    long_entry_ma_distance_perc := 2.1
    long_stop_price_perc := 4.5
 
    std_deviation_period_short := 12
    std_deviation_coefficient_short := 1.2
    atr_period_short := 14
    atr_parameter_short := 1.5
    short_entry_ma_distance_perc := 1
    short_stop_price_perc := 4.5
 
if asset22 == "ETHUSDTPERP 30m/15m | Binance - v1.2" 
    ma_period := 300
    ma_choice := "VWMA"
    renorm_coeff_comm := 1

    std_deviation_period_long := 14
    std_deviation_coefficient_long := 1
    atr_period_long := 14
    atr_parameter_long := 1
    long_entry_ma_distance_perc := 1
    long_stop_price_perc := 4.5
 
    std_deviation_period_short := 11
    std_deviation_coefficient_short := 0.5
    atr_period_short := 14
    atr_parameter_short := 1.1
    short_entry_ma_distance_perc := 1.4
    short_stop_price_perc := 4.5
// ---------------------------------------------------
// CRYPTO 15 Min
// ---------------------------------------------------


// ---------------------------------------------------
// FOREX
// ---------------------------------------------------
 
if asset22 ==  "GOLD 15m | MID-TERM"
    renorm_coeff_comm := 5
    ma_period := 300
    ma_choice := "HMA"
 
    std_deviation_period_long := 14
    std_deviation_coefficient_long := 0.2
    atr_period_long := 14
    atr_parameter_long := 0.4
    long_entry_ma_distance_perc := 0.5
    long_stop_price_perc := 4.5
 
    std_deviation_period_short := 14
    std_deviation_coefficient_short := 0
    atr_period_short := 7
    atr_parameter_short := 0.4
    short_entry_ma_distance_perc := 0.6
    short_stop_price_perc := 4.5

if asset22 == "GOLD 15m | LONG-TERM" 
    renorm_coeff_comm := 5
    ma_period := 300
    ma_choice := "RMA"
 
    std_deviation_period_long := 14
    std_deviation_coefficient_long := 0.2
    atr_period_long := 14
    atr_parameter_long := 0.4
    long_entry_ma_distance_perc := 0.5
    long_stop_price_perc := 4.5
 
    std_deviation_period_short := 14
    std_deviation_coefficient_short := 0
    atr_period_short := 7
    atr_parameter_short := 0.4
    short_entry_ma_distance_perc := 0.6
    short_stop_price_perc := 5

if asset22 == "GBPAUD 15m | LONG-TERM" 
    renorm_coeff_comm := 1.993543
    ma_period := 213
    ma_choice := "EMA"
 
    std_deviation_period_long := 1
    std_deviation_coefficient_long := 8.5
    atr_period_long := 3
    atr_parameter_long := 5.2
    long_entry_ma_distance_perc := 0
    long_stop_price_perc := 0.5
 
    std_deviation_period_short := 1
    std_deviation_coefficient_short := 8.5
    atr_period_short := 3
    atr_parameter_short := 5.1
    short_entry_ma_distance_perc := 0
    short_stop_price_perc := 0.5

// ------------------------------------------
// GLOBAL CALCULATIONS
// ------------------------------------------
ma_value = 0.0

if ma_choice == "EMA" 
    ma_value := ta.ema(close, ma_period) 
if ma_choice == "HMA" 
    ma_value := ta.hma(close, ma_period) 
if ma_choice == "RMA"
    ma_value := ta.rma(close, ma_period) 
if ma_choice == "SMA"
    ma_value := ta.sma(close, ma_period)
if ma_choice == "VWAP"
    ma_value := ta.vwap(close) 
if ma_choice == "VWMA"
    ma_value := ta.vwma(close, ma_period) 
if ma_choice == "WMA"
    ma_value := ta.wma(close, ma_period)


///

// BTC Trend Last 3 Days
[BTC_high, BTC_low] = request.security("BTCUSDTPERP", timeframe.period, [high, low])
high_counter1 = 0
low_counter1 = 0
 
BTC_trend_bars = BTC_trend_days * 24 * 60 / str.tonumber(timeframe.period) 

 
if (barstate.islast or barstate.islastconfirmedhistory) or bar_index == 0 // bar_index part initializes the "beginning of time" for Pine, so that he knows how much he could look back 
    for i = 0 to math.floor(BTC_trend_bars) - 1
        if BTC_high[i] > nz(BTC_high[i + 1], BTC_high)
            high_counter1 += 1
        if BTC_low[i] < nz(BTC_low[i + 1],BTC_low)
            low_counter1 += 1
 
 
var string BTC_trend = na 
 
BTC_trend := switch
    high_counter1 > 1.05 * low_counter1 => "⬆️"
    low_counter1 > 1.05 * high_counter1 => "⬇️"
    => "📊"

// XAUUSD Trend Last 3 Days
[XAU_high, XAU_low] = request.security("XAUUSD", timeframe.period, [high, low])
high_counter = 0
low_counter = 0
 
XAU_trend_bars = XAU_trend_days * 24 * 60 / str.tonumber(timeframe.period) 

if (barstate.islast or barstate.islastconfirmedhistory) or bar_index == 0 // bar_index part initializes the "beginning of time" for Pine, so that he knows how much he could look back 
    for i = 0 to math.floor(XAU_trend_bars) - 1
        if XAU_high[i] > nz(XAU_high[i + 1], XAU_high)
            high_counter += 1
        if XAU_low[i] < nz(XAU_low[i + 1],XAU_low)
            low_counter += 1
 
var string XAU_trend = na 
 
XAU_trend := switch
    high_counter > 1.05 * low_counter => "⬆️"
    low_counter > 1.05 * high_counter => "⬇️"
    => "📊"

// GBPAUD Trend Selama 3 Hari Terakhir
[GBPAUD_high, GBPAUD_low] = request.security("GBPAUD", timeframe.period, [high, low])
high_counter2 = 0
low_counter2 = 0
 
GBPAUD_trend_bars = GBPAUD_trend_days * 24 * 60 / str.tonumber(timeframe.period) 

if (barstate.islast or barstate.islastconfirmedhistory) or bar_index == 0 // bagian bar_index menginisialisasi "awal waktu" untuk Pine, sehingga ia tahu sejauh mana ia dapat melihat ke belakang
    for i = 0 to math.floor(GBPAUD_trend_bars) - 1
        if GBPAUD_high[i] > nz(GBPAUD_high[i + 1], GBPAUD_high)
            high_counter2 += 1
        if GBPAUD_low[i] < nz(GBPAUD_low[i + 1],GBPAUD_low)
            low_counter2 += 1
 
var string GBPAUD_trend = na 
 
GBPAUD_trend := switch
    high_counter2 > 1.05 * low_counter2 => "⬆️"
    low_counter2 > 1.05 * high_counter2 => "⬇️"
    => "📊"

// LONG TRADE
// ------------------------------------------
var label pre_entry_label_long22 = na
var label positionMaxLabel_long22 = na
var firstOrderPlaced_long22 = false
var ath22 = high
var peak_profit_long22 = 0.0
var string peak_profit_string_long22 = na

var drawdown_long22 = close
var peak_drawdown_long22 = 0.0
var entryNumber22= 0 // Deklarasi dan inisialisasi variabel entryNumber

// ------------------------------------------
// LONG CALCULATIONS // GARIS TP
// ------------------------------------------
stdev_long = ta.stdev(close, std_deviation_period_long)
stdev_long_value = stdev_long * std_deviation_coefficient_long
atr_long_value = ta.atr(atr_period_long) * atr_parameter_long
long_entry_ma_value_trigger = ma_value * (1 + (long_entry_ma_distance_perc / 100)) + atr_long_value + stdev_long_value

avg_price_long = strategy.position_avg_price * (1 + 0.003/renorm_coeff_comm)
tp1_price_long = tpb1t
tp2_price_long = tpb2t
tp3_price_long = tpb3t
tp4_price_long = tpb4t
tp5_price_long = tpb5t
tp6_price_long = tpb6t

//PREPARE FOR LONG
///prepare_value_long = long_entry_ma_value_trigger * (1 - 0.003/renorm_coeff_comm)
////prepare_long_condition = close >= long_entry_ma_value_trigger * 0.8
 
long_stop_price = strategy.position_avg_price * (1 - (long_stop_price_perc / 100)/renorm_coeff_comm)
long_stop_trade = close <= long_stop_price
long_start_trade = close >= long_entry_ma_value_trigger 

// Prepare Flag
///var bool flag_prepare_long = true
///if strategy.position_size > 9 or ta.barssince(strategy.position_size - strategy.position_size[1] == 1) < 7
  //  flag_prepare_long := false
//else
 //   flag_prepare_long := true
 

 
// ------------------------------------------
// SHORT CALCULATIONS // GARIS TP
// ------------------------------------------
stdev_short = ta.stdev(close, std_deviation_period_short)
stdev_short_value = stdev_short * std_deviation_coefficient_short
atr_short_value = ta.atr(atr_period_short) * atr_parameter_short
short_entry_ma_value_trigger = ma_value * (1 - (short_entry_ma_distance_perc / 100)) - atr_short_value - stdev_short_value

avg_price_short = strategy.position_avg_price * (1 - 0.003/renorm_coeff_comm)
tp1_price_short = tps1t
tp2_price_short = tps2t
tp3_price_short = tps3t
tp4_price_short = tps4t
tp5_price_short = tps5t
tp6_price_short = tps6t

//PREPARE FOR SHORT 
//prepare_value_short = short_entry_ma_value_trigger * (1 + 0.002/renorm_coeff_comm)
//prepare_short_condition = close <= short_entry_ma_value_trigger * 1.2
 
short_stop_price = strategy.position_avg_price * (1 + (short_stop_price_perc / 100)/renorm_coeff_comm)
short_stop_trade = close >= short_stop_price
short_start_trade = close <= short_entry_ma_value_trigger

// Prepare Flag
//var bool flag_prepare_short = true
//if strategy.position_size < - 9 or ta.barssince(strategy.position_size[1] - strategy.position_size == 1) < 7
//    flag_prepare_short := false
//else
//    flag_prepare_short := true
 
//prepare_label_complete_condition_short = prepare_short_condition and flag_prepare_short
 
// ------------------------------------------


//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// 
// ------------------------------------------
// PLOT
// ------------------------------------------
// Plot Entry Short/Long Points

color_stop_loss_price = strategy.position_size < 0 ? short_stop_price : long_stop_price



//plot(short_stop_price == 999999.99 ? na: short_stop_price, style=plot.style_circles, color=color.new(color.green, 40), linewidth=1)
//plot(long_stop_price == 0 ? na : long_stop_price, style=plot.style_circles, color=color.new(color.red, 40), linewidth=1)








////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// ------------------------------------------------------------------
// Cumulative (Long AND Short)
// ------------------------------------------------------------------
// Function for defining the Peak Profit Percentage of a specific past closed trade
tradePercentPossible(trade_num) =>
    endP = strategy.closedtrades.entry_price(trade_num) * math.abs(strategy.closedtrades.size(trade_num))
    prof = strategy.closedtrades.max_runup(trade_num)
    possibleInPercent = prof / endP *100
// Function for defining the Peak Drawdown Percentage of a specific past closed trade
drawdownPercentPossible(trade_num) =>
    money = strategy.closedtrades.entry_price(trade_num) * math.abs(strategy.closedtrades.size(trade_num))
    dd = strategy.closedtrades.max_drawdown(trade_num)
    dd_possibleInPercent = dd / money *100
// Function for defining the average Peak Profit of the last N trades
average_max_profit(N, leverage) =>
    start = strategy.closedtrades
    float sum_peak = 0.0
    float result = 0.0
    
    if barstate.islast or barstate.islastconfirmedhistory
        for i = (start - 1) to (start - N)
            sum_peak := sum_peak + tradePercentPossible(i)
        result := sum_peak * leverage / N
            
    math.round(result,2)


// Function for defining the average Peak Drawdown of the last N trades
average_max_drawdown(N, leverage) =>
    start = strategy.closedtrades
    float sum_peak = 0.0
    float result = 0.0
    
    if barstate.islast or barstate.islastconfirmedhistory
        for i = (start - 1) to (start - N)
            sum_peak := sum_peak + drawdownPercentPossible(i)
        result := sum_peak * leverage / N
            
    math.round(result,2)
    

// Function which gives the % of times a certain Peak Profit Percentage has been achieved for the last N trades
tpWinRate(_numTrade, _prof)=>
    float perc  = 0
    int count   = 0
    start       = strategy.closedtrades
    if barstate.islast or barstate.islastconfirmedhistory
        for i = start - 1 to (start - _numTrade)
            prof    = tradePercentPossible(i)
            if prof >= _prof 
                count += 1
            perc := count*100 / _numTrade 
    perc

tpWinningRate(_prof)=>
    float perc  = 0
    int count   = 0
    start       = strategy.closedtrades
    if barstate.islast or barstate.islastconfirmedhistory
        for i = start - 1 to (start - strategy.closedtrades)
            prof    = tradePercentPossible(i)
            if prof >= _prof 
                count += 1
            perc := count
    perc
tp1_winning = tpWinningRate((TP1)*100/(cb))
tp2_winning = tpWinningRate((TP2)*100/(cb))
tp3_winning = tpWinningRate((TP3)*100/(cb))
tp4_winning = tpWinningRate((TP4)*100/(cb))
tp5_winning = tpWinningRate((TP5)*100/(cb))
tp6_winning = tpWinningRate((TP6)*100/(cb))

tp1_net_profit = math.round     (tp1_winning * ((ratr * TP1)*100/(cb)) * 10, 2)
tp2_net_profit = math.round     (tp2_winning * ((ratr * TP2)*100/(cb)) * 10, 2)
tp3_net_profit = math.round     (tp3_winning * ((ratr * TP3)*100/(cb))* 10, 2)
tp4_net_profit = math.round     (tp4_winning * ((ratr * TP4)*100/(cb))* 10, 2)
tp5_net_profit = math.round     (tp5_winning * ((ratr * TP5)*100/(cb))* 10, 2)
tp6_net_profit = math.round     (tp6_winning * ((ratr * TP6)*100/(cb))* 10, 2)
//winning last 30 tp2+
tpWinningRate_30(_prof)=>
    float perc  = 0
    int count   = 0
    start       = strategy.closedtrades
    if barstate.islast or barstate.islastconfirmedhistory
        for i = start - 1 to (start - 30)
            prof    = tradePercentPossible(i)
            if prof >= _prof 
                count += 1
            perc := count
    perc


tp2_winning_30 = tpWinningRate((ratr * TP2)*100/(cb))
tp3_winning_30 = tpWinningRate((ratr * TP3)*100/(cb))
tp4_winning_30 = tpWinningRate((ratr * TP4)*100/(cb))
tp5_winning_30 = tpWinningRate((ratr * TP5)*100/(cb))
tp6_winning_30 = tpWinningRate((ratr * TP6)*100/(cb))

winning_last_30_tp2_plus = tp2_winning_30+ tp3_winning_30+tp4_winning_30+tp4_winning_30+tp6_winning_30
// ------------------------------------------------------------------
// Specific (Long OR Short)
// ------------------------------------------------------------------

// Function computing the winrate for a specific %TP and a specific kind of position (L or S)
tp_winrate(type, tp_perc, num_tr) =>
    start = strategy.closedtrades
    float perc  = 0
    int count   = 0
    int short_counted = 0
    int long_counted = 0
    
    if barstate.islast or barstate.islastconfirmedhistory
        for i = start - 1 to 0
            if type == "short" and strategy.closedtrades.size(i) < 0 and short_counted < num_tr 
                prof = tradePercentPossible(i)
                short_counted += 1
                if prof >= tp_perc 
                    count += 1
                
                perc := count*100 / num_tr 
            
            if type == "long" and strategy.closedtrades.size(i) > 0 and long_counted < num_tr
                prof = tradePercentPossible(i)
                long_counted += 1
                if prof >= tp_perc 
                    count += 1
                
                perc := count*100 / num_tr
    math.round(perc, 0)


// Function computing the max profit for N specific positions (L or S)
max_profit(type, num_tr, leverage) =>
    start = strategy.closedtrades
    float sum_peak = 0.0
    float result = 0.0
    int short_counted = 0
    int long_counted = 0
    
    if barstate.islast or barstate.islastconfirmedhistory
        for i = start - 1 to 0
            if type == "short" and strategy.closedtrades.size(i) < 0 and short_counted < num_tr
                sum_peak := sum_peak + tradePercentPossible(i)
                short_counted += 1
            
            if type == "long" and strategy.closedtrades.size(i) > 0 and long_counted < num_tr
                sum_peak := sum_peak + tradePercentPossible(i)
                long_counted += 1
            
        result := sum_peak * leverage / num_tr
            
    math.round(result,2)        
 
max_profit_30 = max_profit("long", 30, leverage) + max_profit("short", 30, leverage)

// Function computing the max drawdown for N specific positions (L or S)
max_drawdown(type, num_tr, leverage) =>
    start = strategy.closedtrades
    float sum_peak = 0.0
    float result = 0.0
    int short_counted = 0
    int long_counted = 0
    
    if barstate.islast or barstate.islastconfirmedhistory
        for i = start - 1 to 0
            if type == "short" and strategy.closedtrades.size(i) < 0 and short_counted < num_tr
                sum_peak := sum_peak + drawdownPercentPossible(i)
                short_counted += 1
            
            if type == "long" and strategy.closedtrades.size(i) > 0 and long_counted < num_tr
                sum_peak := sum_peak + drawdownPercentPossible(i)
                long_counted += 1
            
        result := sum_peak * leverage / num_tr
            
    math.round(result,2)  
 
// ------------------------------------------------------------------
// Cumulative Results
// ------------------------------------------------------------------

N100=average_max_profit(num_cum4, leverage)
N75=average_max_profit(num_cum3, leverage)
N50=average_max_profit(num_cum2, leverage)
N25=average_max_profit(num_cum1, leverage)

D100=average_max_drawdown(num_cum4, leverage)
D75=average_max_drawdown(num_cum3, leverage)
D50=average_max_drawdown(num_cum2, leverage)
D25=average_max_drawdown(num_cum1, leverage)

tp1WR_100 = tpWinRate(num_cum4, 0.1/renorm_coeff_comm)
tp2WR_100= tpWinRate(num_cum4, 0.2/renorm_coeff_comm)
tp3WR_100 = tpWinRate(num_cum4, 0.3/renorm_coeff_comm)
tp4WR_100= tpWinRate(num_cum4, 0.4/renorm_coeff_comm)
tp5WR_100 = tpWinRate(num_cum4, 0.5/renorm_coeff_comm)
tp6WR_100= tpWinRate(num_cum4, 0.6/renorm_coeff_comm)

tp1WR_75 = math.round   (tpWinRate(num_cum3, (ratr * TP1)*100/(cb)), 2)
tp2WR_75= math.round    (tpWinRate(num_cum3, (ratr * TP2)*100/(cb)), 2)
tp3WR_75 = math.round   (tpWinRate(num_cum3, (ratr * TP3)*100/(cb)), 2)
tp4WR_75= math.round    (tpWinRate(num_cum3, (ratr * TP4)*100/(cb)), 2)
tp5WR_75 = math.round   (tpWinRate(num_cum3, (ratr * TP5)*100/(cb)), 2)
tp6WR_75= math.round    (tpWinRate(num_cum3, (ratr * TP6)*100/(cb)), 2)


accuracy_last30_tp2_plus = (tp2WR_75 + tp3WR_75 + tp4WR_75+ tp5WR_75+ tp6WR_75)/5


tp1WR_50 = tpWinRate(num_cum2, (ratr * TP1)*100/(cb))
tp2WR_50= tpWinRate(num_cum2, (ratr * TP2)*100/(cb))
tp3WR_50 = tpWinRate(num_cum2, (ratr * TP3)*100/(cb))
tp4WR_50= tpWinRate(num_cum2, (ratr * TP4)*100/(cb))
tp5WR_50 = tpWinRate(num_cum2, (ratr * TP5)*100/(cb))
tp6WR_50= tpWinRate(num_cum2, (ratr * TP6)*100/(cb))
tp7WR_50 = tpWinRate(num_cum2, (ratr * TP6)*100/(cb))
tp8WR_50= tpWinRate(num_cum2, (ratr * TP6)*100/(cb))

accuracy_last20_tp2_plus = (tp2WR_50 + tp3WR_50 + tp4WR_50+ tp5WR_50+ tp6WR_50)/5


//
tp1WR_25 = tpWinRate(num_cum1, (ratr * TP1)*100/(cb))
tp2WR_25= tpWinRate(num_cum1, (ratr * TP2)*100/(cb))  
tp3WR_25 = tpWinRate(num_cum1, (ratr * TP3)*100/(cb))
tp4WR_25= tpWinRate(num_cum1, (ratr * TP4)*100/(cb))
tp5WR_25 = tpWinRate(num_cum1, (ratr * TP5)*100/(cb))
tp6WR_25= tpWinRate(num_cum1, (ratr * TP6)*100/(cb))
tp7WR_25 = tpWinRate(num_cum1, (ratr * TP6)*100/(cb))
tp8WR_25= tpWinRate(num_cum1, (ratr * TP6)*100/(cb))

accuracy_last10_tp2_plus = (tp2WR_25 + tp3WR_25 + tp4WR_25+ tp5WR_25+ tp6WR_25)/5

//TP ưinrate tất cả
tp1WR_all = math.round   (tpWinRate(strategy.closedtrades, (ratr * TP1)*100/(cb)), 2)
tp2WR_all= math.round    (tpWinRate(strategy.closedtrades, (ratr * TP2)*100/(cb)), 2)
tp3WR_all = math.round   (tpWinRate(strategy.closedtrades, (ratr * TP3)*100/(cb)), 2)
tp4WR_all= math.round    (tpWinRate(strategy.closedtrades, (ratr * TP4)*100/(cb)), 2)
tp5WR_all = math.round   (tpWinRate(strategy.closedtrades, (ratr * TP5)*100/(cb)), 2)
tp6WR_all= math.round    (tpWinRate(strategy.closedtrades, (ratr * TP6)*100/(cb)), 2)


//
next_entry = strategy.position_size > 0 ? short_entry_ma_value_trigger : long_entry_ma_value_trigger

// Plot Entry Short/Long Points
plot((barstate.islast or barstate.islastconfirmedhistory) and strategy.position_size >= 0 ? short_entry_ma_value_trigger : na, title = "Next Entry Short",  style=plot.style_cross, color=color.red, linewidth=4, offset=1, show_last=1) // plot_0 --> 1° Plot
plot((barstate.islast or barstate.islastconfirmedhistory) and strategy.position_size <= 0 ? long_entry_ma_value_trigger : na, title = "Next Entry Long", style=plot.style_cross, color=color.green, linewidth=4, offset=1, show_last=1)  
 
 //==================================================
//INFO TABEL 
//-----------------------------------------------------------------------------------------------------------------------------------------------------------------

iff_5 = strategy.position_size<0 ? -1 : 0
SR = strategy.position_size>0 ? 1 : iff_5
// Rounding levels to min tick
nround(x) =>
    n = math.round(x / syminfo.mintick) * syminfo.mintick
    n
// ———————————————————— Function used to round values.
RoundToTick(_price) =>
    math.round(_price / syminfo.mintick) * syminfo.mintick

Round(_val, _decimals) =>
    // Rounds _val to _decimals places.
    _p = math.pow(10, _decimals)
    math.round(math.abs(_val) * _p) / _p * math.sign(_val)

//Settings
//-----------------------------------------------------------------------------{
mult22 = 8 //'Multiplicative Factor', minval = 0, step = .5)
atrLen22 = 50//'ATR Length', minval = 0)
extLast22 = 4//'Extend Last', minval = 0)

//###############################################################################################
showDashboard     = input.bool(true, "Show Table", group=" ############# 🔳 TABLE Settings 🔳 ############ ")
locationDashboard = input.string("Middle Right", "Table Location", ["Top Right", "Middle Right", "Bottom Right", "Top Center", "Middle Center", "Bottom Center", "Top Left", "Middle Left", "Bottom Left"], group=" ############# 🔳 TABLE Settings 🔳 ############ ")
sizeDashboard     = input.string("Small", "Table Size", ["Large", "Normal", "Small", "Tiny"], group=" ############# 🔳 TABLE Settings 🔳 ############ ")
table_txt_size = input.string("small", "Text Size",  options = ["auto", "tiny", "small", "normal", "large", "huge"])

table_txt_bg_color =color.new(color.gray, 80),// "Text Background Color", inline = "2", group = "Table Parameters")
table_txt_color =  color.new(color.white, 25),// "Text Color", inline = "2", group = "Table Parameters")

var dashboard_loc  = locationDashboard == "Top Right" ? position.top_right : locationDashboard == "Middle Right" ? position.middle_right : locationDashboard == "Bottom Right" ? position.bottom_right : locationDashboard == "Top Center" ? position.top_center : locationDashboard == "Middle Center" ? position.middle_center : locationDashboard == "Bottom Center" ? position.bottom_center : locationDashboard == "Top Left" ? position.top_left : locationDashboard == "Middle Left" ? position.middle_left : position.bottom_left
var dashboard_size = sizeDashboard == "Large" ? size.large : sizeDashboard == "Normal" ? size.normal : sizeDashboard == "Small" ? size.small : size.tiny


///
if panel ==  "Dashboard 2"

    // Membuat tabel baru
    var myTable_2 = table.new(position = dashboard_loc, columns = 39, rows = 39, bgcolor = color.rgb(255, 255, 255), border_width = 2, frame_color = color.rgb(8, 71, 153, 7), frame_width = 2, border_color = color.rgb(8, 71, 153, 7))
    // TABLE PROBABILITY LONG/SHORT
    table.cell(table_id = myTable_2, column = 1, row = 1, text="📊 DYADYA VOVA Trade Statistics" , text_color = color.white, bgcolor = color.blue, text_size = table_txt_size)   
    table.cell(table_id = myTable_2, column = 2, row = 1, text="", text_valign=text.align_center, text_halign=text.align_center, bgcolor=table_txt_bg_color, text_color=table_txt_color, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 3, row = 1, text="", text_valign=text.align_center, text_halign=text.align_center, bgcolor=table_txt_bg_color, text_color=table_txt_color, text_size=table_txt_size)
    table.merge_cells(myTable_2, 1,1,7,1)
    table.cell(table_id = myTable_2, column = 1, row = 2, text=  '⚙️ STRATEGY: ' + str.tostring(syminfo.ticker) + ' ' + name_tf + ' | Long-Term v.2' + ' 📅 First Trade : ' + str.tostring(dayofmonth(MyDate)) + "-" + str.tostring(month(MyDate)) + "-" + str.tostring(year(MyDate)), 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue, text_color = color.white, text_size=table_txt_size)
    table.merge_cells(myTable_2, 1,2,7,2)
    //row 3
    table.cell(table_id = myTable_2, column = 1, row = 3, text="", text_valign=text.align_center, text_halign=text.align_center, bgcolor=color.rgb(8, 71, 153, 7), text_color=table_txt_color, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 2, row = 3, text=  "Accuracy" + "\n"+ "Last 10" + "\n"+ "(TP2+)", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.rgb(8, 71, 153, 7),text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 3, row = 3, text=  "Accuracy" + "\n"+ "Last 20" + "\n"+ "(TP2+)", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.rgb(8, 71, 153, 7),text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 4, row = 3, text=  "Accuracy" + "\n"+ "Last 30" + "\n"+ "(TP2+)", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.rgb(8, 71, 153, 7),text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 5, row = 3, text=  "Average" + "\n"+ "Profit" + "\n"+ "Last 30", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.rgb(8, 71, 153, 7),text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 6, row = 3, text=  "Winning" + "\n"+ "Last 30" + "\n"+ "(TP2+)", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.rgb(8, 71, 153, 7),text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 7, row = 3, text=  "Total" + "\n"+ "(max profit)" + "\n"+ "(Last 30)", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.rgb(8, 71, 153, 7),text_color = color.white, text_size=table_txt_size)
    //row 4
    table.cell(table_id = myTable_2, column = 1, row = 4, text="", text_valign=text.align_center, text_halign=text.align_center, bgcolor=color.rgb(8, 71, 153, 7), text_color=table_txt_color, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 2, row = 4, text=  + str.tostring(accuracy_last10_tp2_plus) + "%" , 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 3, row = 4, text=  + str.tostring(accuracy_last20_tp2_plus) + "%" , 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 4, row = 4, text=  + str.tostring(accuracy_last30_tp2_plus) + "%" , 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 5, row = 4, text=  + str.tostring(N75) + "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 6, row = 4, text=  + str.tostring(winning_last_30_tp2_plus) + "/30"   , 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 7, row = 4, text=  + str.tostring(max_profit_30) + "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    //row 5
    table.cell(table_id = myTable_2, column = 1, row = 5, text="Targets", text_valign=text.align_center, text_halign=text.align_center, bgcolor=color.rgb(8, 71, 153, 7), text_color=table_txt_color, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 2, row = 5, text=  "TP1", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.rgb(8, 71, 153, 7),text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 3, row = 5, text=  "TP2", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.rgb(8, 71, 153, 7),text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 4, row = 5, text=  "TP3", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.rgb(8, 71, 153, 7),text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 5, row = 5, text=  "TP4", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.rgb(8, 71, 153, 7),text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 6, row = 5, text=  "TP5", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.rgb(8, 71, 153, 7),text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 7, row = 5, text= "TP6", text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.rgb(8, 71, 153, 7),text_color = color.white, text_size=table_txt_size)
    //Row 6
    table.cell(table_id = myTable_2, column = 1, row = 6, text="Accuracy" + "\n"+ "(Last 10)", text_valign=text.align_center, text_halign=text.align_center, bgcolor=color.rgb(8, 71, 153, 7), text_color=table_txt_color, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 2, row = 6, text=   + str.tostring(tp1WR_25) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = (tp1WR_25) < 50 ? #fa7b54 : (tp1WR_25) < 70 ? color.rgb(255, 230, 0) : color.lime,text_color = color.black, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 3, row = 6, text=  + str.tostring(tp2WR_25) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = (tp2WR_25) < 50 ? #fa7b54 : (tp2WR_25) < 70 ? color.rgb(255, 230, 0) : color.lime,text_color = color.black, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 4, row = 6, text=  + str.tostring(tp3WR_25) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = (tp3WR_25) < 50 ? #fa7b54 : (tp3WR_25) < 70 ? color.rgb(255, 230, 0) : color.lime,text_color = color.black, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 5, row = 6, text=  + str.tostring(tp4WR_25) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = (tp4WR_25) < 50 ? #fa7b54 : (tp4WR_25) < 70 ? color.rgb(255, 230, 0) : color.lime,text_color = color.black, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 6, row = 6, text=  + str.tostring(tp5WR_25) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = (tp5WR_25) < 50 ? #fa7b54 : (tp5WR_25) < 70 ? color.rgb(255, 230, 0) : color.lime,text_color = color.black, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 7, row = 6, text=  + str.tostring(tp6WR_25) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = (tp6WR_25) < 50 ? #fa7b54 : (tp6WR_25) < 70 ? color.rgb(255, 230, 0) : color.lime,text_color = color.black, text_size=table_txt_size)
    //Row 7
    table.cell(table_id = myTable_2, column = 1, row = 7, text="Accuracy" + "\n"+ "(Last 20)", text_valign=text.align_center, text_halign=text.align_center, bgcolor=color.rgb(8, 71, 153, 7), text_color=table_txt_color, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 2, row = 7, text=  + str.tostring(tp1WR_50) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = (tp1WR_50) < 50 ? #fa7b54 : (tp1WR_50) < 70 ? color.rgb(255, 230, 0) : color.lime,text_color = color.black, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 3, row = 7, text=  + str.tostring(tp2WR_50) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = (tp2WR_50) < 50 ? #fa7b54 : (tp2WR_50) < 70 ? color.rgb(255, 230, 0) : color.lime,text_color = color.black, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 4, row = 7, text=  + str.tostring(tp3WR_50) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = (tp3WR_50) < 50 ? #fa7b54 : (tp3WR_50) < 70 ? color.rgb(255, 230, 0) : color.lime,text_color = color.black, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 5, row = 7, text=  + str.tostring(tp4WR_50) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = (tp4WR_50) < 50 ? #fa7b54 : (tp4WR_50) < 70 ? color.rgb(255, 230, 0) : color.lime,text_color = color.black, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 6, row = 7, text=  + str.tostring(tp5WR_50) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = (tp5WR_50) < 50 ? #fa7b54 : (tp5WR_50) < 70 ? color.rgb(255, 230, 0) : color.lime,text_color = color.black, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 7, row = 7, text=  + str.tostring(tp6WR_50) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = (tp6WR_50) < 50 ? #fa7b54: (tp6WR_50) < 70 ? color.rgb(255, 230, 0) : color.lime,text_color = color.black, text_size=table_txt_size)
  
   //Row 8
    table.cell(table_id = myTable_2, column = 1, row = 8, text="Accuracy" + "\n"+ "(Last 30)", text_valign=text.align_center, text_halign=text.align_center, bgcolor=color.rgb(8, 71, 153, 7), text_color=table_txt_color, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 2, row = 8, text=  + str.tostring(tp1WR_75) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = (tp1WR_75) < 50 ? #fa7b54 : (tp1WR_75) < 70 ? color.rgb(255, 230, 0) : color.lime,text_color = color.black, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 3, row = 8, text=  + str.tostring(tp2WR_75) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = (tp2WR_75) < 50 ? #fa7b54 : (tp2WR_75) < 70 ? color.rgb(255, 230, 0) : color.lime,text_color = color.black, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 4, row = 8, text=  + str.tostring(tp3WR_75) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = (tp3WR_75) < 50 ? #fa7b54 : (tp3WR_75) < 70 ? color.rgb(255, 230, 0) : color.lime,text_color = color.black, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 5, row = 8, text=  + str.tostring(tp4WR_75) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = (tp4WR_75) < 50 ? #fa7b54 : (tp4WR_75) < 70 ? color.rgb(255, 230, 0) : color.lime,text_color = color.black, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 6, row = 8, text=  + str.tostring(tp5WR_75) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = (tp5WR_75) < 50 ? #fa7b54 : (tp5WR_75) < 70 ? color.rgb(255, 230, 0) : color.lime,text_color = color.black, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 7, row = 8, text=  + str.tostring(tp6WR_75) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = (tp6WR_75) < 50 ? #fa7b54 : (tp6WR_75) < 70 ? color.rgb(255, 230, 0) : color.lime,text_color = color.black, text_size=table_txt_size)
  
    //
    table.cell(table_id = myTable_2, column = 1, row = 9, text=  "Profit Table (10x Leverage)", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.rgb(8, 71, 153, 7), text_color = color.white, text_size=table_txt_size)
    table.merge_cells(myTable_2, 1,9,7,9)
    //row 10
    table.cell(table_id = myTable_2, column = 1, row = 10, text="Winning", text_valign=text.align_center, text_halign=text.align_center, bgcolor=color.rgb(8, 71, 153, 7), text_color=table_txt_color, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 2, row = 10, text=  + str.tostring(tp1_winning) +  "/"  + str.tostring(strategy.closedtrades)  , 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 3, row = 10, text=  + str.tostring(tp2_winning) +  "/"  + str.tostring(strategy.closedtrades), 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 4, row = 10, text=  + str.tostring(tp3_winning) +  "/"  + str.tostring(strategy.closedtrades), 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 5, row = 10, text=  + str.tostring(tp4_winning) +  "/"  + str.tostring(strategy.closedtrades), 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 6, row = 10, text=  + str.tostring(tp5_winning) +  "/"  + str.tostring(strategy.closedtrades), 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 7, row = 10, text=  + str.tostring(tp6_winning) +  "/"  + str.tostring(strategy.closedtrades), 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)

    //row 11
    table.cell(table_id = myTable_2, column = 1, row = 11, text="Gross Profit", text_valign=text.align_center, text_halign=text.align_center, bgcolor=color.rgb(8, 71, 153, 7), text_color=table_txt_color, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 2, row = 11, text=  + str.tostring(tp1_net_profit) +  "%" , 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 3, row = 11, text=  + str.tostring(tp2_net_profit) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 4, row = 11, text=  + str.tostring(tp3_net_profit) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 5, row = 11, text=  + str.tostring(tp4_net_profit) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 6, row = 11, text=  + str.tostring(tp5_net_profit) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 7, row = 11, text=  + str.tostring(tp6_net_profit) +  "%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)

    //row 12
    table.cell(table_id = myTable_2, column = 1, row = 12, text="Gross Loss", text_valign=text.align_center, text_halign=text.align_center, bgcolor=color.rgb(8, 71, 153, 7), text_color=table_txt_color, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 2, row = 12, text=  "0%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 3, row = 12, text=  "0%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 4, row = 12, text=  "0%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 5, row = 12, text=  "0%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 6, row = 12, text=  "0%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 7, row = 12, text=  "0%", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)

    //row 13
    table.cell(table_id = myTable_2, column = 1, row = 13, text="Net Profit", text_valign=text.align_center, text_halign=text.align_center, bgcolor=color.rgb(8, 71, 153, 7), text_color=table_txt_color, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 2, row = 13, text=  + str.tostring(tp1_net_profit) +  "%" , 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 3, row = 13, text=  + str.tostring(tp2_net_profit) +  "%" , 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 4, row = 13, text=  + str.tostring(tp3_net_profit) +  "%" , 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 5, row = 13, text=  + str.tostring(tp4_net_profit) +  "%" , 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 6, row = 13, text=  + str.tostring(tp5_net_profit) +  "%" , 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)
    table.cell(table_id = myTable_2, column = 7, row = 13, text=  + str.tostring(tp6_net_profit) +  "%" , 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.blue,text_color = color.white, text_size=table_txt_size)

    //Row 14
    table.cell(table_id = myTable_2, column = 1, row = 14, text=  "", 
     text_valign=text.align_center, text_halign=text.align_center, bgcolor = color.rgb(8, 71, 153, 7), text_color = color.white, text_size=table_txt_size)
    table.merge_cells(myTable_2, 1,14,7,14)
