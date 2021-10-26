# devops-netology
В папке Terraform я создал файл .gitignore который описывает какие файлы игнорировать в данной директории
**/.terraform/* - во всех директориях игнорировать содержимое директории .terraform

*.tfstate - игнорировать файлы которые содержат 0 и более символов перед .tfstate
*.tfstate.* - игнорировать файлы содержашие 0 и более перед .tfstate и после .tfstate

crash.log - игнорировать конкретный файл crash.log

*.tfvars - игнорировать 0 и более символов перед .tfvars

override.tf - игнорировать конкретный файл override.tf
override.tf.json - игнорировать конкретный файл override.tf.json
*_override.tf - игнорировать 0 и более символов перед _override.tf
*_override.tf.json - игнорировать 0 и более символов перед _override.tf.json

.terraformrc - игнорировать конкретно .terraformrc
terraform.rc - игнорировать конкретно terraform.rc
