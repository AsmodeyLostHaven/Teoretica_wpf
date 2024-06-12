```markdown
# WPF Магазин

Этот проект представляет собой приложение магазина, разработанное с использованием WPF (Windows Presentation Foundation) и Microsoft SQL Server для управления базой данных. Приложение включает в себя функционал регистрации и входа пользователей, список товаров, профиль пользователя и стилизацию интерфейса.

## Описание

Приложение позволяет пользователям регистрироваться и входить в систему, просматривать список товаров и управлять своим профилем. В приложении используется ADO.NET для взаимодействия с базой данных Microsoft SQL Server. Пароли пользователей хешируются с использованием SHA256 для обеспечения безопасности.

## Структура проекта

- `App.xaml` и `App.xaml.cs`: Главные файлы приложения, содержащие глобальные ресурсы и обработку событий приложения.
- `MainWindow.xaml` и `MainWindow.xaml.cs`: Главное окно приложения, содержащее элементы интерфейса для входа в систему.
- `RegisterWindow.xaml` и `RegisterWindow.xaml.cs`: Окно регистрации нового пользователя.
- `ProductListWindow.xaml` и `ProductListWindow.xaml.cs`: Окно со списком товаров.
- `ProfileWindow.xaml` и `ProfileWindow.xaml.cs`: Окно профиля пользователя.
- `CaptchaWindow.xaml` и `CaptchaWindow.xaml.cs`: Окно для ввода капчи.
- `DatabaseHelper.cs`: Класс для работы с базой данных (выполнение запросов, получение данных).
- `PasswordHelper.cs`: Класс для хеширования паролей.
- `CaptchaGenerator.cs`: Класс для генерации капчи.

## Установка и запуск

### Требования

- Visual Studio с поддержкой .NET
- Microsoft SQL Server
- NuGet пакеты:
  - System.Data.SqlClient

### Настройка базы данных

1. Создайте базу данных в Microsoft SQL Server и добавьте таблицы для пользователей и товаров:

```sql
CREATE TABLE Users (
    Id INT PRIMARY KEY IDENTITY,
    Username NVARCHAR(50) NOT NULL,
    PasswordHash NVARCHAR(64) NOT NULL,
    Email NVARCHAR(100) NOT NULL,
    Role NVARCHAR(50),
    ProfileImage VARBINARY(MAX)
);

CREATE TABLE Products (
    Id INT PRIMARY KEY IDENTITY,
    Name NVARCHAR(100) NOT NULL,
    Price DECIMAL(18, 2) NOT NULL
);
```

2. Добавьте строку подключения к базе данных в `App.config`:

```xml
<configuration>
    <connectionStrings>
        <add name="DefaultConnection" connectionString="Server=localhost;Database=YourDatabase;Trusted_Connection=True;" providerName="System.Data.SqlClient" />
    </connectionStrings>
</configuration>
```

### Запуск приложения

1. Клонируйте репозиторий:

```sh
git clone https://github.com/yourusername/wpf-shop.git
```

2. Откройте проект в Visual Studio.

3. Убедитесь, что все NuGet пакеты установлены.

4. Постройте и запустите проект.

## Использование

### Регистрация

1. Запустите приложение и откройте окно регистрации.
2. Введите имя пользователя, пароль и email.
3. Нажмите кнопку регистрации. Пользователь будет сохранен в базе данных.

### Вход

1. На главном окне приложения введите имя пользователя и пароль.
2. Нажмите кнопку входа. Если учетные данные верны, откроется окно профиля пользователя.

### Просмотр товаров

1. Войдите в систему.
2. Откройте окно со списком товаров, где будут отображены все доступные товары из базы данных.

### Профиль пользователя

1. Войдите в систему.
2. Откройте окно профиля, где будет отображено имя пользователя, роль и изображение профиля.

## Вклад

Вы можете внести вклад в проект, создав пулл-реквест или открыв issue на GitHub.

## Лицензия

Этот проект лицензирован под лицензией MIT. Подробности см. в файле LICENSE.
```
