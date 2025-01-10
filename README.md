# Домашнее задание к занятию "`Введение в Terraform`" - `Дедюрин Денис`

---
## Задание 1.

1. Перейдите в каталог src. Скачайте все необходимые зависимости, использованные в проекте.
1. Изучите файл .gitignore. В каком terraform-файле, согласно этому .gitignore, допустимо сохранить личную, секретную информацию?(логины,пароли,ключи,токены итд)
1. Выполните код проекта. Найдите в state-файле секретное содержимое созданного ресурса random_password, пришлите в качестве ответа конкретный ключ и его значение.
1. Раскомментируйте блок кода, примерно расположенный на строчках 29–42 файла main.tf. Выполните команду terraform validate. Объясните, в чём заключаются намеренно допущенные ошибки Исправьте их.
1. Выполните код. В качестве ответа приложите: исправленный фрагмент кода и вывод команды docker ps.
1. Замените имя docker-контейнера в блоке кода на hello_world. Не перепутайте имя контейнера и имя образа. Мы всё ещё продолжаем использовать name = "nginx:latest". Выполните команду terraform apply -auto-approve. Объясните своими словами, в чём может быть опасность применения ключа -auto-approve. Догадайтесь или нагуглите зачем может пригодиться данный ключ? В качестве ответа дополнительно приложите вывод команды docker ps.
1. Уничтожьте созданные ресурсы с помощью terraform. Убедитесь, что все ресурсы удалены. Приложите содержимое файла terraform.tfstate.
1. Объясните, почему при этом не был удалён docker-образ nginx:latest. Ответ ОБЯЗАТЕЛЬНО НАЙДИТЕ В ПРЕДОСТАВЛЕННОМ КОДЕ, а затем ОБЯЗАТЕЛЬНО ПОДКРЕПИТЕ строчкой из документации terraform провайдера docker. (ищите в классификаторе resource docker_image )

### Ответ:

Согласно файлу.gitignore

```
# Local .terraform directories and files
**/.terraform/*
.terraform*

!.terraformrc

# .tfstate files
*.tfstate
*.tfstate.*

# own secret vars store.
personal.auto.tfvars
```
Допустимо сохранить личную, секретную информацию в файле personal.auto.tfvars

Выполняем иницализацию проекта, создаем план и применяем изменения командами:
```
terraform init
terraform plan
terraform apply
```
<img src = "img/01.png" width = 100%>
<img src = "img/02.png" width = 100%>
<img src = "img/03.png" width = 100%>

В state-файле секретное содержимое созданного ресурса random_password имеет следующее значение:
```
"result": "6uJbptLYbNv5cQIj"
```
<img src = "img/04.png" width = 100%>

Раскометруем блок, указанный в задании, и выполняем команду:
```
terraform validate
```
Получаем следующие ошибки:
<img src = "img/05.png" width = 100%>

Текст ошибкки говорит о том, что
1. Отсутствует имя для ресурса docker_image.
```
resource "docker_image" {}
```
Правильное значение должно быть например такое:
```
resource "docker_image" "nginx" {}
```
2. Неверное имя ресурса docker_container.
```
resource "docker_container" "1nginx" {}
```
Имена ресурсов не могут начинаться с цифры. Переименовываем:
```
resource "docker_container" "nginx_container" {}
```
После исправления ошибок и повторном запуске команды получаем еще одну ошибку:
<img src = "img/06.png" width = 100%>
