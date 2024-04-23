Дипломный проект SkillFactory
---

Спринт 1.

Описать инфраструктру через Terraform.

---

Инфраструктура включает в себя три виртуальные машины: два узла для кластера Kubernetes и один сервер, предназначенный для инструментов мониторинга, журналирования и сборки контейнеров.

Настройка производится с использованием Terraform и Ansible.
 
---

Чтобы запустить этот проект, необходимо:

1. Terraform>=1.3.9

2. Ansible>=7

3. Зарегистрироваться в Яндекс Облако

3.1. Получить персональный OAuth token

4. Зарегистрироваться в GitLab

4.1. Создать проект stage-2

4.2. Создать в "Settings / CI/CD" регистрационный токен для gitlab-runner

4.3. Отключить общие раннеры (Shared runners)

---

В main.tf подставить свои регистрационные данные из Яндекс облака:

```
provider "yandex" {
  token     = "<OAuth_token>"
  cloud_id  = "<cloud_id>"
  folder_id = "<folder_id>"
  zone      = "ru-central1-a"
}
```

В variables.tf изменить путь до необходимого публичного и приватного ssh-ключей(если они имееют другое название):

```
  default     = {
    user        = "ubuntu"
    private_key = "~/.ssh/id_rsa"
    pub_key     = "~/.ssh/id_rsa.pub"
  }
```

В gitlab-run-token.yml добавить регистрационный токен из пункта 4.2

```
gitlab_run_token: "<runner_token>"
```

Скрипт ansibe-run.sh сделать исполняемым:

```
sudo chmod +x ansibe-run.sh
```

Запустить инициализацию Terraform:

```
terraform init
```

Запустить развёртку Terraform:

```
terraform apply -auto-approve
```
