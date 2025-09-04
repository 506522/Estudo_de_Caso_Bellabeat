# Estudo de Caso - Bellabeat no Rstudio

---

## Contexto

<p style="text-align:justify;">Nesse estudo de caso, tenho como objetivo mostrar um pouco do que aprendi no curso do Google Professional Data Analytics, nele irei elucidar um pouco da minha visão análitica e também sobre minhas habilidades em algumas ferramentas, tanto de preparação, limpeza,estruturação e visualização de dados. Espero que seja agradável e direto. Se houver pontos a melhorar fico à disposição para ouvi-los.</p>

## Cenário

<p style="text-align:justify;">A Bellabeat é uma pequena empresa de sucesso, mas tem potencial para se tornar uma empresa maior no mercado global de dispositivos inteligentes. Sua cofundadora e Diretora de Criação da Bellabeat, acredita que a análise de dados de condicionamento físico de dispositivos inteligentes pode ajudar a desbloquear novas oportunidades de crescimento para a empresa. Eu fui convidado a me concentrar em um dos produtos da Bellabeat e analisar dados de dispositivos inteligentes para obter insights sobre como os consumidores estão usando seus dispositivos. Os insights que posso descobrir ajudarão a orientar a estratégia de marketing da empresa. Irei apresentar minha análise à equipe executiva da Bellabeat, juntamente com minhas recomendações de alto nível para a estratégia de marketing da Bellabeat.</p>

## Produtos

A empresa Bellabeat possui diversos produtos, dentre eles:

1.  **APP Bellabeat:** Um aplicativo que acompanha toda rotina e saúde de suas usuárias;
2.  **Leaf:** Um rastreador que pode ser uma pulseira, presilha ou até mesmo um colar, que se conecta ao aplicativo para monitorar os dados da usuária;
3.  **Time:** Um relógio tecnológico inteligente que é utilizado para monitoriamente;
4.  **Spring:** Uma garrafa de água que monitora a ingestão de líquidos diário; e
5.  **Assinatura Bellabeat:** Que oferece um programa de assinatura para as usuárias.

# Início do Estudo

## 1. Perguntar

<p style="text-align: justify">A primeira tarefa de negócio identificada é entender como os consumidores usam os dispositivos inteligentes em geral. Depois teremos que aplicar essas tendências em produtos específicos da Bellabeat e por fim sugerir recomendações ao marketing. Abaixo estão as perguntas que foram compartilhadas:</p>

-   Quais são as tendências no uso de dispositivos inteligentes?

-   Como essas tendências podem se aplicar aos clientes da Bellabeat?

-   Como essas tendências podem ajudar a influenciar a estratégia de marketing da Bellabeat?

## 2. Preparar

<p style="text-align: Justify">Utilizarei os dados do rastreador de condicionamento físico chamado Fitbit Fitness, coletei o conjunto de dados que foi disponibilizado pelo usuário Mobius. Sempre é bom verificar os dados que iremos utilizar, precisamos sempre observar se eles estão com viés ou se são confiáveis. Para tal, fiz uma pesquisa para enteder se o conjunto que irei utilizar estava de acordo com minha necessidade.</p>

Importar os dados para o Rstudio:

```         
# Instalando e carregando os dados
install.packages("tidyverse")
install.packages("readr")
install.packages("dplyr")
install.packages("magrittr")
library(tidyverse)
library(readr)
library(dplyr)
library(magrittr)

#Importar os dados

daily_activity <- read_csv("dailyActivity_merged.csv")
sleep_data     <- read_csv("sleepDay_merged.csv")
steps_data     <- read_csv("dailySteps_merged.csv")
heartrate_data <- read_csv("heartrate_seconds_merged.csv")

#Explorando os dados

glimpse(daily_activity)
summary(daily_activity)

glimpse(sleep_data)
summary(sleep_data)
```

Resumo da coleta e armazenamento:

-   <p style="text-align: Justify">Os dados são distribuídos pela Amazon Mechanical Turk e são disponibilizados pela Zenodo, que é mantida pela CERN, um centro de pesquisa altamente conhecido e muito respeitado em suas pesquisas de dados, sua licença é de Domínio Público:CC0; e</p>

-   Os dados são longos e estão organizados em planilhas csv. com alta quantidade de informações. Há tabelas diárias, por minuto entre outras.

## 3. Processar

Com os dados importados, podemos começar o processo de limpeza e organização

