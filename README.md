# SPPRC / SPPTW Label-setting Visualizer

這是一個用來理解 Adrian Haarbach 網站文章中 **label-setting algorithm for SPPRC / SPPTW** 的小型視覺化工具。

## Online Demo

如果已啟用 GitHub Pages，網站會在這裡：

```text
https://bagle102.github.io/spprc-label-setting-visualizer/
```

## 使用方式

直接用瀏覽器打開：

```text
index.html
```

或啟用 GitHub Pages 後，直接開啟線上網址。

然後按：

- `Next Step`：一步一步看演算法；
- `Run All`：直接跑到結束；
- `Reset`：重新開始。

## 你會看到什麼？

- `U`：尚未處理的 labels；
- `P`：已處理的 labels；
- 每個 label 的 `(time, cost)`；
- label extension 的計算；
- feasible / infeasible 判斷；
- dominance pruning；
- 最後選出 target node `t` 的 minimum-cost feasible label。

## 如何修改範例？

打開 `index.html`，找到：

```javascript
const nodes = { ... }
const edges = [ ... ]
```

你可以修改：

- node 的 time window `[a,b]`
- edge 的 `time`
- edge 的 `cost`

例如：

```javascript
{from: "s", to: "A", time: 2, cost: 6}
```

代表從 `s` 到 `A` 需要 2 單位時間、成本 6。

## Research Context

這個小工具主要用於理解：

- Shortest Path Problem with Resource Constraints, SPPRC
- Shortest Path Problem with Time Windows, SPPTW
- Label-setting algorithm
- Resource extension function, REF
- Feasibility check
- Dominance rule

後續可以把 time / cost 資源改寫成 Space DTN 中的 contact time、residual capacity、buffer constraint 或 bundle size constraint。