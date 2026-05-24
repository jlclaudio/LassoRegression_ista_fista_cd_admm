# 机器学习数值分析 (Numerical Analysis for Machine Learning)
## 结课项目需求文档：Lasso与稀疏压缩感知 (Lasso and Sparse Compressed Sensing)

> [!NOTE]
> 本文档旨在为《机器学习数值分析》课程的结课项目提供标准化的需求与指导。项目要求学生不仅掌握算法的应用，更要深入理解其背后的数值优化理论、收敛性及矩阵计算的稳定性。

---

### 1. 项目背景与目标 (Background & Objectives)

**Lasso ($\ell_1$ 正则化)** 与 **压缩感知 (Compressed Sensing, CS)** 是现代机器学习与信号处理中极其重要的工具。它们打破了传统的香农-奈奎斯特采样定理，通过利用信号的“稀疏性”，能够从极少量的测量中高精度地恢复原始信号或模型参数。

**项目总目标：**
将数值线性代数、凸优化理论与机器学习实际应用相结合。学生需从底层实现针对稀疏优化问题的经典数值算法，探究算法的数值性质（如收敛速度、稳定性），并将其应用于实际的重构问题中。

---

### 2. 核心要求 (Core Requirements)

本项目要求学生完成以下四个维度的任务：

#### 2.1 数学建模理论推导 (Mathematical Formulation)
- **理论阐述**：明确阐述 $\ell_0$ 范数最小化（NP-hard问题）如何以及为何可以松弛为 $\ell_1$ 范数最小化（凸优化问题）。
- **问题定义**：写出 Lasso 的标准无约束优化形式：$\min_{x} \frac{1}{2} \|Ax - y\|_2^2 + \lambda \|x\|_1$，并推导其最优性条件（KKT条件）及次梯度（Subgradient）。
- **矩阵性质**：探讨测量矩阵 $A$ 需要满足的条件（如约束等距性 RIP (Restricted Isometry Property) 或 互相干性 (Mutual Incoherence)）以保证稀疏解的精确恢复。

#### 2.2 优化算法底层实现 (Algorithm Implementation)
> [!IMPORTANT]
> 学生**禁止**直接调用 `scikit-learn`、`cvxpy` 等高级封装库进行模型求解。必须使用纯矩阵计算库（如 `numpy`、`scipy.linalg`）从头实现以下算法。可以调用高级库作为 `Baseline` 进行结果校验。

要求至少实现以下**三种**数值优化算法中的**两种**：
1. **坐标轴下降法 (Coordinate Descent, CD)**：针对 $\ell_1$ 惩罚项不可导的特性，逐维度进行闭式解更新。
2. **近端梯度下降法 (Proximal Gradient Method)**：
   - 基础版：ISTA (Iterative Shrinkage-Thresholding Algorithm) 
   - 加速版：FISTA (Fast ISTA)，需体现 Nesterov 加速动量的推导与实现。
3. **交替方向乘子法 (ADMM)**：将无约束问题转化为约束问题，推导并实现增广拉格朗日函数的交替迭代更新。

#### 2.3 数值实验与特性分析 (Numerical Experiments and Analysis)
本部分是“数值分析”课程的核心，需重点探讨：
- **收敛性分析 (Convergence Analysis)**：绘制不同算法的 **目标函数值 vs. 迭代次数** 以及 **误差 vs. CPU计算时间** 的对比曲线，验证理论上的收敛阶数（如 FISTA 的 $\mathcal{O}(1/k^2)$）。
- **参数敏感性 (Parameter Sensitivity)**：通过正则化路径 (Regularization Path) 展示超参数 $\lambda$ 对解的稀疏度 (Sparsity Level) 和均方误差 (MSE) 的影响。
- **矩阵条件数与稳定性 (Conditioning and Stability)**：构造具有不同条件数的测量矩阵 $A$（例如通过奇异值分解人为控制奇异值衰减），分析其对优化算法数值稳定性和重构成功率的影响。

