SimCLR 压缩成一张图

```shell
                     原始图片
                        │
               ┌────────┴────────┐
               ↓                 ↓
             Aug1              Aug2
               ↓                 ↓
              x_i               x_j
               │                 │
               └───────┬─────────┘
                       ↓
                    Encoder
                       ↓
                  h_i       h_j
                       │
                 Projection Head
                       ↓
                  z_i       z_j
                       │
                       ↓
              Cosine Similarity
                       │
                       ↓
                NT-Xent Loss
                       │
             ┌─────────┴─────────┐
             ↓                   ↓
      Positive拉近          Negative推远
```

## 第一步：一张图片生成两个视图

SimCLR随机做两次数据增强，虽然 `x_i` 和 `x_j` 长得不完全一样，但它们来同一张原图

```shell
x_i = 裁剪 + 颜色扰动
x_j = 裁剪 + 模糊 + 颜色扰动
```

x_i和x_j被定义成一个：正样本对 Positive Pair

每张生成两个增强视图：

```shell
A → A1, A2
B → B1, B2
C → C1, C2
D → D1, D2
```

所以一个 batch 最终得到：A1 A2 B1 B2 C1 C2 D1 D2。一共：2N=8

对于 `A1`： 正样本只有 A2。负样本：B1 B2 C1 C2 D1 D2


