# CI-CD GITLAB Windows

Качаем раннер.
для x86 https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-windows-386.exe

Для amd64
https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-windows-amd64.exe
Создаем папку C:\GitRunner, помещаем туда бинарник и переименовываем его в gitlab-runner
Создаем пользователя в системе из под которого будет запускаться раннер. Не уверен, что ранеру нужны админские права, но нужно чтобы этот пользователь мог читать/ писать/запускать из папок в том числе и Windows/TEMP
Идем в настройки CICD и открываем вкладку Runners

От туда берем url для подключения https://gitlab.com/ и токен
Запускаем процесс регистрации ранера:
./gitlab-runner.exe register

Вводим Url репозитория, в нашем случае это  https://gitlab.com/
Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com )
Вводим токен полученный из настроек вашего гитлаба
Please enter the gitlab-ci token for this runner
Вводим описание ранера, потом можно поправить.
Please enter the gitlab-ci description for this runner
test runner

Вводим имя метки. По факту, именно исходя из имени метки в gitlab-ci, гитлаб будет знать каким раннером нужно собирать ту или иную сборку.
Please enter the gitlab-ci tags for this runner (comma separated):
testtag
Вводим метку среды выполнения. В нашем случае, т.к. у нас все делается на shell скриптах, указываем shell
Please enter the executor: ssh, docker+machine, docker-ssh+machine, kubernetes, docker, parallels, virtualbox, docker-ssh, shell:
shell
Теперь устанавливаем ранер
.\gitlab-runner.exe install --user ENTER-YOUR-USERNAME --password ENTER-YOUR-PASSWORD
Тут важный момент. Раннер нужно ставить указывая пользователя, который был для него создан на первом этапе. Если этого не сделать, у ранера может не хватать прав на изменения на этой машине и вы будете получать невразумительные ошибки типа 
'"git"' is not recognized as an internal or external command, хотя сам гит у вас отлично работает

Запускаем ранер
.\gitlab-runner.exe start
Вся дальнейшая настройка поведения сборки будет идти через файл .gitlab-ci.yml в вашем репозитории
Например для указания, что нужно этот stage собирать только что установленным раннером используем метки tags

посмотреть список раннеров .\gitlab-runner.exe list
