import tradingview_ta as tvta


def strategy(symbol, interval):
    """
    استراتژی TradingView برای نشان دادن تمام ارزهای دیجیتال صرافی کوینکس که سیگنال خرید و فروش دارند

    Args:
        symbol: نماد ارز دیجیتال
        interval: بازه زمانی نمودار

    Returns:
        List[Indicator]: لیست اندیکاتورها
    """

    # بارگیری داده‌های قیمت

    data = tvta.STOCHRSIIndicator(
        symbol=symbol,
        interval=interval,
        fastk_period=14,
        slowk_period=3,
        slowk_matype="EMA",
        slowd_period=3,
        slowd_matype="EMA",
    ).sma_indicator()

    # ایجاد اندیکاتور سیگنال خرید

    buy_signal = tvta.CrossOverIndicator(
        data["slowk"], data["slowd"], direction="up"
    )

    # ایجاد اندیکاتور سیگنال فروش

    sell_signal = tvta.CrossOverIndicator(
        data["slowk"], data["slowd"], direction="down"
    )

    return [buy_signal, sell_signal]


def main():
    """
    برنامه اصلی
    """

    # تنظیم پارامترها

    symbols = ["BTCUSDT", "ETHUSDT", "BNBUSDT", "ADAUSDT", "USDTUSD"]
    interval = "1h"

    # ایجاد استراتژی

    strategies = [strategy(symbol, interval) for symbol in symbols]

    # نمایش سیگنال‌ها

    for strategy in strategies:
        print(strategy)


if __name__ == "__main__":
    main()
