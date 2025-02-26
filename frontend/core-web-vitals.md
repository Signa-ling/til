# Core Web Vitals

Web の UX に関する品質指標のうち、特に重要な指標のこと。2025.02.25 時点では LCP, INP, CLS という3つが据えられている。

### LCP(Largest Contentful Paint)

読み込みパフォーマンスの測定をする指標で、以下の要素を計測対象としてみなしている

- `<img>` 要素
- `<svg>` 要素内の `<image>` 要素
- `<video>` 要素
- `url()` 関数を使用して読み込まれた背景画像を持つ要素
- テキストノードや他のインラインレベルのテキスト要素の子要素を含むブロックレベルの要素

ユーザーがページに移動したタイミングから、これらの LCP 対象のうち最大のもののレンダリング完了時間までの期間が LCP 値となる。

LCP の良し悪しは以下の通り

- 2.5秒未満: 良好
- 2.5 ~ 4.0秒未満: 要改善
- 4.0秒以上: 悪い

### INP(Interaction to Next Paint)

ユーザー操作に対するインタラクティブ性の指標のこと。

あるページアクセス中に発生したクリック、タップ、キーボード操作のレイテンシから最大時間を要した値が INP 値となる。

INP の良し悪しは以下の通り

- 200ミリ秒未満: 良好
- 200ミリ秒 ~ 500ミリ秒未満: 要改善
- 500ミリ秒以上: 悪い

ちなみに [FID(First Input Delay)](https://web.dev/articles/fid?hl=ja) の後継指標で、FID はページの最初の操作の[入力遅延](https://web.dev/articles/optimize-input-delay?hl=ja#what_is_input_delay)（ユーザーの操作からその操作のイベントコールバックが実行開始するまでの時間）のみを計測していたらしい。

### CLS（Cumulative Layout Shift）

視覚的安定性に関する指標のことで、わかりやすくいうと意図しないレイアウトのズレがどの程度発生したかを測定する。

こうしたズレ（レイアウト移動）のスコアの最大値が CLS 値になる。

ある期間（例えば1秒）の中で発生したズレのスコアは impact fraction （衝撃率）と distance fraction （移動率）を用いて以下のように求める。

```javascript
const calcImpactFraction = (beforeRect, afterRect, viewportArea) => {
  const unionArea = calcUnionArea(beforeRect, afterRect); // 移動前後の面積の和集合をとる

  return unionArea / viewportArea;
};

const calcDistanceFraction = (beforeRect, afterRect, maxDimension) => {
  const beforeCenterX = beforeRect.x + (beforeRect.width / 2);
  const beforeCenterY = beforeRect.y + (beforeRect.height / 2);
  const afterCenterX = afterRect.x + (afterRect.width / 2);
  const afterCenterY = afterRect.y + (afterRect.height / 2);

  // フレーム間の縦横の移動距離のうち大きい方を幅 or 高さの最大で割る
  const moveX = abs(afterCenterX - beforeCenterX);
  const moveY = abs(afterCenterY - beforeCenterY);
  const distanceMoved = max(moveX, moveY);

  return distanceMoved / maxDimension;
};

const calcLayoutShiftScore = (movedElements, viewportWidth, viewportHeight) => {
    // ビューポートの面積
    const viewportArea = viewportWidth * viewportHeight;
    // 幅と高さの大きい方
    const maxDimension = max(viewportWidth, viewportHeight);

    let layoutShiftScore = 0;

    movedElements.forEach(element => {
      const beforeRect = element.beforeRect;  // n フレーム目の要素の位置{ x, y, width, height }
      const afterRect = element.afterRect;  // n + 1 フレーム目の要素の位置{ x, y, width, height }

      const elementScore = impactFraction * distanceFraction;
      layoutShiftScore += elementScore;
    })

    return layoutShiftScore;
};
```

上記を元に期間毎に算出された LayoutShiftScore のうち、最も高かったものが CLS になる。

![ズレのイメージ（(Cumulative Layout Shift（CLS）  |  Articles  |  web.dev)[https://web.dev/articles/cls?hl=ja]より引用し、補足している）](./assets/cls_calculate_image.png)

CLS の良し悪しは以下の通り

- 0.1以下: 良好
- 0.1 ~ 0.25: 要改善
- 0.25以上: 悪い


なおズレは、リソースが非同期に読み込まれた場合や、既存コンテンツの前に DOM 要素が動的にページに追加された場合などに発生する。
具体的にはサイズが不明な動画像の読み込みや、サイズを動的に変更するサードパーティの広告などが原因となるらしい。

## 備考

Web Vitals には他にも指標があるが、これは今後学んだ際に別途記載していく

## 参考

- [Web Vitals  |  Articles  |  web.dev](https://web.dev/articles/vitals#evolving-web-vitals)
- [Largest Contentful Paint（LCP）  |  Articles  |  web.dev](https://web.dev/articles/lcp?hl=ja)
- [Interaction to Next Paint（INP）  |  Articles  |  web.dev](https://web.dev/articles/inp?hl=ja)
- [First Input Delay（FID）  |  Articles  |  web.dev](https://web.dev/articles/fid?hl=ja)
- [入力遅延を最適化する  |  Articles  |  web.dev](https://web.dev/articles/optimize-input-delay?hl=ja)
- [Cumulative Layout Shift（CLS）  |  Articles  |  web.dev](https://web.dev/articles/cls?hl=ja)
- [Chrome Speed - Web Vitals Changelogs](http://chromium.googlesource.com/chromium/src/+/master/docs/speed/metrics_changelog/README.md)
  - 上記では取り上げてないが、変更ログを確認できるので関連して掲載しておく
