---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 制限および最大値
{: #limits-and-maximum-values}

## 存続時間の最大値はありますか。 最小値もありますか。

存続時間の最大値は 2,147,483,647 秒で、これは約 68 年と同等です。 最小値は 0 秒です。

## オリジンと TTL の項目数に制限はありますか

はい。両方を合計した制限は、CDN につき 75 項目です。

## Akamai CDN から配信できる最大ファイル・サイズは

1.8 GB より大きいファイルを取得したり配信したりしようとすると、デフォルトのパフォーマンス構成では`「403 アクセス禁止」`応答が表示されます。 ラージ・ファイルの最適化が有効になっている場合、320 GB までのファイル・ダウンロードが可能です。
