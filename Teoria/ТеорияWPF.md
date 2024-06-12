### Основы WPF

#### Базовые элементы интерфейса

1. **Создание окна и базовых элементов:**
   ```xml
   <Window x:Class="YourNamespace.MainWindow"
           xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
           xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
           Title="Магазин" Height="450" Width="800">
       <Grid>
           <TextBox Name="UsernameTextBox" Width="200" Height="30" Margin="10"/>
           <PasswordBox Name="PasswordBox" Width="200" Height="30" Margin="10,50,0,0"/>
           <Button Content="Войти" Width="100" Height="30" Margin="10,100,0,0" Click="LoginButton_Click"/>
       </Grid>
   </Window>
   ```

2. **Код за файлами разметки (code-behind):**
   ```csharp
   public partial class MainWindow : Window
   {
       public MainWindow()
       {
           InitializeComponent();
       }

       private void LoginButton_Click(object sender, RoutedEventArgs e)
       {
           string username = UsernameTextBox.Text;
           string password = PasswordBox.Password;
           // Обработка логина
       }
   }
   ```

### Подключение к локальному Microsoft SQL Server

1. **Добавление подключения к базе данных в `App.config`:**
   ```xml
   <configuration>
       <connectionStrings>
           <add name="DefaultConnection" connectionString="Server=localhost;Database=YourDatabase;Trusted_Connection=True;" providerName="System.Data.SqlClient" />
       </connectionStrings>
   </configuration>
   ```

2. **Использование ADO.NET для взаимодействия с базой данных:**
   ```csharp
   using System.Data.SqlClient;

   public class DatabaseHelper
   {
       private string connectionString = ConfigurationManager.ConnectionStrings["DefaultConnection"].ConnectionString;

       public void ExecuteQuery(string query)
       {
           using (SqlConnection connection = new SqlConnection(connectionString))
           {
               SqlCommand command = new SqlCommand(query, connection);
               connection.Open();
               command.ExecuteNonQuery();
           }
       }

       // Метод для выборки данных
       public DataTable GetDataTable(string query)
       {
           using (SqlConnection connection = new SqlConnection(connectionString))
           {
               SqlCommand command = new SqlCommand(query, connection);
               SqlDataAdapter adapter = new SqlDataAdapter(command);
               DataTable dataTable = new DataTable();
               adapter.Fill(dataTable);
               return dataTable;
           }
       }
   }
   ```

### Настройки внешнего вида (стилизация)

1. **Добавление стилей в ресурсах приложения (`App.xaml`):**
   ```xml
   <Application x:Class="YourNamespace.App"
                xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
                StartupUri="MainWindow.xaml">
       <Application.Resources>
           <Style TargetType="Button">
               <Setter Property="Background" Value="LightBlue"/>
               <Setter Property="FontSize" Value="16"/>
           </Style>
           <Style TargetType="TextBox">
               <Setter Property="Margin" Value="5"/>
               <Setter Property="Padding" Value="5"/>
           </Style>
       </Application.Resources>
   </Application>
   ```

### Создание капчи

1. **Генерация капчи:**
   ```csharp
   public class CaptchaGenerator
   {
       public string GenerateCaptcha()
       {
           var random = new Random();
           var captcha = new StringBuilder();
           for (int i = 0; i < 6; i++)
           {
               captcha.Append((char)random.Next(65, 90));
           }
           return captcha.ToString();
       }
   }
   ```

2. **Отображение капчи:**
   ```xml
   <Window x:Class="YourNamespace.CaptchaWindow"
           xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
           xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
           Title="Captcha" Height="200" Width="400">
       <Grid>
           <TextBlock Name="CaptchaTextBlock" FontSize="24" HorizontalAlignment="Center" VerticalAlignment="Center"/>
           <TextBox Name="CaptchaTextBox" Width="200" Height="30" Margin="10,50,0,0" HorizontalAlignment="Center" VerticalAlignment="Center"/>
           <Button Content="Submit" Width="100" Height="30" Margin="10,100,0,0" Click="SubmitButton_Click"/>
       </Grid>
   </Window>
   ```

