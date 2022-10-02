## Task Tracker

1. Таск-трекер должен быть отдельным дашбордом и доступен всем сотрудникам компании UberPopug Inc.

```
Actor - Account
Command - Login to TaskDashboard
Data - authorId, date
Event - Account.LoginedToTaskDashboard
```

4. Новые таски может создавать кто угодно (администратор, начальник, разработчик, менеджер и любая другая роль). У задачи должны быть описание, статус (выполнена или нет) и рандомно выбранный попуг (кроме менеджера и администратора), на которого заассайнена задача.

```
Actor - Account
Command - Create New Task
Data - authorId, taskDesc, taskStatus, taskAssignId, taskFee, taskPayment
Event - Task.CreatedEvent
```

5. Менеджеры или администраторы должны иметь кнопку «заассайнить задачи», которая возьмёт все открытые задачи и рандомно заассайнит каждую на любого из сотрудников (кроме менеджера и администратора) . Не успел закрыть задачу до реассайна — сорян, делай следующую.

```
Actor - Account (Admin, Manager)
Command - Assign Tasks
Data - null
Event - Tasks.AssignedEvent

Actor - Tasks.AssignedEvent
Command - Create audit record - reasign unfinished task
Data - accountId, taskId
Event - Task.ReasignedEvent

Actor - Tasks.AssignedEvent
Command - Create audit record - assign task
Data - accountId, taskId, date
Event - Task.AssignedAccountEvent

Actor - Task.AssignedAccountEvent
Command - Create audit record - charge task fee from account
Data - taskFee, accountId
Event - null
```

6. Каждый сотрудник должен иметь возможность видеть в отдельном месте список заассайненных на него задач + отметить задачу выполненной.

```
Actor - Account
Command - Finish Task
Data - TaskId, AccountId
Event - Task.FinishedEvent

Actor - Task.FinishedEvent
Command - Create audit record - finish task
Data - accountId, taskId, date
Event - null

Actor - Task.FinishedEvent
Command - Add task payment to account
Data - accountId, taskPayment, date
Event - null
```

## Accounting

1. Аккаунтинг должен быть в отдельном дашборде и доступным только для администраторов и бухгалтеров.

```
Actor - Account
Command - Login to AccountingDashboard
Data - authorId, date
Event - Account.LoginedToAccountingDashboard
```

6. В конце дня необходимо:
   1. считать сколько денег сотрудник получил за рабочий день
   2. отправлять на почту сумму выплаты.

```
Actor - Cron
Command - Calculate daily account payment
Data - authorId, payment, date
Event - Account.CalculatedDailyPaymentEvent
```

```
Actor - Account.CalculatedDailyPaymentEvent
Command - Send daily account payment
Data - authorId, payment sum
Event - Account.SentDaylyPaymentEvent
```

## Analyitics

1. Аналитика — это отдельный дашборд, доступный только админам.

```
Actor - Account
Command - Login to AnalyiticsDashboard
Data - authorId, date
Event - Account.LoginedToAnalyticsDashboard
```
