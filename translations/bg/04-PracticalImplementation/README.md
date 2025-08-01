<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "bb1ab5c924f58cf75ef1732d474f008a",
  "translation_date": "2025-07-14T17:23:46+00:00",
  "source_file": "04-PracticalImplementation/README.md",
  "language_code": "bg"
}
-->
# Практическа реализация

Практическата реализация е моментът, в който силата на Model Context Protocol (MCP) става осезаема. Докато разбирането на теорията и архитектурата зад MCP е важно, истинската стойност се проявява, когато приложите тези концепции, за да изградите, тествате и внедрите решения, които решават реални проблеми. Тази глава преодолява пропастта между концептуалните знания и практическата разработка, като ви води през процеса на създаване на приложения, базирани на MCP.

Независимо дали разработвате интелигентни асистенти, интегрирате AI в бизнес процеси или създавате персонализирани инструменти за обработка на данни, MCP предоставя гъвкава основа. Неговият езиконезависим дизайн и официалните SDK-та за популярни програмни езици го правят достъпен за широк кръг разработчици. Използвайки тези SDK-та, можете бързо да прототипирате, итеративно да подобрявате и мащабирате решенията си в различни платформи и среди.

В следващите секции ще намерите практически примери, примерен код и стратегии за внедряване, които показват как да реализирате MCP на C#, Java, TypeScript, JavaScript и Python. Ще научите също как да отстранявате грешки и тествате MCP сървъри, да управлявате API-та и да внедрявате решения в облака чрез Azure. Тези практически ресурси са създадени, за да ускорят вашето обучение и да ви помогнат уверено да изграждате стабилни, готови за продукция MCP приложения.

## Преглед

Този урок се фокусира върху практическите аспекти на реализацията на MCP на няколко програмни езика. Ще разгледаме как да използваме MCP SDK-та на C#, Java, TypeScript, JavaScript и Python за изграждане на стабилни приложения, отстраняване на грешки и тестване на MCP сървъри, както и създаване на повторно използваеми ресурси, подсказки и инструменти.

## Цели на обучението

Към края на този урок ще можете да:
- Реализирате MCP решения, използвайки официалните SDK-та на различни програмни езици
- Систематично да отстранявате грешки и тествате MCP сървъри
- Създавате и използвате функции на сървъра (Resources, Prompts и Tools)
- Проектирате ефективни MCP работни потоци за сложни задачи
- Оптимизирате MCP реализации за производителност и надеждност

## Официални SDK ресурси

Model Context Protocol предлага официални SDK-та за няколко езика:

