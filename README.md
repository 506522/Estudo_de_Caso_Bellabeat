# 📊 Bellabeat Case Study – Google Data Analytics Capstone

Este projeto faz parte do curso **Google Data Analytics Professional Certificate** e tem como objetivo analisar dados de dispositivos Fitbit para gerar insights que podem ser aplicados ao marketing da **Bellabeat**, uma empresa de tecnologia de bem-estar voltada para mulheres.

---

## 🔎 1. Objetivo do Estudo
Responder à pergunta de negócio:
**“Quais tendências de uso de dispositivos inteligentes podem ser aplicadas ao público da Bellabeat para impulsionar sua estratégia de marketing?”**

---

## 📂 2. Dados Utilizados
- Fonte: [Kaggle – Fitbit Fitness Tracker Data](https://www.kaggle.com/datasets/arashnic/fitbit)
- Período: abril – maio de 2016
- Amostra: 30 usuários
- Principais arquivos:
  - `dailyActivity_merged.csv`
  - `sleepDay_merged.csv`

**Limitações:** pequena amostra, período curto, inconsistência de registros entre usuários.

---

## 🧹 3. Processamento
- Remoção de duplicatas no dataset de sono
- Padronização de datas (`ActivityDate` e `SleepDay`)
- Verificação de valores ausentes
- Junção dos datasets de atividade e sono

---

## 📊 4. Análise
### Estatísticas principais
- Média de passos: ~7.600/dia (abaixo da meta de 10.000)
- Média de sono: ~7h/dia
- Média de calorias gastas: ~2.300/dia

### Tendências
- Usuários mais ativos no sábado (~9.000 passos)  
- Usuários menos ativos na segunda (~6.800 passos)  

### Relações
- Correlação forte entre passos e calorias (r ≈ 0.8)  
- Usuários que dormem mais de 7h tendem a ser mais ativos  

---

## 📈 5. Visualizações
| Gráfico | Insight |
|---------|----------|
| Distribuição de passos diários | Maioria dos usuários caminha entre 5k–10k passos |
| Média de passos por dia da semana | Sábado é o dia mais ativo |
| Passos vs Calorias | Relação linear positiva clara |
| Sono vs Atividade | Mais sono = mais passos |

<img width="757" height="399" alt="Distribuição de Passos" src="https://github.com/user-attachments/assets/43667c65-51c0-4165-961f-64fafae16541" />

<img width="757" height="1000" alt="media_passos_semana" src="https://github.com/user-attachments/assets/38268e87-0863-463b-b603-0395dc5bde7a" />

<img width="757" height="399" alt="Relação de Passo x Calorias" src="https://github.com/user-attachments/assets/3a47027a-7f3b-4b3e-ac42-abe2db083de8" />

<img width="757" height="1000" alt="sono_vs_passos" src="https://github.com/user-attachments/assets/f405c775-5cdc-4d68-8668-16d159508141" />

--

## 💡 6. Insights Principais
1. Usuários estão **abaixo da meta de 10.000 passos/dia**.  
2. **Fim de semana mais ativo** que dias úteis.  
3. **Sono adequado (≥ 7h)** está associado a mais atividade física.  
4. **Passos têm correlação direta com gasto calórico**.  

---

## 📢 7. Recomendações para a Bellabeat
- Incentivar usuárias a atingir **10.000 passos/dia** via gamificação e notificações.  
- Criar campanhas voltadas para **quebrar longos períodos sedentários**.  
- Destacar a relação entre **sono e disposição** em materiais de marketing.  
- Lançar desafios semanais (ex.: “Fim de semana ativo”) para engajamento.  

---
