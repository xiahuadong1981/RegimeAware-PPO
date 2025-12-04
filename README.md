---

# 财务分析仓库（Financial Analysis Repository）

本仓库包含对资产回报、投资组合优化和风险评估的全面财务分析，使用的历史金融数据主要来自纽约大学（NYU）Aswath Damodaran 教授提供的 “Finance Portfolio 2.csv”。

---

## 数据集概览（Dataset Overview）

### Finance Portfolio.csv

主要数据集 “Finance Portfolio 2.csv” 由 NYU 的 Aswath Damodaran 教授提供，包含多个资产类别的历史年度收益，包括：

* 股票（如：S&P 500、小盘股）
* 债券（如：10 年期国债、Baa 企业债）
* 房地产
* 大宗商品（如黄金）
* 无风险资产（如国库券 T-Bills）

该数据集跨越多个历史时期，覆盖多轮经济周期及关键金融压力年份（1931、1974、1987、2001、2008 和 2020），非常适合分析不同市场情境下的资产行为，并用于稳健的投资组合风险管理策略。

### cleaned_real_asset_returns.csv

原始数据的清洗和处理版本，适合计算分析。通过运行脚本 `cleaned_real_asset_returns.py` 生成，包含标准化并格式化的收益率数据。

---

## Python 脚本（Python Scripts）

### 标准财务分析脚本（Standard Financial Analysis Scripts）

* **`cleaned_real_asset_returns.py`**：用于清洗和处理原始数据的脚本
* **`sharpe_stress_analysis.py`**：计算资产在压力和非压力时期的夏普比率
* **`rolling_sharpe_analysis.py`**：计算 2/5/10 年窗口的滚动夏普比率
* **`centered_window_analysis.py`**：围绕压力年份执行居中窗口的夏普比率分析
* **`bootstrap_ci.py`**：使用蒙特卡洛模拟生成累计收益的自举置信区间
* **`resilience_analysis.py`**：基于波动率、累计收益和 CAGR 计算韧性评分
* **`pca_resilience.py`**：使用 PCA 提取资产韧性得分
* **`portfolio_optimization.py`**：基于最大化夏普比率的投资组合优化
* **`sortino_optimization.py`**：基于最大化 Sortino 比率的投资组合优化
* **`factor_attribution.py`**：使用 Fama-French + 动量因子进行因子归因分析
* **`drawdown_analysis.py`**：计算每个资产的最大历史回撤
* **`rolling_factor_analysis.py`**：计算滚动 beta 并对因子回归残差执行统计诊断测试

### 具备 Regime（市场状态）感知能力的 AI 扩展脚本（Regime-Aware AI Extensions）

* **`regime_features.py`**：提取市场状态相关特征，如回撤、波动率、利差等
* **`regime_clustering.py`**：使用 KMeans、GMM、HMM 进行无监督状态聚类
* **`regime_classification.py`**：使用自编码器、随机森林、XGBoost 进行监督状态分类
* **`monte_carlo_simulation.py`**：基于静态转移概率的市场状态蒙特卡洛模拟
* **`enhanced_monte_carlo.py`**：结合宏观经济输入进行动态市场状态建模的蒙特卡洛模拟
* **`regime_rl_env.py`**：自定义 OpenAI Gym 环境，以 HMM 的状态概率作为状态输入
* **`ppo_train_regime_rl.py`**：使用 stable-baselines3 的 PPO 算法训练市场状态感知的投资组合策略
* **`ppo_rl_pipeline.py`**：具有简单奖励函数的基础 PPO agent
* **`ppo_rl_stable.py`**：最终实现，包含交易成本建模、波动率调整奖励、clipping、资本重置与冲击模拟

---

### 输出文件汇总（Output Summary）

每个脚本会生成如下输出：

* `ppo_rl_stable.py`:

  * `ppo_advanced_vs_equal_returns.csv` — 最终评估指标
  * `ppo_vs_equal_portfolio_growth.png` — 累计增长对比
