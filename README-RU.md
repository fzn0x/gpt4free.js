# GPT4js 🔮

GPT4js - это пакет, который упрощает взаимодействие с различными моделями искусственного интеллекта, исключая необходимость в ключе API или других методах авторизации для доступа к моделям завершения чата и генерации изображений.

Этот пакет можно использовать в среде Node.js или в браузере.

![Static Badge](https://img.shields.io/badge/Zachey-GPT4js-GPT4js)
![GitHub top language](https://img.shields.io/github/languages/top/zachey01/gpt4free.js)
![GitHub Repo stars](https://img.shields.io/github/stars/zachey01/gpt4free.js)
![GitHub issues](https://img.shields.io/github/issues/zachey01/gpt4free.js)
![NPM Downloads](https://img.shields.io/npm/dm/gpt4js)

## 📚 Содержание

- [🛠️ Установка](#installation)
  - [Используя NPM](#using-npm)
  - [Используя Yarn](#using-yarn)
  - [Используя Bun](#using-bun)
  - [Используя CDN](#using-cdn)
- [🧩 Примеры](#examples)
  - [📤 Завершение Чата](#chat-completion)
    - [⚙️ Основное Использование](#basic-usage)
      - [Простой Запрос](#simple-fetch)
      - [Укажите Ваши Инструкции](#give-your-instructions)
      - [Роли в Разговоре](#conversation-roles)
    - [🔩 Настройки](#configurable-options)
    - [🚀 Провайдеры Завершения Чата](#chat-completion-providers)
    - [📚 Модели Завершения Чата](#chat-completion-models)
  - [📷 Генерация Изображений (BETA)](#image-generation)
    - [🌐 Опции Провайдера Генерации Изображений](#image-generation-provider-options)
    - [🧮 Опции Типа Чисел](#number-type-options)
    - [🖼️ Провайдеры Генерации Изображений](#image-generation-providers)
- [🧠 Искусственный Интеллект Google Chrome](#google-chrome-ai)
  - [Настройка Браузера](#setting-browser)
  - [Простое Использование](#simple-usage)
- [⌨️ CLI](#cli)
- [🧪 Тестирование](#testing)
- [🚧 Сборка](#building)
  - [Webpack](#webpack)
  - [Bun](#bun)
- [🤝 Сотрудничество](#contribute)

<a id="installation"></a>

## 🛠️ Установка

<a id="using-npm"></a>

### Используя NPM

```sh
npm install gpt4js
```

<a id="using-yarn"></a>

### Используя Yarn

```sh
yarn add gpt4js
```

<a id="using-bun"></a>

### Используя Bun

```sh
bun add gpt4js
```

<a id="using-cdn"></a>

### Используя CDN

```html
<script src="https://cdn.jsdelivr.net/npm/gpt4js/dist/gpt4js.min.js"></script>
```

<a id="examples"></a>

# 🧩 Примеры

<a id="chat-completion"></a>

## 📤 Завершение Чата

С функцией `chatCompletion` вы можете получать текстовые ответы на разговор с учетом контекста, используя провайдеры и модели, предназначенные для этой задачи. Кроме того, вы можете модифицировать ответ перед тем, как преобразовать его в поток или заставить ИИ дать вам определенный ответ, создав несколько попыток.

<a id="basic-usage"></a>

### ⚙️ Основное Использование

<a id="simple-fetch"></a>

#### Простой Запрос

Он захватывает сообщения и контекст, и любой провайдер отвечает строкой.

```js
// CommonJS
const getGPT4js = require("gpt4js");
const GPT4js = await getGPT4js();
// ESM
import GPT4js from "gpt4js";
// В браузерах используйте тег <script>

const messages = [{ role: "user", content: "hi!" }];
const options = {
  provider: "Nextway",
  model: "gpt-4o-free",
};

(async () => {
  const provider = GPT4js.createProvider(options.provider);
  try {
    const text = await provider.chatCompletion(messages, options);
    console.log(text);
  } catch (error) {
    console.error("Ошибка:", error);
  }
})();
```

**Примечание:** Для корректного ответа в разговоре необходимо включить хотя бы одно сообщение с ролью **user**.

<a id="give-your-instructions"></a>

#### Укажите Ваши Инструкции

Вы можете предоставить свои инструкции для разговора перед его началом, используя роль **system**.

```js
const messages = [
  { role: "system", content: "Вы экспертный бот в программировании." },
  { role: "user", content: "Привет, напиши мне что-нибудь." },
];
const options = {
  provider: "Nextway",
  model: "gpt-4o-free",
};

(async () => {
  const provider = GPT4js.createProvider(options.provider);
  try {
    const text = await provider.chatCompletion(messages, options);
    console.log(text);
  } catch (error) {
    console.error("Ошибка:", error);
  }
})();
```

<a id="conversation-roles"></a>

#### Роли в Разговоре

| Роль        | Описание                                                                 |
| ----------- | ------------------------------------------------------------------------ |
| `system`    | Используется для предоставления инструкций и контекста перед разговором. |
| `user`      | Используется для идентификации сообщений пользователя.                   |
| `assistant` | Используется для идентификации сообщений ИИ.                             |

<a id="configurable-options"></a>

### 🔩 Настройки

| Опция           | Тип     | Описание                                                                                                                                                                  |
| --------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `provider`      | string  | Выберите провайдера для завершения чата. Возможные значения: `Nextway`, `BlackBox` и др. Определяет сервис, который будет обрабатывать запрос.                            |
| `model`         | string  | Выберите модель для использования провайдером, поддерживающим эту модель. Например, `gpt-4`, `gpt-3.5-turbo` и др. Определяет конкретную модель для генерации завершений. |
| `stream`        | boolean | Определяет, должны ли данные передаваться порциями или нет. Если `true`, ответ будет передаваться в режиме реального времени по мере генерации.                           |
| `temperature`   | number  | Устанавливает температуру для контроля случайности вывода. Значение от 0 до 1, где более высокие значения (ближе к 1) делают вывод более случайным.                       |
| `webSearch`     | boolean | Включает или отключает функционал веб-поиска. Если `true`, система может выполнять веб-поиски для получения информации в реальном времени.                                |
| `codeModelMode` | boolean | Включает или отключает режим модели для кода. Если `true`, система будет использовать модель, оптимизированную для понимания и генерации кода.                            |
| `isChromeExt`   | boolean | Указывает, используется ли система как расширение Chrome. Если `true`, это указывает на интеграцию с Chrome, что может повлиять на определенные функции и разрешения.     |

<a id="chat-completion-providers"></a>

### 🚀 Провайдеры Завершения Чата

| Веб-сайт                                | Провайдер  | GPT-3.5 | GPT-4 | Поток | Статус                                                     |
| --------------------------------------- | ---------- | ------- | ----- | ----- | ---------------------------------------------------------- |
| [Aryahcr](https://nexra.aryahcr.cc)     | `Aryahcr`  | ✔️      | ✔️    | ✔️    | ![Active](https://img.shields.io/badge/Active-brightgreen) |
| [BlackBox](https://www.blackbox.ai)     | `BlackBox` | ❌      | ❌    | ❌    | ![Active](https://img.shields.io/badge/Active-brightgreen) |
| [Nextway](https://chat.eqing.tech)      | `Nextway`  | ✔️      | ✔️    | ✔️    | ![Active](https://img.shields.io/badge/Active-brightgreen) |
| [Chrome](https://www.google.ru/chrome/) | `Chrome`   | ❌      | ❌    | ❌    | ![Active](https://img.shields.io/badge/Active-brightgreen) |

<a id="chat-completion-models"></a>

### 📚 Модели Завершения Чата

| Модель                 | Провайдеры, поддерживающие её |
| ---------------------- | ----------------------------- |
| gpt-4                  | `Aryahcr`, `Nextway`          |
| gpt-4-0613             | `Aryahcr`                     |
| gpt-4-32k              | `Aryahcr`                     |
| gpt-4-0314             | `Aryahcr`                     |
| gpt-4-32k-0314         | `Aryahcr`                     |
| gpt-3.5-turbo          | `Aryahcr`, `Nextway`          |
| gpt-3.5-turbo-16k      | `Aryahcr`                     |
| gpt-3.5-turbo-0613     | `Aryahcr`                     |
| gpt-3.5-turbo-16k-0613 | `Aryahcr`                     |
| gpt-3.5-turbo-0301     | `Aryahcr`                     |
| text-davinci-003       | `Aryahcr`                     |
| text-davinci-002       | `Aryahcr`                     |
| code-davinci-002       | `Aryahcr`                     |
| gpt-3                  | `Aryahcr`                     |
| text-curie-001         | `Aryahcr`                     |
| text-babbage-001       | `Aryahcr`                     |
| text-ada-001           | `Aryahcr`                     |
| davinci                | `Aryahcr`                     |
| curie                  | `Aryahcr`                     |
| babbage                | `Aryahcr`                     |
| ada                    | `Aryahcr`                     |
| babbage-002            | `Aryahcr`                     |
| davinci-002            | `Aryahcr`                     |
| gemini-pro             | `Nextway`                     |
| gpt-4o-free            | `Nextway`                     |
| gemini-nano            | `Chrome`                      |
| Все модели Ollama      | `Ollama`                      |

<a id="image-generation"></a>

# 📷 Генерация Изображений (BETA)

С функцией `imageGeneration` вы можете генерировать изображения на основе текстового ввода с возможностью настройки параметров для кастомизации и стилизации изображений в различных художественных стилях.

<a id="image-generation-provider-options"></a>

## 🌐 Опции Провайдера Генерации Изображений

| Опция            | Тип    | Описание                                                                                                                                    |
| ---------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `negativePrompt` | string | Указывает направление, которого не следует принимать в производстве.                                                                        |
| `height`         | number | Указывает высоту изображения.                                                                                                               |
| `width`          | number | Указывает ширину изображения.                                                                                                               |
| `samplingSteps`  | number | Указывает количество итераций. Чем больше число, тем выше качество.                                                                         |
| `samplingMethod` | string | Выбирает метод выборки для контроля разнообразия, качества и согласованности изображений.                                                   |
| `cfgScale`       | number | Устанавливает Classifier-Free Guidance для контроля того, насколько сгенерированное изображение соответствует заданному текстовому запросу. |

<a id="number-type-options"></a>

## 🧮 Опции Типа Чисел

| Провайдер         | Поддерживаемые Опции Типа Чисел и Значения                                                                                                                                                              |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `StableDiffusion` | - `height`: По умолчанию 512, Мин 50, Макс 1024<br>- `width`: По умолчанию 512, Мин 50, Макс 1024<br>- `samplingSteps`: По умолчанию 25, Мин 1, Макс 30<br>- `cfgScale`: По умолчанию 7, Мин 1, Макс 20 |

<a id="image-generation-providers"></a>

## 🖼️ Провайдеры Генерации Изображений

| Провайдер         | Статус                                                     | Стиль По Умолчанию                                                                      |
| ----------------- | ---------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| `Dalle2`          | ![Active](https://img.shields.io/badge/Active-brightgreen) | Полу-реалистичный, детализированный с яркими цветами и естественным освещением.         |
| `StableDiffusion` | ![Active](https://img.shields.io/badge/Active-brightgreen) | Фотореалистичный, передающий мелкие детали и текстуры для имитации сцен реальной жизни. |

<a id="google-chrome-ai"></a>

## 🧠 ИИ встроенный в Google Chrome

> Предупреждение: Это экспериментальная функция и может работать некорректно, она работает только в Google Chrome 127 или выше (Chrome Dev). История также не поддерживается.

### Настройка Браузера

1. [chrome://flags/#prompt-api-for-gemini-nano](chrome://flags/#optimization-guide-on-device-model) Выберите 'Enabled'

2. [chrome://flags/#optimization-guide-on-device-model](chrome://flags/#optimization-guide-on-device-model) Выберите 'Enabled BypassPrefRequirement'

3. [chrome://components](chrome://flags/#optimization-guide-on-device-model) Нажмите 'Check for Update' на Optimization Guide On Device Model для загрузки модели. Если вы не видите Optimization Guide, убедитесь, что правильно установлены флаги выше, перезапустите браузер и обновите страницу.

#### Простое Использование

```js
const messages = [{ role: "user", content: "hi!" }];
const options = {
  provider: "Chrome",
};

(async () => {
  const provider = GPT4js.createProvider(options.provider);
  try {
    const text = await provider.chatCompletion;
  } catch (error) {
    console.error("Ошибка:", error);
  }
})();
```

<a id="cli"></a>

# ⌨️ CLI

Запуск GUI: `npx gpt4js --gui --port <port>`

![output-onlinetools](https://github.com/zachey01/gpt4free.js/assets/63107653/daa66f93-e32c-4bc7-8157-b4e41815ad5d)

Использование: `npx gpt4js --model <model> --provider <provider> --prompt <prompt>`

<a id="testing"></a>

# 🧪 Тестирование

Запуск: `npm test`

<a id="building"></a>

# 🚧 Сборка

<a id="webpack"></a>

## Webpack

- `npm run build` - Сборка с использованием Webpack.
- `npm run dev` - Сборка webpack с hotreload

<a id="bun"></a>

## Bun

- `npm run build:bun` - Сборка с использованием Bun.
- `npm run dev:bun` - Сборка Bun с hotreload

<a id="contribute"></a>

# 🤝 Вклад

Если вы хотите внести свой вклад в этот проект, вы можете сделать это непосредственно на [GitHub](https://github.com/zachey01/gpt4free.js/g4f-ts). Кроме того, если вы столкнулись с ошибками, мешающими использованию любой функциональности проекта, пожалуйста, [сообщите о них здесь](https://github.com/zachey01/gpt4free.js/issues). Ваш отзыв помогает нашему сообществу свободно использовать инструменты ИИ!

---

<center>
  <div align="center">
    <img src="https://github.com/zachey01/gpt4free.js/assets/63107653/a605f58f-1090-4f88-b3fc-30929b410404" alt="логотип" width="300"/>
  </div>
</center>
