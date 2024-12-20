---
title: Логування компонентів
description: Описує, як використовувати логування на рівні компонентів для отримання інформації про поведінку працюючих компонентів.
weight: 70
keywords: [ops]
aliases:
  - /uk/help/ops/component-logging
  - /uk/docs/ops/troubleshooting/component-logging
owner: istio/wg-user-experience-maintainers
test: no
---

Компоненти Istio побудовані з використанням гнучкої системи логування, яка надає ряд функцій та елементів керування для допомоги в експлуатації цих компонентів і полегшення діагностики. Ви контролюєте ці функції логування, передаючи параметри командного рядка при запуску компонентів.

## Сфери дії логування {#logging-scopes}

Повідомлення логування, що виводяться компонентом, класифікуються за *сферами дії*. Сфера становить собою набір повʼязаних лог-повідомлень, які ви можете контролювати як єдине ціле. Різні компоненти мають різні сфери, залежно від функціональних можливостей компонента. Всі компоненти мають сферу дії `default`, яка використовується для некласифікованих лог-повідомлень.

Наприклад, на момент написання `istioctl` має 25 сфер, які представляють різні функціональні області в межах команди:

- `ads`, `adsc`, `all`, `analysis`, `authn`, `authorization`, `ca`, `cache`, `cli`, `default`, `installer`, `klog`, `mcp`, `model`, `patch`, `processing`, `resource`, `source`, `spiffe`, `tpath`, `translator`, `util`, `validation`, `validationController`, `wle`

Pilot-Agent, Pilot-Discovery та Istio Operator мають свої власні сфери дії, які ви можете дізнатися, переглянувши їх [документацію](/docs/reference/commands/).

Кожена сфера дії має унікальний рівень виводу, який є одним з:

1. none
1. error
1. warn
1. info
1. debug

де `none` не виробляє виходу для сфери, а `debug` продукує максимальну кількість виводу. Стандартне значення для всіх сфер дії — `info`, яке призначене для надання необхідної кількості інформації про логування для експлуатації Istio в нормальних умовах.

Щоб контролювати рівень виводу, ви використовуєте параметр командного рядка `--log_output_level`. Наприклад:

{{< text bash >}}
$ istioctl analyze --log_output_level klog:none,cli:info
{{< /text >}}

Окрім контролю рівня виводу з командного рядка, ви також можете контролювати рівень виводу працюючого компонента з використанням його [ControlZ](/docs/ops/diagnostic-tools/controlz) інтерфейсу.

## Контроль виводу {#controlling-output}

Повідомлення логування зазвичай надсилаються на стандартний вихідний потік компонента. Параметр `--log_target` дозволяє направляти вихід на будь-яку кількість різних місць. Ви передаєте параметру список шляхів до файлової системи, розділений комами, разом з спеціальними значеннями `stdout` і `stderr` для позначення стандартного виходу та стандартного потоку помилок відповідно.

Повідомлення логування зазвичай виводяться у зручному для людини форматі. Параметр `--log_as_json` можна використовувати для примусового виводу у формат JSON, що може бути зручніше для обробки інструментами.

## Ротація логів {#log-rotation}

Компоненти панелі управління Istio можуть автоматично керувати ротацією логів, що спрощує розподіл великих логів на менші файли. Параметр `--log_rotate` дозволяє вказати базову назву файлу для ротації. Похідні назви будуть використовуватися для окремих лог-файлів.

Параметр `--log_rotate_max_age` дозволяє вказати максимальну кількість днів до початку ротації файлів, а параметр `--log_rotate_max_size` дозволяє вказати максимальний розмір у мегабайтах до початку ротації файлів. Нарешті, параметр `--log_rotate_max_backups` дозволяє контролювати максимальну кількість ротованих файлів для збереження; старі файли будуть автоматично видалені.

## Налагодження компонентів {#component-debugging}

Параметри `--log_caller` та `--log_stacktrace_level` дозволяють контролювати, чи включає інформація про логування інформацію для розробників. Це корисно при спробі виявлення помилок у компоненті, але зазвичай не використовується в щоденній експлуатації.
