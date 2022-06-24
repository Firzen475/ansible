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
Пример структуры папок
`../Soft/`
├── 1c_source
│   ├── 0x0402.ini
│   ├── 0x0407.ini
│   ├── 0x0408.ini
│   ├── 0x0409.ini
│   ├── 0x040a.ini
│   ├── 0x040c.ini
│   ├── 0x040e.ini
│   ├── 0x0410.ini
│   ├── 0x0415.ini
│   ├── 0x0418.ini
│   ├── 0x0419.ini
│   ├── 0x041f.ini
│   ├── 0x0422.ini
│   ├── 0x0426.ini
│   ├── 0x0427.ini
│   ├── 0x042a.ini
│   ├── 0x042b.ini
│   ├── 0x0804.ini
│   ├── 1026.mst
│   ├── 1026_xp.mst
│   ├── 1031.mst
│   ├── 1031_xp.mst
│   ├── 1032.mst
│   ├── 1032_xp.mst
│   ├── 1033.mst
│   ├── 1033_xp.mst
│   ├── 1034.mst
│   ├── 1034_xp.mst
│   ├── 1036.mst
│   ├── 1036_xp.mst
│   ├── 1038.mst
│   ├── 1038_xp.mst
│   ├── 1040.mst
│   ├── 1040_xp.mst
│   ├── 1045.mst
│   ├── 1045_xp.mst
│   ├── 1048.mst
│   ├── 1048_xp.mst
│   ├── 1049.mst
│   ├── 1049_xp.mst
│   ├── 1055.mst
│   ├── 1055_xp.mst
│   ├── 1058.mst
│   ├── 1058_xp.mst
│   ├── 1062.mst
│   ├── 1062_xp.mst
│   ├── 1063.mst
│   ├── 1063_xp.mst
│   ├── 1066.mst
│   ├── 1067.mst
│   ├── 1CEnterprise 8.msi
│   ├── 1CEnterprise 8_xp.msi
│   ├── 2052.mst
│   ├── 2052_xp.mst
│   ├── adminstallrelogon.mst
│   ├── adminstallrestart.mst
│   ├── Data1.cab
│   ├── license-tools
│   ├── setup.exe
│   ├── Setup.ini
│   ├── vc_redist.x86.exe
│   └── WindowsInstaller-KB893803-x86.exe
├── 1c_source2
│   ├── 0x0402.ini
│   ├── 0x0407.ini
│   ├── 0x0408.ini
│   ├── 0x0409.ini
│   ├── 0x040a.ini
│   ├── 0x040c.ini
│   ├── 0x040e.ini
│   ├── 0x0410.ini
│   ├── 0x0415.ini
│   ├── 0x0418.ini
│   ├── 0x0419.ini
│   ├── 0x041f.ini
│   ├── 0x0422.ini
│   ├── 0x0426.ini
│   ├── 0x0427.ini
│   ├── 0x042a.ini
│   ├── 0x042b.ini
│   ├── 0x0442.ini
│   ├── 0x0804.ini
│   ├── 1026.mst
│   ├── 1026_xp.mst
│   ├── 1031.mst
│   ├── 1031_xp.mst
│   ├── 1032.mst
│   ├── 1032_xp.mst
│   ├── 1033.mst
│   ├── 1033_xp.mst
│   ├── 1034.mst
│   ├── 1034_xp.mst
│   ├── 1036.mst
│   ├── 1036_xp.mst
│   ├── 1038.mst
│   ├── 1038_xp.mst
│   ├── 1040.mst
│   ├── 1040_xp.mst
│   ├── 1045.mst
│   ├── 1045_xp.mst
│   ├── 1048.mst
│   ├── 1048_xp.mst
│   ├── 1049.mst
│   ├── 1049_xp.mst
│   ├── 1055.mst
│   ├── 1055_xp.mst
│   ├── 1058.mst
│   ├── 1058_xp.mst
│   ├── 1062.mst
│   ├── 1062_xp.mst
│   ├── 1063.mst
│   ├── 1063_xp.mst
│   ├── 1066.mst
│   ├── 1067.mst
│   ├── 1090.mst
│   ├── 1CEnterprise 8.msi
│   ├── 1CEnterprise 8_xp.msi
│   ├── 2052.mst
│   ├── 2052_xp.mst
│   ├── adminstallrelogon.mst
│   ├── adminstallrestart.mst
│   ├── Data1.cab
│   ├── extinfo
│   ├── setup.exe
│   ├── Setup.ini
│   ├── vc_redist.x86.exe
│   └── WindowsInstaller-KB893803-x86.exe
├── ArtCam
│   └── ArtCAM 2018
├── ChromeSetup.exe
├── CIMCO Edit 8.10.07
│   └── CIMCO Edit 8.10.07
├── KOMPAS-3D
│   ├── 1. KOMPAS-3D V20 x64
│   ├── 2. KOMPAS-3D V20 AEC x64
│   ├── 3. KOMPAS-Expert V1.16 x64
│   ├── 4. KOMPAS-Electric V20 x64
│   ├── 5. CNC V20 x64
│   ├── 6. UMExpress V1.42 x64
│   └── UPDATES
├── KOMPAS-3D V19
│   └── Новая папка
├── KOMPAS-3D V20 x641
├── KOMPAS-3D V20 x64.rar
├── Microsoft.Office.2016
│   ├── AUTORUN.exe
│   ├── AUTORUN.inf
│   └── Office
├── Office Timeline Pro 6.06.00.00_fu11.rar
├── OffScrub_O16msi.vbs
├── ProNest.2019.Build.13.0.4.Win64-SSQ
│   ├── Postprocessors
│   ├── ProNest13_00_04_64bit.exe
│   ├── _SolidSQUAD_
│   └── z_checksums.sfv
├── Siemens.NX.12.0.0.Win64-SSQ
│   ├── readme_windows.txt
│   ├── Release_Info
│   ├── Siemens.NX.12.0.0.Docs.API.Win64.iso
│   ├── Siemens.NX.12.0.0.Docs.Eng.Win64.iso
│   ├── Siemens.NX12.0.0.Win64
│   ├── Siemens.NX12.0.0.Win64.iso
│   ├── _SolidSQUAD_
│   ├── _SolidSQUAD_.7z
│   ├── Unisign siemens-840C.cps
│   └── z_checksums.sfv
├── winrar.exe
└── Астра
    ├── Astra5.zip
    ├── Thumbs.db
    ├── Астра Раскрой
    └── Новый точечный рисунок.bmp