3. **Код за файлами разметки:**
   ```csharp
   public partial class CaptchaWindow : Window
   {
       private string captcha;

       public CaptchaWindow()
       {
           InitializeComponent();
           CaptchaGenerator generator = new CaptchaGenerator();
           captcha = generator.GenerateCaptcha();
           CaptchaTextBlock.Text = captcha;
       }

       private void SubmitButton_Click(object sender, RoutedEventArgs e)
       {
           if (CaptchaTextBox.Text == captcha)
           {
               MessageBox.Show("Captcha verified");
           }
           else
           {
               MessageBox.Show("Captcha incorrect");
           }
       }
   }
   ```

### Хеширование паролей

1. **Хеширование пароля с использованием SHA256:**
   ```csharp
   using System.Security.Cryptography;
   using System.Text;

   public class PasswordHelper
   {
       public string HashPassword(string password)
       {
           using (SHA256 sha256 = SHA256.Create())
           {
               byte[] bytes = sha256.ComputeHash(Encoding.UTF8.GetBytes(password));
               StringBuilder builder = new StringBuilder();
               foreach (byte b in bytes)
               {
                   builder.Append(b.ToString("x2"));
               }
               return builder.ToString();
           }
       }
   }
   ```

### Форма логина и регистрации

1. **Форма логина:**
   ```xml
   <Window x:Class="YourNamespace.LoginWindow"
           xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
           xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
           Title="Login" Height="300" Width="400">
       <Grid>
           <TextBox Name="UsernameTextBox" Width="200" Height="30" Margin="10"/>
           <PasswordBox Name="PasswordBox" Width="200" Height="30" Margin="10,50,0,0"/>
           <Button Content="Войти" Width="100" Height="30" Margin="10,100,0,0" Click="LoginButton_Click"/>
       </Grid>
   </Window>
   ```

2. **Форма регистрации:**
   ```xml
   <Window x:Class="YourNamespace.RegisterWindow"
           xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
           xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
           Title="Register" Height="400" Width="400">
       <Grid>
           <TextBox Name="UsernameTextBox" Width="200" Height="30" Margin="10"/>
           <PasswordBox Name="PasswordBox" Width="200" Height="30" Margin="10,50,0,0"/>
           <TextBox Name="EmailTextBox" Width="200" Height="30" Margin="10,100,0,0"/>
           <Button Content="Регистрация" Width="100" Height="30" Margin="10,150,0,0" Click="RegisterButton_Click"/>
       </Grid>
   </Window>
   ```

3. **Код за файлами разметки для логина и регистрации:**
   ```csharp
   public partial class LoginWindow : Window
   {
       public LoginWindow()
       {
           InitializeComponent();
       }

       private void LoginButton_Click(object sender, RoutedEventArgs e)
       {
           string username = UsernameTextBox.Text;
           string password = PasswordBox.Password;

           PasswordHelper passwordHelper = new PasswordHelper();
           string hashedPassword = passwordHelper.HashPassword(password);

           // Проверьте учетные данные пользователя в базе данных
       }
   }

   public partial class RegisterWindow : Window
   {
       public RegisterWindow()
       {
           InitializeComponent();
       }

       private void RegisterButton_Click(object sender, RoutedEventArgs e)
       {
           string username = UsernameTextBox.Text;
           string password = PasswordBox.Password;
           string email = EmailTextBox.Text;

           PasswordHelper passwordHelper = new PasswordHelper();
           string hashedPassword = passwordHelper.HashPassword(password);

           // Сохраните нового пользователя в базе данных
       }
   }
   ```

### Список товаров и профиля пользователя

1. **Список товаров:**
   ```xml
   <Window x:Class="YourNamespace.ProductListWindow"
           xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
           xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
           Title="Product List" Height

="450" Width="800">
       <Grid>
           <ListView Name="ProductListView">
               <ListView.View>
                   <GridView>
                       <GridViewColumn Header="Название" DisplayMemberBinding="{Binding Name}" Width="200"/>
                       <GridViewColumn Header="Цена" DisplayMemberBinding="{Binding Price}" Width="100"/>
                   </GridView>
               </ListView.View>
           </ListView>
       </Grid>
   </Window>
   ```