#### 2.4 实际应用场景 (Application Scenarios)
将上述自行实现的算法应用于以下至少一个具体场景：
- **一维稀疏信号恢复**：生成人工稀疏信号，添加高斯白噪声，通过随机高斯矩阵或伯努利矩阵进行降维观测，验证欠采样下的恢复概率（相变图 Phase Transition）。
- **二维图像压缩重构 (如 MRI/CT)**：将真实图像（如 Shepp-Logan Phantom）通过小波变换 (Wavelet) 或离散余弦变换 (DCT) 进行稀疏化，仅保留少量的随机傅里叶采样数据，利用 Lasso 算法重建图像。

---

### 3. 交付物 (Deliverables)

1. **项目源代码 (Source Code)**：
   - 推荐使用 Jupyter Notebook，将代码与 Markdown 公式说明结合。
   - 代码结构需清晰，包含必要的函数注释 (Docstrings)。
2. **结课学术报告 (Final Report)** (不少于 6 页，双栏 IEEE 格式)：
   - **摘要 & 引言**：问题背景描述。
   - **方法与推导**：算法的数学细节。
   - **数值实验**：详实的图表对比与数值分析讨论。
   - **结论**：算法优劣势总结及可能改进方向。

---

### 4. 评分标准 (Grading Rubric - 100 pts)

| 评分项 | 描述 | 权重 |
| :--- | :--- | :--- |
| **算法实现 (30%)** | ISTA/FISTA/CD/ADMM 等算法手写实现的正确性、向量化计算的效率（避免写显式的 for 循环迭代样本）。 | 30分 |
| **数值分析深度 (30%)** | 对收敛速率、条件数影响、$\lambda$ 敏感性的分析是否透彻，是否符合数值线性代数的理论预期。 | 30分 |
| **应用与实验 (20%)** | 实验设计是否严谨（如考虑了噪声），图表是否清晰直观（如误差对数图），应用场景的效果。 | 20分 |
| **报告与代码规范 (20%)**| 报告的逻辑清晰度、学术格式规范、公式书写准确性；代码的可读性与复现性。 | 20分 |

---

### 5. 参考文档与学习资源 (References & Resources)

> [!TIP]
> 建议在项目初期精读以下列出的经典论文和教材章节，这将极大地帮助你理解算法的理论基础。

#### 5.1 经典教材与书籍
1. **Foucart, S., & Rauhut, H. (2013).** *A Mathematical Introduction to Compressive Sensing*. Springer. (理论基石，重点关注 RIP 矩阵和 $\ell_1$ 恢复理论章节)
2. **Boyd, S., et al. (2011).** *Distributed Optimization and Statistical Learning via the Alternating Direction Method of Multipliers*. Foundations and Trends in Machine Learning. (ADMM算法的必读圣经)
3. **Nocedal, J., & Wright, S. (2006).** *Numerical Optimization*. Springer. (提供优化算法的基础视角)

#### 5.2 核心论文 (Papers)
1. **FISTA 提出者**: Beck, A., & Teboulle, M. (2009). *A Fast Iterative Shrinkage-Thresholding Algorithm for Linear Inverse Problems*. SIAM Journal on Imaging Sciences. (详细推导了收敛速率)
2. **Lasso 提出者**: Tibshirani, R. (1996). *Regression shrinkage and selection via the lasso*. Journal of the Royal Statistical Society: Series B.
3. **坐标下降法应用于Lasso**: Friedman, J., Hastie, T., & Tibshirani, R. (2010). *Regularization Paths for Generalized Linear Models via Coordinate Descent*. Journal of Statistical Software.

#### 5.3 开源实现参考 (可作为 Baseline 对比)
- [Scikit-Learn Lasso Documentation](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.Lasso.html)
- 相关的 Python `scipy.optimize` 官方文档。
