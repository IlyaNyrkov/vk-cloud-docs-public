## Установка аддона

Для аддона доступно [несколько вариантов установки](../../../../concepts/addons-and-settings/addons#osobennosti-ustanovki-addonov).

Примите во внимание суммарные [максимальные системные требования](../../../../concepts/addons-and-settings/addons) аддонов, которые будут размещены на группах worker-узлов. При необходимости [выполните ручное масштабирование](../../../scale#vypolnit-ruchnoe-masshtabirovanie) групп worker-узлов или [настройте автоматическое масштабирование](../../../scale#nastroit-avtomaticheskoe-masshtabirovanie--tolko-dlya-grupp-worker-uzlov-) перед установкой.

<tabs>
<tablist>
<tab>Стандартная установка</tab>
<tab>Установка на выделенные worker-узлы</tab>
<tab>Быстрая установка</tab>
</tablist>
<tabpanel>

1. Установите аддон:

   <tabs>
   <tablist>
   <tab>Личный кабинет</tab>
   </tablist>
   <tabpanel>

   1. Перейдите в [личный кабинет](https://mcs.mail.ru/app/) VK Cloud.
   1. Выберите проект и регион, где находится нужный кластер.
   1. Перейдите в раздел **Контейнеры → Кластеры Kubernetes**.
   1. Нажмите на имя нужного кластера.
   1. Перейдите на вкладку **Аддоны**.
   1. Если в кластере уже есть установленные аддоны, нажмите на кнопку **Добавить аддон**.
   1. Нажмите кнопку **Установить аддон** на карточке аддона `kube-prometheus-stack`.
   1. При необходимости отредактируйте:

      - название приложения;
      - название пространства имен, куда будет установлен аддон;
      - [код настройки аддона](#redaktirovanie-koda-nastroyki-addona-pri-ustanovke).

        <warn>

        Некорректно заданный код настройки может привести к ошибкам при установке или неработоспособности аддона.

        </warn>

   1. Нажмите кнопку **Установить аддон**.

      Начнется установка аддона в кластер. Этот процесс может занять длительное время.

   </tabpanel>
   </tabs>

1. При необходимости [измените размер диска Prometheus](#izmenenie-razmera-diska-prometheus).
1. При необходимости [получите пароль для Grafana из секрета Kubernetes](#poluchenie-parolya-dlya-grafana-iz-sekreta-kubernetes).

</tabpanel>
<tabpanel>

1. Подготовьте выделенную группу worker-узлов для установки аддона, если это еще не сделано:

   <tabs>
   <tablist>
   <tab>Личный кабинет</tab>
   </tablist>
   <tabpanel>

   1. Перейдите в [личный кабинет](https://mcs.mail.ru/app/) VK Cloud.
   1. Выберите проект и регион, где находится нужный кластер.
   1. Перейдите в раздел **Контейнеры → Кластеры Kubernetes**.
   1. Найдите в списке нужный кластер.

   1. Убедитесь, что в кластере есть выделенная группа worker-узлов, на которых будут размещаться аддоны.

      Если такой группы нет — [добавьте ее](../../../manage-node-group#dobavit-gruppu-worker-uzlov).

   1. [Задайте](../../../manage-node-group#nastroit-metki-i-ogranicheniya) для этой группы узлов, если это еще не сделано:

      - **Метку (label)**: ключ `addonNodes`, значение `dedicated`.
      - **Ограничение (taint)**: эффект `NoSchedule`, ключ `addonNodes`, значение `dedicated`.

   </tabpanel>
   </tabs>

1. Установите аддон:

   <tabs>
   <tablist>
   <tab>Личный кабинет</tab>
   </tablist>
   <tabpanel>

   1. Перейдите в [личный кабинет](https://mcs.mail.ru/app/) VK Cloud.
   1. Выберите проект и регион, где находится нужный кластер.
   1. Перейдите в раздел **Контейнеры → Кластеры Kubernetes**.
   1. Нажмите на имя нужного кластера.
   1. Перейдите на вкладку **Аддоны**.
   1. Если в кластере уже есть установленные аддоны, нажмите на кнопку **Добавить аддон**.
   1. Нажмите кнопку **Установить аддон** на карточке аддона `kube-prometheus-stack`.
   1. При необходимости отредактируйте:

      - название приложения;
      - название пространства имен, куда будет установлен аддон;
      - [код настройки аддона](#redaktirovanie-koda-nastroyki-addona-pri-ustanovke).

   1. Задайте нужные исключения (tolerations) и селекторы узлов (nodeSelector) в коде настройки аддона:

      <tabs>
      <tablist>
      <tab>Исключения</tab>
      <tab>Селекторы узлов</tab>
      </tablist>
      <tabpanel>

      ```yaml
      tolerations:
        - key: "addonNodes"
          operator: "Equal"
          value: "dedicated"
          effect: "NoSchedule"
      ```

      Задайте это исключение для полей:

      - `grafana.tolerations`;
      - `alertmanager.alertmanagerSpec.tolerations`;
      - `prometheusOperator.tolerations`;
      - `prometheusOperator.admissionWebhooks.patch.tolerations`;
      - `prometheus.prometheusSpec.tolerations`;
      - `kube-state-metrics.tolerations`.

      </tabpanel>
      <tabpanel>

      ```yaml
      nodeSelector:
        addonNodes: dedicated
      ```

      Задайте этот селектор для полей:

      - `grafana.nodeSelector`;
      - `alertmanager.alertmanagerSpec.nodeSelector`;
      - `prometheusOperator.nodeSelector`;
      - `prometheusOperator.admissionWebhooks.patch.nodeSelector`;
      - `prometheus.prometheusSpec.nodeSelector`;
      - `kube-state-metrics.nodeSelector`.

      </tabpanel>
      </tabs>

      <warn>

      Некорректно заданный код настройки может привести к ошибкам при установке или неработоспособности аддона.

      </warn>

   1. Нажмите кнопку **Установить аддон**.

      Начнется установка аддона в кластер. Этот процесс может занять длительное время.

   </tabpanel>
   </tabs>

1. При необходимости [измените размер диска Prometheus](#izmenenie-razmera-diska-prometheus).
1. При необходимости [получите пароль для Grafana из секрета Kubernetes](#poluchenie-parolya-dlya-grafana-iz-sekreta-kubernetes).

</tabpanel>
<tabpanel>

<info>

При быстрой установке будет создан секрет Kubernetes, содержащий постоянный пароль для входа в веб-интерфейс Grafana.

Чтобы [указать временный пароль](#redaktirovanie-koda-nastroyki-addona-pri-ustanovke) вместо постоянного, выполните **стандартную установку** или **установку на выделенные worker-узлы**.

</info>

1. Установите аддон:

   <tabs>
   <tablist>
   <tab>Личный кабинет</tab>
   </tablist>
   <tabpanel>

   1. Перейдите в [личный кабинет](https://mcs.mail.ru/app/) VK Cloud.
   1. Выберите проект и регион, где находится нужный кластер.
   1. Перейдите в раздел **Контейнеры → Кластеры Kubernetes**.
   1. Нажмите на имя нужного кластера.
   1. Перейдите на вкладку **Аддоны**.
   1. Если в кластере уже есть установленные аддоны, нажмите на кнопку **Добавить аддон**.
   1. Нажмите кнопку **Установить аддон** на карточке аддона `kube-prometheus-stack`.
   1. При необходимости отредактируйте:

      - название приложения;
      - название пространства имен, куда будет установлен аддон;

   1. Нажмите кнопку **Установить аддон**.

      Начнется установка аддона в кластер. Этот процесс может занять длительное время.

   </tabpanel>
   </tabs>

1. При необходимости [измените размер диска Prometheus](#izmenenie-razmera-diska-prometheus).
1. [Получите пароль для Grafana из секрета Kubernetes](#poluchenie-parolya-dlya-grafana-iz-sekreta-kubernetes).

</tabpanel>
</tabs>

## Редактирование кода настройки аддона при установке

<info>

- Редактирование кода аддона применимо для стандартной установки и установки на выделенные worker-узлы.
- Полный код настройки аддона вместе с описанием полей доступен на [GitHub](https://github.com/prometheus-community/helm-charts/blob/main/charts/kube-prometheus-stack/values.yaml).

</info>

### Установка временного пароля для веб-интерфейса Grafana

При установке аддона с параметрами по умолчанию будет создан секрет Kubernetes, содержащий постоянный пароль для входа в веб-интерфейс Grafana.

Также при установке аддона можно указать временный пароль пользователя. В этом случае первый вход в веб-интерфейс Grafana выполняется в этим паролем, затем будет предложено его сменить. Для этого измените значение поля в коде настройки аддона:

```yaml
grafana:
  adminPassword: "<временный пароль>"
```

После редактирования кода аддона [продолжите установку аддона](#ustanovka-addona).

## Изменение размера диска Prometheus

Эта операция доступна, если в кластере [установлен](#ustanovka-addona) аддон мониторинга `kube-prometheus-stack`.

На диске Prometheus хранятся данные мониторинга кластера. Если для них недостаточно места, или вы хотите увеличить производительность диска Prometheus, увеличьте размер диска.

<tabs>
<tablist>
<tab>Личный кабинет</tab>
</tablist>
<tabpanel>

1. Перейдите в [личный кабинет](https://mcs.mail.ru/app/) VK Cloud.
1. Выберите проект и регион, где находится нужный кластер.
1. Перейдите в раздел **Контейнеры → Кластеры Kubernetes**.
1. Нажмите на имя нужного кластера.
1. Перейдите на вкладку **Аддоны**.
1. Раскройте меню аддона `kube-prometheus-stack` и выберите пункт **Изменить размер диска Prometheus**.
1. Задайте нужный размер диска. Операция работает только в сторону увеличения.
1. Нажмите кнопку **Подтвердить**.

</tabpanel>
</tabs>

## Получение пароля для Grafana из секрета Kubernetes

Если аддон был установлен без указания временного пароля, значение пароля для входа в веб-интерфейс Grafana можно получить из секрета Kubernetes.

<info>

Если при добавлении аддона были выбраны имя сервиса, отличное от `kube-prometheus-stack`, или пространство имен, отличное от `prometheus-monitoring`, скорректируйте приведенные ниже шаги.

</info>

<tabs>
<tablist>
<tab>Kubernetes Dashboard</tab>
<tab>kubectl</tab>
</tablist>
<tabpanel>

1. [Подключитесь к кластеру](../../../../connect/k8s-dashboard) с помощью Kubernetes Dashboard.
1. В выпадающем списке рядом слева от строки поиска выберите пространство имен `prometheus-monitoring`.
1. Перейдите в раздел меню **Config and Storage → Secrets**.
1. Найдите в списке секретов `kube-prometheus-stack-grafana` и нажмите на имя секрета.
1. В блоке **Data** нажмите на иконку глаза рядом с параметром `admin-password`.

   Будет отображен пароль.

</tabpanel>
<tabpanel>

1. [Убедитесь](../../../../connect/kubectl#proverka-podklyucheniya-k-klasteru), что вы можете подключиться к кластеру с помощью `kubectl`.

1. Получите пароль для входа в Grafana из секрета Kubernetes:

   <tabs>
   <tablist>
   <tab>Windows (PowerShell)</tab>
   <tab>Linux (bash)/macOS (zsh)</tab>
   </tablist>
   <tabpanel>

   ```powershell
   $ENCODED = kubectl -n prometheus-monitoring get secret kube-prometheus-stack-grafana -o jsonpath='{.data.admin-password}'; `
   [System.Text.Encoding]::Utf8.GetString([System.Convert]::FromBase64String($ENCODED)) | Write-Output
   ```

   </tabpanel>
   <tabpanel>

   ```bash
   kubectl -n prometheus-monitoring get secret kube-prometheus-stack-grafana -o jsonpath='{.data.admin-password}' | base64 --decode
   ```

   </tabpanel>
   </tabs>

</tabpanel>
</tabs>