2. **Код за файлами разметки для списка товаров:**
   ```csharp
   public partial class ProductListWindow : Window
   {
       public ProductListWindow()
       {
           InitializeComponent();
           LoadProducts();
       }

       private void LoadProducts()
       {
           DatabaseHelper dbHelper = new DatabaseHelper();
           DataTable products = dbHelper.GetDataTable("SELECT Name, Price FROM Products");
           ProductListView.ItemsSource = products.DefaultView;
       }
   }
   ```

3. **Профиль пользователя:**
   ```xml
   <Window x:Class="YourNamespace.ProfileWindow"
           xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
           xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
           Title="Profile" Height="300" Width="400">
       <Grid>
           <Image Name="ProfileImage" Width="100" Height="100" Margin="10"/>
           <TextBlock Name="UsernameTextBlock" FontSize="16" Margin="10,120,0,0"/>
           <TextBlock Name="RoleTextBlock" FontSize="16" Margin="10,150,0,0"/>
       </Grid>
   </Window>
   ```

4. **Код за файлами разметки для профиля пользователя:**
   ```csharp
   public partial class ProfileWindow : Window
   {
       public ProfileWindow(string username)
       {
           InitializeComponent();
           LoadUserProfile(username);
       }

       private void LoadUserProfile(string username)
       {
           DatabaseHelper dbHelper = new DatabaseHelper();
           DataTable userProfile = dbHelper.GetDataTable($"SELECT Username, Role, ProfileImage FROM Users WHERE Username = '{username}'");

           if (userProfile.Rows.Count > 0)
           {
               UsernameTextBlock.Text = userProfile.Rows[0]["Username"].ToString();
               RoleTextBlock.Text = userProfile.Rows[0]["Role"].ToString();

               byte[] imageBytes = (byte[])userProfile.Rows[0]["ProfileImage"];
               BitmapImage image = new BitmapImage();
               using (MemoryStream ms = new MemoryStream(imageBytes))
               {
                   image.BeginInit();
                   image.CacheOption = BitmapCacheOption.OnLoad;
                   image.StreamSource = ms;
                   image.EndInit();
               }
               ProfileImage.Source = image;
           }
       }
   }
   ```

### Процесс входа и регистрации

1. **Процесс входа:**
   ```csharp
   private void LoginButton_Click(object sender, RoutedEventArgs e)
   {
       string username = UsernameTextBox.Text;
       string password = PasswordBox.Password;

       PasswordHelper passwordHelper = new PasswordHelper();
       string hashedPassword = passwordHelper.HashPassword(password);

       DatabaseHelper dbHelper = new DatabaseHelper();
       DataTable user = dbHelper.GetDataTable($"SELECT * FROM Users WHERE Username = '{username}' AND PasswordHash = '{hashedPassword}'");

       if (user.Rows.Count > 0)
       {
           ProfileWindow profileWindow = new ProfileWindow(username);
           profileWindow.Show();
           this.Close();
       }
       else
       {
           MessageBox.Show("Неправильное имя пользователя или пароль");
       }
   }
   ```

2. **Процесс регистрации:**
   ```csharp
   private void RegisterButton_Click(object sender, RoutedEventArgs e)
   {
       string username = UsernameTextBox.Text;
       string password = PasswordBox.Password;
       string email = EmailTextBox.Text;

       PasswordHelper passwordHelper = new PasswordHelper();
       string hashedPassword = passwordHelper.HashPassword(password);

       DatabaseHelper dbHelper = new DatabaseHelper();
       dbHelper.ExecuteQuery($"INSERT INTO Users (Username, PasswordHash, Email) VALUES ('{username}', '{hashedPassword}', '{email}')");

       MessageBox.Show("Регистрация успешна");
   }
   ```