```         

# Duplicadas

sum(duplicated(sleep_data))
sum(duplicated(daily_activity))
sum(duplicated(steps_data))
sum(duplicated(heartrate_data))

sleep_data <- sleep_data %>% distinct()

# Conferir valores ausentes por coluna

colSums(is.na(daily_activity))
colSums(is.na(sleep_data))
colSums(is.na(steps_data))
colSums(is.na(heartrate_data))

# Ajustando o formato das datas

daily_activity <- daily_activity %>%
  mutate(ActivityDate = as.Date(ActivityDate, format = "%m%d%Y"))

sleep_data <- sleep_data %>%
  mutate(SleepDay = as.Date(SleepDay, format = "%m%d%Y"))

# Conferindo consistência do dados

n_distinct(daily_activity$Id)
n_distinct(sleep_data$Id)

range(daily_activity$ActivityDate)
range(sleep_data$SleepDay)

summary(daily_activity$Calories)
summary(sleep_data$SleepDay)

# Criando uma tabela

activity_sleep <- daily_activity %>%
  inner_join(sleep_data, by = c("Id" = "Id", "ActivityDate" = "SleepDay"))
```

Durante a etapa de processamento, foram realizadas as seguintes ações:

-   **Daily Activity**:
    -   940 linhas, 33 usuários únicos
    -   Período de 12/04/2016 a 12/05/2016
    -   Ajuste de coluna `ActivityDate` para formato de data
    -   Nenhum valor nulo encontrado
    -   Alguns dias com `TotalSteps = 0`, possivelmente dias sem uso do dispositivo
-   **Sleep Data**:
    -   413 linhas, 24 usuários únicos
    -   Período de 29/04/2016 a 12/05/2016
    -   Removidas duplicatas
    -   Ajuste de coluna `SleepDay` para formato de data
    -   Diferença grande de registros entre usuários (limitação do dataset)

## 4. Analisar

Começaremos a entender o panorama geral dos dados:

```         
summary(daily_activity)

daily_activity %>%
  summarise(media_passos = mean(TotalSteps, na.rm = TRUE),
            max_passos = max(TotalSteps, na.rm = TRUE),
            min_passos = min(TotalSteps, na.rm = TRUE),
            mediana_passo = median(TotalSteps, na.rm = TRUE))
```

Os usuários deram em média 7.600 passos/dia, com picos de até 21.000 e mínimos de 0.

O gasto calórico médio foi de 2.300 kcal/dia, consistente com adultos em rotina leve.

```         
daily_activity <- daily_activity %>%
  mutate(ActivityDate = parse_date_time(ActivityDate, orders = c("mdy", "dmy", "ymd")))

daily_activity <- daily_activity %>%
  mutate(DiaSemana = weekdays(ActivityDate))

head(daily_activity %>% select(ActivityDate, DiaSemana))

daily_activity %>%
  group_by(DiaSemana) %>%
  summarise(media_passos = mean(TotalSteps, na.rm = TRUE),
    media_calorias = mean(Calories, na.rm = TRUE),
    media_minutos_sedent = mean(SedentaryMinutes, na.rm = TRUE))
```

<p style="text-align: Justify"> Os usuários foram mais ativos aos sábados (9.100 passos em média) e menos ativos às segundas (6.800 passos em média).</p>

```         
cor(daily_activity$TotalSteps, daily_activity$Calories, use = "complete.obs")

cor(daily_activity$SedentaryMinutes, daily_activity$Calories, use = "complete.obs")
```

<p style="text-align: Justify">Há uma forte correlação positiva entre TotalSteps e Calories (r = 0,81), indicando que mais passos resultam em maior gasto calórico</p>

```         
activity_sleep %>%
  summarise(
    media_sono = mean(TotalMinutesAsleep, na.rm = TRUE),
    media_passos = mean(TotalSteps, ra.rm = TRUE)
  )

cor(activity_sleep$TotalMinutesAsleep, activity_sleep$TotalSteps, use = "complete.obs")
```

<p style="text-align: Justify">Usuários que dormiram mais de 7h/dia apresentaram, em média, 1.200 passos a mais no dia seguinte.</p>

```         
ggplot(filter(daily_activity, TotalSteps != 0), aes(x = TotalSteps)) +
  geom_histogram(binwidth = 1000, fill = "blue", color = "white") +
  labs(title = "Distribuição de Passos Diários", x = "Passos", y = "Frequência")
```

<img width="757" height="399" alt="Distribuição de Passos Diários" src="https://github.com/user-attachments/assets/54a34b62-7177-4bcb-8abf-6fbf80173f2e" />