- [C# SDK](https://github.com/modelcontextprotocol/csharp-sdk)
- [Java SDK](https://github.com/modelcontextprotocol/java-sdk) 
- [TypeScript SDK](https://github.com/modelcontextprotocol/typescript-sdk)
- [Python SDK](https://github.com/modelcontextprotocol/python-sdk)
- [Kotlin SDK](https://github.com/modelcontextprotocol/kotlin-sdk)

## Работа с MCP SDK-та

Тази секция предоставя практически примери за реализиране на MCP на няколко програмни езика. Можете да намерите примерен код в директорията `samples`, организиран по езици.

### Налични примери

Репозиторият включва [примерни реализации](../../../04-PracticalImplementation/samples) на следните езици:

- [C#](./samples/csharp/README.md)
- [Java](./samples/java/containerapp/README.md)
- [TypeScript](./samples/typescript/README.md)
- [JavaScript](./samples/javascript/README.md)
- [Python](./samples/python/README.md)

Всеки пример демонстрира ключови концепции и модели за реализация на MCP за съответния език и екосистема.

## Основни функции на сървъра

MCP сървърите могат да реализират всяка комбинация от следните функции:

### Resources  
Resources предоставят контекст и данни за потребителя или AI модела:
- Хранилища с документи
- Бази знания
- Структурирани източници на данни
- Файлови системи

### Prompts  
Prompts са шаблонирани съобщения и работни потоци за потребителите:
- Предефинирани шаблони за разговори
- Водени модели на взаимодействие
- Специализирани диалогови структури

### Tools  
Tools са функции, които AI моделът може да изпълнява:
- Утилити за обработка на данни
- Интеграции с външни API-та
- Изчислителни възможности
- Функции за търсене

## Примерни реализации: C#

Официалният репозиторий на C# SDK съдържа няколко примерни реализации, демонстриращи различни аспекти на MCP:

- **Basic MCP Client**: Прост пример, показващ как да се създаде MCP клиент и да се извикват инструменти
- **Basic MCP Server**: Минимална реализация на сървър с основна регистрация на инструменти
- **Advanced MCP Server**: Пълнофункционален сървър с регистрация на инструменти, автентикация и обработка на грешки
- **ASP.NET Integration**: Примери за интеграция с ASP.NET Core
- **Tool Implementation Patterns**: Различни модели за реализиране на инструменти с различна степен на сложност

MCP C# SDK е в предварителна версия и API-тата могат да се променят. Ще обновяваме този блог постоянно с развитието на SDK-то.

### Ключови функции  
- [C# MCP Nuget ModelContextProtocol](https://www.nuget.org/packages/ModelContextProtocol)

- Изграждане на [първия ви MCP сървър](https://devblogs.microsoft.com/dotnet/build-a-model-context-protocol-mcp-server-in-csharp/).

За пълни примерни реализации на C# посетете [официалния репозиторий с примери за C# SDK](https://github.com/modelcontextprotocol/csharp-sdk)

## Примерна реализация: Java

Java SDK предлага стабилни възможности за реализация на MCP с функции на корпоративно ниво.

### Ключови функции

- Интеграция със Spring Framework
- Силна типова безопасност
- Поддръжка на реактивно програмиране
- Обширна обработка на грешки

За пълен пример на Java реализация вижте [Java пример](samples/java/containerapp/README.md) в директорията с примери.

## Примерна реализация: JavaScript

JavaScript SDK предоставя лек и гъвкав подход за реализация на MCP.

### Ключови функции

- Поддръжка за Node.js и браузъри
- API базиран на Promise
- Лесна интеграция с Express и други рамки
- Поддръжка на WebSocket за стрийминг

За пълен пример на JavaScript реализация вижте [JavaScript пример](samples/javascript/README.md) в директорията с примери.

## Примерна реализация: Python

Python SDK предлага Python-ориентиран подход за реализация на MCP с отлична интеграция с ML рамки.

### Ключови функции

- Поддръжка на async/await с asyncio
- Интеграция с FastAPI
- Лесна регистрация на инструменти
- Натурална интеграция с популярни ML библиотеки

За пълен пример на Python реализация вижте [Python пример](samples/python/README.md) в директорията с примери.

## Управление на API

Azure API Management е отлично решение за осигуряване на MCP сървъри. Идеята е да поставите Azure API Management пред вашия MCP сървър и да му позволите да управлява функции, които вероятно ще искате, като:

- ограничаване на честотата на заявките
- управление на токени
- мониторинг
- балансиране на натоварването
- сигурност

### Azure пример

Ето един Azure пример, който прави точно това, т.е. [създаване на MCP сървър и осигуряването му чрез Azure API Management](https://github.com/Azure-Samples/remote-mcp-apim-functions-python).

Вижте как протича процесът на авторизация на следната снимка:

![APIM-MCP](https://github.com/Azure-Samples/remote-mcp-apim-functions-python/blob/main/mcp-client-authorization.gif?raw=true) 

На горната снимка се случва следното:

- Аутентикация/Авторизация се извършва чрез Microsoft Entra.
- Azure API Management действа като шлюз и използва политики за насочване и управление на трафика.
- Azure Monitor записва всички заявки за по-нататъшен анализ.

#### Поток на авторизация

Нека разгледаме по-подробно потока на авторизация:

![Sequence Diagram](https://github.com/Azure-Samples/remote-mcp-apim-functions-python/blob/main/infra/app/apim-oauth/diagrams/images/mcp-client-auth.png?raw=true)

#### Спецификация за MCP авторизация

Научете повече за [MCP Authorization specification](https://modelcontextprotocol.io/specification/2025-03-26/basic/authorization#2-10-third-party-authorization-flow)

## Внедряване на отдалечен MCP сървър в Azure

Нека видим дали можем да внедрим примера, който споменахме по-рано:

1. Клонирайте репозиторито

    ```bash
    git clone https://github.com/Azure-Samples/remote-mcp-apim-functions-python.git
    cd remote-mcp-apim-functions-python
    ```

1. Регистрирайте ресурсния доставчик `Microsoft.App`.
    * Ако използвате Azure CLI, изпълнете `az provider register --namespace Microsoft.App --wait`.
    * Ако използвате Azure PowerShell, изпълнете `Register-AzResourceProvider -ProviderNamespace Microsoft.App`. След това проверете състоянието на регистрацията с `(Get-AzResourceProvider -ProviderNamespace Microsoft.App).RegistrationState` след известно време.

2. Изпълнете тази [azd](https://aka.ms/azd) команда, за да осигурите услугата за управление на API, функцията (с код) и всички други необходими Azure ресурси

    ```shell
    azd up
    ```

    Тази команда трябва да внедри всички облачни ресурси в Azure

### Тестване на сървъра с MCP Inspector

1. В **нов прозорец на терминала** инсталирайте и стартирайте MCP Inspector

    ```shell
    npx @modelcontextprotocol/inspector
    ```

    Трябва да видите интерфейс, подобен на:

    ![Connect to Node inspector](/03-GettingStarted/01-first-server/assets/connect.png) 

1. Натиснете CTRL и кликнете, за да заредите уеб приложението MCP Inspector от URL адреса, показан от приложението (например http://127.0.0.1:6274/#resources)
1. Задайте типа на транспорта на `SSE`
1. Задайте URL адреса към вашия работещ API Management SSE крайна точка, показана след `azd up` и натиснете **Connect**:

    ```shell
    https://<apim-servicename-from-azd-output>.azure-api.net/mcp/sse
    ```

5. **Списък с инструменти**. Кликнете върху инструмент и изберете **Run Tool**.  

Ако всички стъпки са успешни, вече сте свързани със MCP сървъра и сте успели да извикате инструмент.

## MCP сървъри за Azure

[Remote-mcp-functions](https://github.com/Azure-Samples/remote-mcp-functions-dotnet): Този набор от репозитории е шаблон за бърз старт за изграждане и внедряване на персонализирани отдалечени MCP (Model Context Protocol) сървъри, използвайки Azure Functions с Python, C# .NET или Node/TypeScript.

Примерите предоставят пълно решение, което позволява на разработчиците да:

- Изграждат и стартират локално: Разработват и отстраняват грешки на MCP сървър на локална машина
- Внедряват в Azure: Лесно внедряване в облака с проста команда azd up
- Свързват се от клиенти: Свързване към MCP сървъра от различни клиенти, включително режим Copilot на VS Code и инструмента MCP Inspector

### Ключови функции:

- Сигурност по дизайн: MCP сървърът е защитен чрез ключове и HTTPS
- Опции за автентикация: Поддържа OAuth чрез вградена автентикация и/или API Management
- Мрежова изолация: Позволява мрежова изолация чрез Azure Virtual Networks (VNET)
- Безсървърна архитектура: Използва Azure Functions за мащабируемо, събитийно базирано изпълнение
- Локална разработка: Пълна поддръжка за локална разработка и отстраняване на грешки
- Лесно внедряване: Оптимизиран процес за внедряване в Azure

Репозиторият включва всички необходими конфигурационни файлове, изходен код и инфраструктурни дефиниции, за да започнете бързо с продукционно готова реализация на MCP сървър.

- [Azure Remote MCP Functions Python](https://github.com/Azure-Samples/remote-mcp-functions-python) - Примерна реализация на MCP с Azure Functions и Python

- [Azure Remote MCP Functions .NET](https://github.com/Azure-Samples/remote-mcp-functions-dotnet) - Примерна реализация на MCP с Azure Functions и C# .NET

- [Azure Remote MCP Functions Node/Typescript](https://github.com/Azure-Samples/remote-mcp-functions-typescript) - Примерна реализация на MCP с Azure Functions и Node/TypeScript.

## Основни изводи

- MCP SDK-тата предоставят езиково-специфични инструменти за реализиране на стабилни MCP решения
- Процесът на отстраняване на грешки и тестване е критичен за надеждни MCP приложения
- Повторно използваемите шаблони за подсказки осигуряват последователни AI взаимодействия
- Добре проектираните работни потоци могат да оркестрират сложни задачи с помощта на множество инструменти
- Реализацията на MCP решения изисква внимание към сигурността, производителността и обработката на грешки

## Упражнение

Проектирайте практичен MCP работен поток, който решава реален проблем във вашата област:

1. Идентифицирайте 3-4 инструмента, които биха били полезни за решаването на този проблем
2. Създайте диаграма на работния поток, показваща как тези инструменти взаимодействат
3. Реализирайте базова версия на един от инструментите, използвайки предпочитания от вас език
4. Създайте шаблон за подсказка, който да помогне на модела ефективно да използва вашия инструмент

## Допълнителни ресурси


---

Следва: [Разширени теми](../05-AdvancedTopics/README.md)

**Отказ от отговорност**:  
Този документ е преведен с помощта на AI преводаческа услуга [Co-op Translator](https://github.com/Azure/co-op-translator). Въпреки че се стремим към точност, моля, имайте предвид, че автоматизираните преводи могат да съдържат грешки или неточности. Оригиналният документ на неговия роден език трябва да се счита за авторитетен източник. За критична информация се препоръчва професионален човешки превод. Ние не носим отговорност за каквито и да е недоразумения или неправилни тълкувания, произтичащи от използването на този превод.