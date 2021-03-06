# 精度評価プログラム

コンペティションで定義されている評価方法に従ってアルゴリズムの精度(誤差)を評価する.

## Requirements

- Python 3
- Libraries
  - numpy

## Usage

evaluate.pyと同じディレクトリへ移動し, ターミナルで以下のコマンドを実行すると, 各シーンにおけるスコア(誤差)と最終的なスコア(誤差)が出力される.

```bash
python evaluate.py --ground-truth-path /path/to/ground/truth --predictions-path /path/to/predictions --meta-data-path /path/to/meta/data
```

- --ground-truth-pathと--predictions-pathにはそれぞれ解答となるファイルと予測結果ファイルのパスを指定する.
- 解答となるファイルと予測結果ファイルの形式はjsonで, 以下のようなフォーマットとする. すなわち, シーン番号をkeyとして, フレーム番号順に速度を記載する.

```json
{
    "000": [...],
    "001": [...],
    ...
}
```

- --meta-data-pathには評価に必要なその他のパラメータに関するデータのパスを指定する.
- その他のパラメータの形式はjsonで, 以下のようなフォーマットとする.

```json
{
    "first_frame": 20,
    "limit_gradient": 0.07,
    "limit_intercept": 3,
    "weights": 
    {
        "000": 1,
        ...
    }
}
```

- "first_frame"には一番最初に評価するフレーム番号を指定する(1から始まる).
- "limit_gradient"と"limit_intercept"には各フレームにおける速度の誤差の上限値を決めるためのパラメータを指定する.
  - 1次関数の形をとるため, それぞれ傾きと切片となる.
- "weights"には各シーンに対する重みを指定する. すなわち, シーン番号をkeyとして, 重みの値を指定する.
  - 学習用データのアノテーションに付与されている"評価値計算時の重み付加"が"有"となるシーンを3, その他を1とする(評価用データに関しては非公開).
- 解答ファイルと予測ファイルの各シーンにおいて全てのフレームに対して予測を行っている想定である(両者で長さが同じ).
- その他のパラメータにおける重みの情報において解答ファイルと予測ファイルのシーンと対応している必要がある.
- 解答ファイル, 予測結果ファイル, その他のパラメータのデータのサンプルが./data/以下にある. 必要に応じて参照すること.
