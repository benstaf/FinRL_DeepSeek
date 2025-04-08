# FinRL-DeepSeek: LLM-Infused Risk-Sensitive Reinforcement Learning for Trading Agents
[![](https://dcbadge.limes.pink/api/server/ekrySuRBf4)](https://discord.gg/ekrySuRBf4)

Blog: https://melwy.com/finrl_deepseek

Paper: https://arxiv.org/abs/2502.07393

Update1: The project is integrated to the original FinRL project by [AI4Finance](https://github.com/AI4Finance-Foundation/FinRL_DeepSeek)!

Update2: The project is the basis of task 1 in [FinRL contest 2025](https://open-finance-lab.github.io/FinRL_Contest_2025/)!

Installation script: `installation_script.sh`

Data: https://huggingface.co/datasets/benstaf/nasdaq_2013_2023/tree/main

Trading agents: https://huggingface.co/benstaf/Trading_agents/tree/main

Backtesting Notebook: [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/benstaf/FinRL_DeepSeek/blob/main/FinRL_DeepSeek_backtesting.ipynb)

# Results

![Alt Text](https://github.com/benstaf/FinRL_DeepSeek/blob/main/IMG_20250207_175434_001.jpg)


# Preliminary conclusion

Bull market -> PPO

Bear market -> CPPO-DeepSeek



## More details on installation of dependencies 
run `installation_script.sh` on Ubuntu server (128 GB RAM CPU instance recommended)

## Datasets and data preprocessing 

The basic dataset is FNSPID:
https://huggingface.co/datasets/Zihan1004/FNSPID (the relevant file is `Stock_news/nasdaq_exteral_data.csv`)

https://github.com/Zdong104/FNSPID_Financial_News_Dataset

https://arxiv.org/abs/2402.06698

LLM signals are added by running `sentiment_deepseek_deepinfra.py` and `risk_deepseek_deepinfra.py`, to obtain:  
- https://huggingface.co/datasets/benstaf/nasdaq_news_sentiment
- https://huggingface.co/datasets/benstaf/risk_nasdaq

Then this data is processed by `train_trade_data_deepseek_sentiment.py` and `train_trade_data_deepseek_risk.py` to generate agent-ready datasets.  
For plain PPO and CPPO, `train_trade_data.py` is used.

## Training and Environments  
- For training PPO, run:  
  `nohup mpirun --allow-run-as-root -np 8 python train_ppo.py > output_ppo.log 2>&1 &`



- For CPPO: `train_cppo.py`  
- For PPO-DeepSeek: `train_ppo_llm.py`  
- For CPPO-DeepSeek: `train_cppo_llm_risk.py`  

Environment files are:  
- `env_stocktrading.py` for PPO and CPPO, same as in the original FinRL  
- `env_stocktrading_llm.py` or `env_stocktrading_llm_01.py` for PPO-DeepSeek (depending on the desired LLM influence. More tweaking would be interesting)  
- `env_stocktrading_llm_risk.py` or `env_stocktrading_llm_risk_01.py` for CPPO-DeepSeek  

Log files are `output_ppo.log`, etc., and should be monitored during training, especially:  
- `AverageEpRet`  
- `KL`  
- `ClipFrac`  

## Evaluation  
Evaluation in the trading phase (2019-2023) happens in the `FinRL_DeepSeek_backtest.ipynb` Colab notebook.  
Metrics used are `Information Ratio`, `CVaR`, and `Rachev Ratio`, but adding others like `Outperformance frequency` would be nice.