Установка путей хранения дистрибутивов  
'cd ./install_soft/'  
`sed -i 's/\[path_to_soft\]/\\\\server\\share$\\some_shsre/' ./playbook.yml`    
`sed -i 's/\[path_to_1cestart\]/\\\\server\\share$\\some_share/' ./playbook.yml`  
В результате путь до инсталлятора Office получится `\\server\share$\\some_share\Soft\Microsoft.Office.2016\Office\helper.exe`

 - Изменение переменных и путей

ansible.windows.win_copy не поддерживает переменные в src и dest, поэтому пути прописываются прямо в playbook

`nano ./install_1c.yml`

В файле inventory добавить ws в группу распростронения и заменить пользователя и пароль локального администратора.

`nano ./inventory`

 - Рекомендации

Устанавливается распакованный архив "Технологическая платформа 1С:Предприятия для Windows". Версия на x64 не тестировалась и не рекомендуется из-за проблем с библиотеками.

Во время очистки удаляются все хэши и списки баз, поэтому рекомендуется создать ссылки на список сетевых баз

`nano ./1cestart.cfg`

Параметр UseHwLicenses=0 задаёт принудительное получение лицензий от сервера. Это нужно если клиенты теряют лицензию через некоторое время после запуска 1с

`nano ./base01.v8i`

Доступностью баз для пользователей или групп пользователей удобно управлять через параметры безопасности (чтение и выполнение)

 - Баги

Устанавливается технологическая платформа с тонким и толстым клиентом, вес дистрибутива почти 1ГБ. Конечно желательно было бы ставить пользователям Тонкий клиент с весом дистрибутива 100МБ. 

Однако, после установки Тонкого клиента ланчер "\1cv8\common\1cestart.exe" не видит установленного Тонкого клиента и игнорирует настройки в файлах баз. Можно принудительно создать ярлык Тонкого клиента на всех рабочих столах, но что-то не хочется.






