# Критерії приймання

- Створено репозиторій `goit-advancedjs-hw-07`.
- Домашня робота містить посилання на робочу сторінку на `GitHub Pages`.
- До роботи прiкрiплено файл репозиторiю у форматi `zip`.
- Виконано всі завдання згідно з описом в файлi ReadMe репозиторію.
- У папці src розташовані файли, з якими проводилася робота.

#### Формат оцінювання:

Оцінка від 0 до 100

#### Формат здачi:

- Посилання на робочу сторінку на `GitHub Pages`
- Прикрiплений файл репозиторію у форматi `zip`

# Завдання

У межах цієї домашньої роботи ти матимеш чотири завдання, які допоможуть тобі
систематизувати та закріпити здобуті знання й навички.

А саме:

- написати скорочену ініціалізацію;
- створити класи з певними властивостями та налаштуваннями;
- визначити інтерфейси гри так, щоб вони відповідали вимогам класу;
- реалізувати сценарій життя, де людина, ключ і будинок взаємодіють один з
  одним.

Подробиці для кожного завдання ти знайдеш в описі ReadMe репозиторію.

#### Стартові файли

Для виконання домашнього завдання тобі буде необхідно створити репозиторій,
використовуючи наданий шаблон, де розписане твоє домашнє завдання:

- В ReadMe репозиторію розписано покроково, що саме треба зробити.
- У папці src розташовані файли, з якими треба працювати.

#### Коротка інструкція

1. Перейди на сторінку шаблону проєкту на GitHub -
   ([посилання на репозиторій](https://github.com/goitacademy/typescript-homework-module-3)).
2. Натисни кнопку "Use this template".
3. Введи назву для твого репозиторію, який буде створено на основі існуючого
   шаблону - `goit-advancedjs-hw-07`.
4. Обери власні налаштування видимості (Public) для нового репозиторію.
5. Натисни кнопку "Create repository from template".
6. GitHub створить новий репозиторій на основі обраного шаблону. Ти будеш
   автоматично перенаправлений на сторінку нового репозиторію.
7. Уважно ознайомся з ReadMe репозиторію, де детально розписані умови завдань і
   переходь до їх виконання у папці src.

# Завдання 1

У вас є компонент React, який використовує useRef та IntersectionObserver для
визначення, коли користувач переглядає кінець вмісту. Ваше завдання полягає в
наступному:

Встановіть правильні типи пропсів для цього компонента. У ньому є дві
властивості: children і onContentEndVisible. children - це будь-який валідний
React вузол, а onContentEndVisible - це функція без аргументів, що повертає
void.

Встановіть правильний тип useRef. Посилання endContentRef використовується для
div, який міститься в кінці вмісту.

Встановіть правильний тип для options (клас також може бути типом для options).

```ts
import React, { useEffect, useRef } from 'react';

// Опишіть Props
export function Observer({ children, onContentEndVisible }: Props) {
  // Вкажіть правильний тип для useRef зверніть увагу, в який DOM елемент ми його передаємо
  const endContentRef = useRef(null);

  useEffect(() => {
    // Вкажіть правильний тип для options, підказка, клас також можна вказувати як тип
    const options = {
      rootMargin: '0px',
      threshold: 1.0,
      root: null,
    };

    const observer = new IntersectionObserver(entries => {
      entries.forEach(entry => {
        if (entry.intersectionRatio > 0) {
          onContentEndVisible();
          observer.disconnect();
        }
      });
    }, options);

    if (endContentRef.current) {
      observer.observe(endContentRef.current);
    }

    return () => {
      observer.disconnect();
    };
  }, [onContentEndVisible]);

  return (
    <div>
      {children}
      <div ref={endContentRef} />
    </div>
  );
}
```

# Завдання 2

Ваше завдання – додати типи для наступних елементів коду:

RequestStep: Це рядковий літерал.

State: Цей тип являє собою об'єкт з двома властивостями isRequestInProgress і
RequestStep

Action: Це тип, що представляє можливі дії, які можуть бути відправлені до
редюсера.

Дивіться код і опишіть для нього правильні типи.

```ts
import React, { useReducer } from 'react';

const initialState: State = {
  isRequestInProgress: false,
  requestStep: 'idle',
};

function requestReducer(state: State, action: Action): State {
  switch (action.type) {
    case 'START_REQUEST':
      return { ...state, isRequestInProgress: true, requestStep: 'start' };
    case 'PENDING_REQUEST':
      return { ...state, isRequestInProgress: true, requestStep: 'pending' };
    case 'FINISH_REQUEST':
      return { ...state, isRequestInProgress: false, requestStep: 'finished' };
    case 'RESET_REQUEST':
      return { ...state, isRequestInProgress: false, requestStep: 'idle' };
    default:
      return state;
  }
}

export function RequestComponent() {
  const [requestState, requestDispatch] = useReducer(
    requestReducer,
    initialState,
  );

  const startRequest = () => {
    requestDispatch({ type: 'START_REQUEST' });
    // Імітуємо запит до сервера
    setTimeout(() => {
      requestDispatch({ type: 'PENDING_REQUEST' });
      // Імітуємо отримання відповіді від сервера
      setTimeout(() => {
        requestDispatch({ type: 'FINISH_REQUEST' });
      }, 2000);
    }, 2000);
  };

  const resetRequest = () => {
    requestDispatch({ type: 'RESET_REQUEST' });
  };

  return (
    <div>
      <button onClick={startRequest}>Почати запит</button>
      <button onClick={resetRequest}>Скинути запит</button>
      <p>Стан запиту: {requestState.requestStep}</p>
    </div>
  );
}

export default RequestComponent;
```

# Завдання 3

Ви створюєте компонент форми у React. Ви маєте поле введення, в якому ви хочете
відстежити зміни. Для цього ви використовуєте обробник подій onChange. Ваше
завдання – правильно типізувати подію, яка передається у цю функцію.

```ts
import React, { useState } from 'react';

export function FormComponent() {
  const [value, setValue] = useState('');

  const handleChange = event => {
    setValue(event.target.value);
  };

  return <input type="text" value={value} onChange={handleChange} />;
}
```

# Завдання 4

Ви вирішили застосувати до меню контекст і тепер вам потрібно його типізувати.

Описати тип SelectedMenu: Це має бути об'єкт, який містить id з типом MenuIds

Описати тип MenuSelected: Цей тип є об'єктом, що містить selectedMenu

Описати тип MenuAction: Цей тип являє собою об'єкт з методом onSelectedMenu,
який приймає об'єкт типу SelectedMenu як аргумент повертає void.

Описати тип PropsProvider: Опишіть правильний тип для дітей

Описати тип PropsMenu: Опишіть тип для menus, він має бути від типу Menu
