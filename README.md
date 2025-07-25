# DLS Project Agent  

В данном репозитории представлены 4 папки с файлами:  

**1)	Baseline.**  
В папке представлены файлы с ноутбуками **базового решения** и эксель файлы с ответами моделей.  
Base_1_logreg_deepvk – логистическая регрессия на эмбеддингах.  
V3_LLM_Base_start_prompt – классификация через LLM deepseek-chat-v3 на стартовом промпте.  
gpt4_1mini_LLM_Base_start_prompt - классификация через LLM gpt-4.1mini на стартовом промпте.  

**2) Best LLM Classification.**  
Тут в отдельную категорию вынесены решения моделей на финальном промпте  для классификации через LLM. Это позволит оценить разницу в решении агента с поиском и LLM без поиска, когда для классификации используется один и тот же промпт. 
В папке представлены ноутбуки и эксель файлы с ответами моделей: deepseek-chat-v3, gpt-4.1-mini, gpt-4.1.  

**3) Final_Agent.**  
**Основная папка с финальной версией агента.**  
Главный файл – агент на gpt-4.1 и его решение в эксель.  
Дополнительный (для сравнения) - агент на deepseek-chat-v3 и его решение.  

**4) Some_Agent.**  
Тут представлен «какой-то агент» с возможностью обращаться к поисковой строке. Также в папке валидационная подвыборка и решение агента на ней.  

**Концепция и логика реализации агента**  
Ознакомиться с концепцией и логикой реализации агента по данному проекту можно в файле - **Final_Agent/gpt_4_1_final_agent.ipynb**

**Основные итоги работы**  
В таблице ниже представлены значения метрики (accuracy), полученные в ходе работы над проектом.  

| Категория/Модель        | deepseek-v3 | gpt-4.1-mini | gpt-4.1 | Log Reg |
|-------------------------|-------------|--------------|---------|---------|
| Baseline                | 0.69        | **0.76**     | —       | 0.67    |
| Best LLM Classification | 0.77        | 0.77         | **0.78**| —       |
| Final_Agent             | 0.73        | —            | **0.77**| —       |  

**Выводы**  

**Когда агент с выходом в интернет НЕ нужен.**
В случае «идеально полной» и статической базы данных с задачей определения релевантности справится «стандартный подход» с LLM на качественном промпте и агентность в таком случае не нужна.  

**Когда агент с выходом в интернет нужен.**
Когда база данных по организациям не полная и происходят периодические изменения в списке организаций и предоставляемых услуг/товаров и т.п.
На основе тестовой выборки известно, что агент делает уточняющие запросы примерно в 30% случаев. Из этих 30% часть организаций **подтвердит** свою (не)/релевантность, а по другой части **произойдет изменение** статуса (1/0). Так вот этот процент изменивших статус и будет той самой долей  метрики качества, за которую имело бы смысл бороться с помощью агента с выходом в интернет.  

**Ключевые факторы для успешной реализации проекта как такового:**  
- идеальная, «рафинированная» разметка данных, чтобы иметь возможность отследить изменение качества работы агента с поиском, относительно LLM классификатора без поиска.

**Ключевые факторы для успешной работы агента:**  
- инструменты поиска – как агрегирующие инфо по запросу, так и выполняющие парсинг сайтов из топ выдачи.
- модель LLM способная четко следовать инструкциям.
- качественный промпт инжиниринг. 