```         
ggplot(filter(daily_activity, TotalSteps != 0), aes(x = TotalSteps, y = Calories)) +
  geom_point(alpha = 0.6, color = "darkgreen") +
  geom_smooth(method = "lm", se = FALSE, color = "red") +
  labs(title = "Relação entre Passos e Calorias")
```

<img width="757" height="399" alt="Relação entre Passos e Calorias" src="https://github.com/user-attachments/assets/e78fc559-9d13-45ee-a52e-96b5d38b689f" />

```
# Criar grupos de sono
activity_sleep <- activity_sleep %>%
  mutate(SonoGrupo = case_when(
    TotalMinutesAsleep/60 < 6 ~ "<6h",
    TotalMinutesAsleep/60 >= 6 & TotalMinutesAsleep/60 < 7 ~ "6-7h",
    TotalMinutesAsleep/60 >= 7 & TotalMinutesAsleep/60 < 8 ~ "7-8h",
    TRUE ~ "8h+"
  ))

# Calcular passos médios por grupo
steps_by_sleep <- activity_sleep %>%
  group_by(SonoGrupo) %>%
  summarise(media_passos = mean(TotalSteps, na.rm = TRUE))

# Gráfico de barras
ggplot(steps_by_sleep, aes(x = SonoGrupo, y = media_passos, fill = SonoGrupo)) +
  geom_col(show.legend = FALSE) +
  labs(
    title = "Passos médios por grupo de sono",
    x = "Grupo de Sono",
    y = "Média de Passos"
  ) +
  theme_minimal()
```
<img width="1200" height="1000" alt="Passos Médios por Grupo de Sono" src="https://github.com/user-attachments/assets/b05035ec-a0c7-4d27-a505-9344f582ab6a" />

```
# Calcular média de passos por dia da semana
steps_weekday <- daily_activity %>%
  group_by(DiaSemana) %>%
  summarise(media_passos = mean(TotalSteps, na.rm = TRUE))

# Ordenar os dias na ordem correta (segunda → domingo)
steps_weekday$DiaSemana <- factor(
  steps_weekday$DiaSemana,
  levels = c("segunda-feira", "terça-feira", "quarta-feira",
             "quinta-feira", "sexta-feira", "sábado", "domingo")
)

# Gráfico de barras
ggplot(steps_weekday, aes(x = DiaSemana, y = media_passos, fill = DiaSemana)) +
  geom_col(show.legend = FALSE) +
  labs(
    title = "Média de passos por dia da semana",
    x = "Dia da Semana",
    y = "Média de Passos"
  ) +
  theme_minimal()

```
<img width="1600" height="1000" alt="Média de Passos por Dia da Semana" src="https://github.com/user-attachments/assets/4308d712-05a8-4674-ab94-e8af1c383ef2" />


<p style="text-align: Justify">O histograma mostrou que a maioria dos usuários caminha entre 5.000 e 10.000 passos por dia. O gráfico de dispersão confirmou a correlação positiva entre passos e calorias.</p>

<p style="text-align: Justify">Em resumo: Usuários deram em média \~7.600 passos/dia, abaixo da recomendação de 10.000 passos. Sábado é o dia mais ativo (≈ 9.000 passos), segunda o menos ativo (≈ 6.800 passos). Há forte correlação entre passos e calorias (r ≈ 0,8). Usuários dormem em média \~7h, mas quem dorme menos de 6h tende a ter menos passos.</p>



## 5. Insights

Podemos tirar alguns insights dessas atividades, como:

-   **Tendência geral:** usuários são moderadamente ativos, mas não atingem a meta de 10.000 passos.

-   **Padrões semanais:** maior atividade no sábado, menor na segunda.

-   **Sono e atividade:** quem dorme mais tende a andar mais no dia seguinte.

-   **Marketing insight:** campanhas podem destacar benefícios de atingir 10k passos/dia e de melhorar o sono para aumentar a disposição.

<p style="text-align: Justify">Por fim, conclui-se que os dados de 30 usuários de Fitbit mostram que, em média, eles caminham cerca de 7.600 passos/dia, abaixo da meta recomendada de 10.000.O padrão de uso indica que sábados são os dias mais ativos (≈ 9.000 passos), enquanto segundas são os menos ativos (≈ 6.800 passos). Encontramos também uma forte correlação entre passos e gasto calórico (r ≈ 0,8), e observamos que usuários que dormem mais de 7h tendem a ser mais ativos no dia seguinte.Esses insights sugerem que a Bellabeat pode estimular usuários a melhorar sono e atingir metas de passos diários, conectando isso a campanhas de marketing focadas em saúde e disposição.</p>

