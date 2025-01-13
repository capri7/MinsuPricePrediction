# 民泊サービス宿泊価格予測プロジェクト

本プロジェクトでは、民泊サービスの物件データを使用して、宿泊価格を予測するモデルを構築しました。データの前処理、特徴量エンジニアリング、複数の機械学習モデルの比較を通じて、高精度の予測モデルを作成しました。このプロジェクトでは、民泊宿泊価格予測で RMSE 147.97 を達成し、上位 16% にランクインしました。

---

## 📊 コンペの概要
- **課題種別**：回帰
- **データ種別**：多変量データ
- **学習データサンプル数**：55,583
- **説明変数の数**：27
- **欠損値**：有り
- **評価指標**：RMSE (Root Mean Squared Error)

---

## 📊 使用データの説明
データセットには、物件情報や宿泊価格に影響を与える可能性のある27の特徴量が含まれています。以下の図は、目的変数の分布と特徴量の関係性を視覚化したものです。

### 目的変数 (y) の分布
![目的変数の分布](images/y_distribution.png)

### 数値変数と目的変数の相関ヒートマップ
![相関ヒートマップ](images/corr_matrix.png)

### LightGBMモデルが重視する特徴量の可視化
![LightGBMの特徴量](images/lightGBM_features.png)

### CatBoostモデルが重視する特徴量の可視化
![LightGBMの特徴量](images/catboost_features.png)


---

## 🛠️ 使用モデルと環境
- **使用モデル**：
  - LightGBM（バージョン：v3.3.2）
  - CatBoost（バージョン：v1.1.1）
  - アンサンブルモデル（LightGBM + CatBoost 加重平均）

- **テスト環境**：Google Colab Pro

---

## 🎯 モデルの構築と成果
以下の3つのアプローチでモデルを作成しました：
1. **LightGBM単体**
2. **CatBoost単体**
3. **加重アンサンブル (LightGBM:CatBoost = 4:6)**

### モデルパフォーマンス
- LightGBM：RMSE 150台
- CatBoost：RMSE **147.97**（最良スコア、上位16%）
- アンサンブル：スコアの改善は見られず、CatBoost単体が最良

### 工夫した点
- **Optuna** を活用したハイパーパラメータ最適化
- **KFoldクロスバリデーション** の実装
- **TimeSeriesSplit** の試行（効果は限定的）

---

## 🔧 データ前処理の工夫
- **ターゲットエンコーディング**
   - 相関性の高い特徴量をグループ化し、平均価格を計算
- **祝日データの活用**
   - `holidays` モジュールで米国の祝日情報を特徴量に追加

### 前処理のステップ
1. **データ確認・欠損値の可視化**
2. **不要な列の削除** (`id`, `name`, `thumbnail_url`など)
3. **カテゴリ変数のエンコーディング**
   - `cancellation_policy`：順序エンコーディング
   - `zipcode`：最初の3桁でグループ化し、平均価格を計算
4. **新規特徴量エンジニアリング**
   - `amenities`（設備のカウント化）
   - `host_since`からの運営年数計算
   - `first_review`と`last_review`の差分
   - `bedrooms`と`accommodates`の組み合わせで価格の平均を計算

---

## 📈 探索的データ分析 (EDA)
- **目的変数の分布**
  - 右に長いテールを持つため、対数変換を試行
- **数値変数とyの相関**
  - `accommodates`, `bathrooms`, `bedrooms`, `beds` との間に0.45~0.5の相関
  - その他の特徴量では非線形の関係

**数値変数と目的変数の散布図**：

- number_reviewとyの散布図
![number_reviewとyの散布図](images/number_review.png)

- review_scores_ratingとyの散布図
![review_scores_ratingとyの散布図](images/review_scores_rating.png)

---

## ✅ 結論と今後の課題
### 成果
- 最良モデル：CatBoost
- 最終スコア：RMSE **147.97**（上位16%）

### 改善点と次のステップ
- 特徴量エンジニアリングのさらなる強化
- 外れ値の検出と処理
- モデルの解釈性向上（SHAP値の導入）

---

## 📂 ファイル構成
```
├── data
│   ├── train.csv
│   └── test.csv
├── notebooks
│   ├── train_preprocessing.ipynb
│   └── test_preprocessing.ipynb   
├── models
│   ├── catboost.ipynb
│   ├── lightgbm.ipynb
│   └── ensamble.ipynb
├── images
│   ├── y_distribution.png
│   ├── corr_matrix.png
│   ├── catboost_feature.png
│   ├── lightGBM_feature.png
│   ├── number_review.png
│   └── review_scores_rating.png
├── README.md
└── license.txt
```

---

## 📧 連絡先
このプロジェクトに関するお問い合わせやコラボレーションのご提案は、以下の連絡先までお願いします。
- GitHub: [capri7](https://github.com/capri7)
- LinkedIn: [プロフィールリンク](https://www.linkedin.com/in/kazue-hayakawa-a650672b1/)

---