* `sharpe_stress_analysis.py`:

  * `sharpe_ratios_stress.csv`
* `drawdown_analysis.py`:

  * `max_drawdowns.csv`
* `bootstrap_ci.py`:

  * `bootstrap_cumulative_returns_ci.csv`
* `rolling_sharpe_analysis.py`:

  * `rolling_sharpe_ratios.csv`
* `rolling_cagr_comparison_all_strategies.png`:

  * 各策略的滚动 CAGR 图

---

## PPO 变体输出（PPO Variant Outputs）

### 已训练的模型（Trained Models）

PPO 模型使用不同的 ablation 设置训练，保存在：

* `stable/ppo_rl_stable_seed0.zip` 至 `stable/ppo_rl_stable_seed4.zip`
* `nocost/nocost_seed0.zip` 至 `nocost/nocost_seed4.zip`
* `noreset/noreset_seed0.zip` 至 `noreset/noreset_seed4.zip`

### 生成的评估文件（Generated Evaluation Files）

* `ppo_vs_equal_portfolio_growth.png` — PPO 与等权投资组合的增长对比
* `ppo_vs_equal_returns.csv` — PPO 与基准策略的最终对数收益
* `ppo_vs_equal_returns_clipped.csv` — 用于压力可视化的裁剪收益序列
* `rolling_cagr_comparison_all_strategies.png` — 等权、动量、夏普优化、PPO 的滚动 CAGR 对比
* `rolling_cagr_stress_overlay.png` — 标注金融压力年份的 PPO CAGR 图
* `KDECAGR.png` — 各基准策略的 CAGR 分布概率密度估计
* `SHAP.png` — 展示 PPO 策略资产配置决策的 SHAP 值

---

## 结果（Results）

* **`analysis_results.md`**：传统投资组合分析的全部结果总结
* **`results.md`**：市场状态感知蒙特卡洛模拟、聚类质量、RL 结果的总结
* **`rl_analysis_report.md`**：PPO 强化学习投资组合训练的详细总结；包含 ablation（nocost、noreset、noclip）、统计测试与 SHAP 可解释性
* **`ppo_full_pipeline.py`**：可一次执行 PPO 训练与评估的完整流程脚本，覆盖基线与多种 ablation 设置，并支持多随机种子复现

---

## 分析组件（Analysis Components）

* 夏普与 Sortino 比率计算
* 固定与动态市场状态的蒙特卡洛模拟
* 基于 PCA 的韧性得分
* 投资组合优化（夏普、Sortino）
* Fama-French + 动量因子的因子归因
* KMeans、GMM、HMM 的市场状态聚类
* 监督式市场状态预测
* 基于强化学习的投资组合优化
* 风险评估：VaR、CVaR、最大回撤
* 基于 PPO 的市场状态感知训练
* PPO 与等权、优化组合的对比
* 滚动 CAGR 与压力测试
* PPO ablation：nocost、noclip、noreset
* PPO 策略的 SHAP 可解释性

---

---

## License

所有内容 © Gabriel Nixon Raj。本项目遵循 MIT License 授权协议。详情见 `LICENSE` 文件。

---

## 致谢（Acknowledgments）

本项目基于纽约大学（NYU）**Aswath Damodaran** 教授公开提供的金融数据，他对数据透明和学术开放的坚持持续给予启发。

特别感谢以下工具和库的贡献者：

* [Stable-Baselines3](https://github.com/DLR-RM/stable-baselines3) — 强化学习代理
* [scikit-learn](https://scikit-learn.org/) 与 [XGBoost](https://xgboost.readthedocs.io/) — 监督式市场状态分类
* [hmmlearn](https://hmmlearn.readthedocs.io/) 与 [pomegranate](https://github.com/jmschrei/pomegranate) — 概率状态建模
* [matplotlib](https://matplotlib.org/) 与 [seaborn](https://seaborn.pydata.org/) — 绘图
* NYU 数据科学中心 — 计算资源与学术环境

本项目完成于 **纽约大学数据科学硕士项目** 的学术探索期间。

---


