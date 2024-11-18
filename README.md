# -_asano
卒論のためのコード
from pytrends.request import TrendReq
import pandas as pd
import matplotlib.pyplot as plt

# pytrends の初期設定
pytrends = TrendReq(hl='ja-JP', tz=540)

# ファッションに関連するキーワードリスト
keywords = ["サステナビリティ ファッション", "デジタルファッション", "高級ブランド ファッション", "ジェンダーレス ファッション"]

# キーワードのトレンドデータ取得（ファッションカテゴリに限定）
pytrends.build_payload(keywords, timeframe='today 12-m', cat=306, geo='JP')  # ファッションカテゴリ、地域を日本に絞る
trend_data = pytrends.interest_over_time()

# データが取得できているか確認
if not trend_data.empty:
    # 結果を表示
    print(trend_data.head())

    # CSVファイルとして保存
    trend_data.to_csv('fashion_trend_data.csv', encoding='utf-8-sig')
    print("データが 'fashion_trend_data.csv' に保存されました。")

    # グラフ化
    plt.figure(figsize=(14, 8))
    for keyword in keywords:
        plt.plot(trend_data[keyword], label=keyword)
    plt.title("Fashion-Related Keyword Trend Comparison (Past 12 Months)")
    plt.xlabel("Date")
    plt.ylabel("Interest Over Time")
    plt.legend()
    plt.show()
else:
    print("データが取得できませんでした。キーワードや期間を見直してください。")
