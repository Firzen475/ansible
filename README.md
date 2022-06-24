# ansible

## Подготовка среды

- Загрузка проекта 

`sudo apt install git`  
`git clone --branch master --single-branch https://github.com/Firzen475/ansible.git`

- Доступ к WS

Для успешного выполнения скриптов нужно выполнить 4 условия  
1) Сервер или контейнер должен иметь возможность получать билет kerberos из домена  
`kinit [USER][@REALM]`  
2) Пользователь в файле inventory должен быть локальным администратором [link](https://winitpro.ru/index.php/2019/11/27/gpo-dobavit-v-gruppu-lok-admins/)  
3) На всех WS установлен PowerShell 5.0+  
4) На всех WS включен WinRM [link](https://winitpro.ru/index.php/2012/01/31/kak-aktivirovat-windows-remote-management-s-pomoshhyu-gruppovoj-politiki/)  
***

## Групповая установка софта  
### Общее  
- Пример структуры папок  

`../Soft/`  
`├── 1c_source`  
`│   ├── ...`  
`│   ├── 1CEnterprise 8.msi`  
`│   ├── ...`  
`│   ├── Setup.ini`  
`│   ├── ...`  
`├── ChromeSetup.exe`  
`├── KOMPAS-3D`  
`│   ├── 1. KOMPAS-3D V20 x64`  
`│   ├── 2. KOMPAS-3D V20 AEC x64`  
`│   ├── 3. KOMPAS-Expert V1.16 x64`  
`│   ├── 4. KOMPAS-Electric V20 x64`  
`│   ├── 5. CNC V20 x64`  
`│   ├── 6. UMExpress V1.42 x64`  
`│   └── UPDATES`  
`├── Microsoft.Office.2016`  
`│   ├── AUTORUN.exe`  
`│   ├── AUTORUN.inf`  
`│   └── Office`  
`│       ├── Data`  
`│       ├── deploy.exe`  
`│       ├── helper.exe`  
`│       └── Utilities`  
`└── winrar.exe`  
- Установка путей хранения дистрибутивов  

ansible.windows.win_copy не поддерживает переменные в src и dest, поэтому пути прописываются прямо в playbook
`cd ./install_soft/`    
`sed -i 's/\[path_to_soft\]/\\\\server\\share$\\some_shsre/' ./playbook.yml`    
`sed -i 's/\[path_to_1cestart\]/\\\\server\\share$\\some_share/' ./playbook.yml`  
В результате путь до инсталлятора Office получится `\\server\share$\\some_share\Soft\Microsoft.Office.2016\Office\helper.exe`  
- Группы распространения  

В файле inventory добавить ws в группу распростронения и заменить пользователя и пароль локального администратора.  
`nano ./inventory`  
### Office  
Для KMS ключей требуется образ корпоративного офиса Click-to-Run. Я воспользовался [этой](https://www.google.com/search?q=office+2016+helper.exe) сборкой  
Можно взять оффициальную но нужно чтобы в установленной сборке были файлы смены лицензий `\Microsoft Office\root\Licenses16`  
И переделать под это playbook 
Дистрибутив распаковать в папку "Microsoft.Office.2016"
### 1С  
- Дистрибутив  

Используется дистрибутив ["Технологическая платформа 1С:Предприятия для Windows"](https://releases.1c.ru/project/Platform83). Версия на x64 не тестировалась и не рекомендуется из-за проблем с библиотеками.  
Дистрибутив распаковать в папку "1c_source"  
- Базы  

Во время очистки удаляются все хэши и списки баз, поэтому рекомендуется создать ссылки на список сетевых баз.  
`nano ./1cestart.cfg`  
`sed -i 's/\[path_to_db_config\]/\\\\server\share$\1c_base/' ./1cestart.cfg`  
Параметр UseHwLicenses=0 задаёт принудительное получение лицензий от сервера. Это нужно если клиенты теряют лицензию через некоторое время после запуска 1с.  
Доступностью баз для пользователей или групп пользователей удобно управлять через параметры безопасности (чтение и выполнение).  
`cat ./base01.v8i`  
- Баги  

Устанавливается технологическая платформа с тонким и толстым клиентом, вес дистрибутива почти 1ГБ. Конечно желательно было бы ставить пользователям Тонкий клиент с весом дистрибутива 100МБ.  
Однако, после установки Тонкого клиента ланчер "\1cv8\common\1cestart.exe" не видит установленного Тонкого клиента и игнорирует настройки в файлах баз. Можно принудительно создать ярлык Тонкого клиента на всех рабочих столах, но что-то не хочется.






