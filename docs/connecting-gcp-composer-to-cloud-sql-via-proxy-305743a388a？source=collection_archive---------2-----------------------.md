# 如何使用代理将 GCP 作曲家连接到云 SQL

> 原文：<https://medium.com/nerd-for-tech/connecting-gcp-composer-to-cloud-sql-via-proxy-305743a388a?source=collection_archive---------2----------------------->

![](img/125522b90632f8c51c4ac5c800d0263c.png)

照片由[米切尔罗](https://unsplash.com/@mitchel3uo?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这是一个分步指南，使用代理将从 Kubernetes 集群运行的 GCP Composer 连接到 PostgreSQL 的 GCP SQL 实例。

这是一个比理论更实用的指南。因此，如果您希望了解这些技术的小细节和架构，您可以随时查看每个部分中的链接。

有一个关于设置这些技术的指南，所以在我们继续之前，请确保该指南已经完成。

[](https://dorlabor1.medium.com/setting-up-cloud-composer-cloud-sql-and-google-cloud-sdk-22f02bc0d1cd) [## 设置 Cloud Composer、云 SQL 和 Google Cloud SDK

### 这是设置 Cloud Composer、Cloud SQL 和 Google Cloud SDK 的分步指南。本指南是…

dorlabor1.medium.com](https://dorlabor1.medium.com/setting-up-cloud-composer-cloud-sql-and-google-cloud-sdk-22f02bc0d1cd) 

# 为什么我会在这里？

帮助您从 Composer 集群连接到 GCP SQL。为此，建议使用云 SQL 代理。

目前，还没有简单完整的指南。我找不到在一个地方收集所有必要信息的指南，事实上，在我看来，文档中的一些信息有点误导。理解这一切，知道它们在哪里，并通过阅读不同的相关文档来学习如何做，是相当复杂和耗时的

所以我来了😎。

# 什么是云 SQL 代理？

[](https://cloud.google.com/sql/docs/postgres/sql-proxy) [## 关于云 SQL 代理|云 SQL for PostgreSQL | Google Cloud

### 此页面提供了云 SQL 代理的基本介绍，并描述了代理选项。对于循序渐进的…

cloud.google.com](https://cloud.google.com/sql/docs/postgres/sql-proxy) 

**云 SQL 代理**提供对实例的安全访问，无需[授权网络](https://cloud.google.com/sql/docs/postgres/configure-ip)或[配置 SSL](https://cloud.google.com/sql/docs/postgres/configure-ssl-instance) 。

使用云 SQL 代理访问云 SQL 实例具有以下优势:

*   **安全连接:**云 SQL 代理使用 TLS 1.2 和 128 位 AES 密码自动加密进出数据库的流量；SSL 证书用于验证客户端和服务器身份。
*   **更简单的连接管理:**云 SQL 代理使用云 SQL 处理身份验证，无需提供静态 IP 地址。

云 SQL 代理不提供新的连接路径；它依赖于现有的 IP 连接。

简单地说，通过云 SQL 代理将 GCP 作曲家连接到云 SQL 有点痛苦，但从长远来看，这是值得的。

保守秘密🤫。

# 什么是秘密？

[](https://cloud.google.com/kubernetes-engine/docs/concepts/secret?hl=en#creating_a_secret) [## 秘密| Kubernetes 引擎文档|谷歌云

### 本页描述了 Kubernetes 中的秘密对象及其在 Google Kubernetes 引擎(GKE)中的使用。秘密是安全的…

cloud.google.com](https://cloud.google.com/kubernetes-engine/docs/concepts/secret?hl=en#creating_a_secret) 

**Secrets** 是存储敏感数据的安全对象，比如密码、OAuth 令牌和 SSH 密钥。

在 Kubernetes 中，[秘密](https://cloud.google.com/kubernetes-engine/docs/concepts/secret)是一种将配置细节传递给应用程序的安全方式。您可以创建一个包含数据库名称、用户和密码等细节的秘密，它可以作为 env 变量注入到您的应用程序中。

根据连接类型的不同，使用机密的方式有很多种:

*   数据库凭据机密包括您正在连接的数据库的用户名和用户的数据库密码。
*   如果与代理连接，可以使用一个密码来保存您的服务帐户的凭据文件。

# 为数据库信息创建秘密对象

*   进入“导航菜单”->“Kubernetes 引擎”。
*   **注意“姓名”和“地点”字段的详细信息。**
*   “名称”将被称为“群集名称”。
*   转到“Kubernetes 引擎”下的“配置”。
*   并排打开:代码编辑器和“配置”页面中的终端。

运行:

```
gcloud container clusters get-credentials CLUSTER_NAME
```

[](https://cloud.google.com/sql/docs/postgres/connect-kubernetes-engine#creating_a_secret_object) [## 从 Google Kubernetes 引擎|云 SQL for PostgreSQL 连接

### 本页描述了如何建立从运行在 Google Kubernetes 引擎中的应用程序到云 SQL 的连接…

cloud.google.com](https://cloud.google.com/sql/docs/postgres/connect-kubernetes-engine#creating_a_secret_object) 

*   运行:

```
kubectl create secret generic <YOUR-DB-SECRET> \
  --from-literal=username=<YOUR-DATABASE-USER> \
  --from-literal=password=<YOUR-DATABASE-PASSWORD> \
  --from-literal=database=<YOUR-DATABASE-NAME>
```

*   <your-db-secret>=给它起一个你想要的名字(**请注意)。**</your-db-secret>

名称必须由小写字母数字字符、'-'或'.'组成，并且必须以字母数字字符开始和结束(例如' example.com '，用于验证的 regex 是'[a-z0–9]([-a-z0–9]*[a-z0–9])？(\.[a-z0–9]([-a-z0–9]*[a-z0–9])？)*').

*   <your-database-user>、<your-database-password>和<your-database-name>它们是您用 SQL 实例创建的(我告诉过您要注意这一点)。</your-database-name></your-database-password></your-database-user>

请刷新您的“配置”页面，然后单击您刚刚创建的密码。

**注意“名称空间”(**应该是默认的**)。**

转到下面的链接，通过单击“Enable API”按钮验证云 SQL Admin API 是否已启用:

[](https://cloud.google.com/sql/docs/postgres/connect-kubernetes-engine#proxy) [## 从 Google Kubernetes 引擎|云 SQL for PostgreSQL 连接

### 本页描述了如何建立从运行在 Google Kubernetes 引擎中的应用程序到云 SQL 的连接…

cloud.google.com](https://cloud.google.com/sql/docs/postgres/connect-kubernetes-engine#proxy) 

我们现在需要向代理提供服务帐户。

为此，我们需要配置 GKE(Google Kubernetes 引擎)来为云 SQL 代理提供服务帐户。有两种推荐的方法可以做到这一点:[工作负载标识](https://cloud.google.com/sql/docs/postgres/connect-kubernetes-engine#workload-identity)或者一个[服务帐户密钥文件](https://cloud.google.com/sql/docs/postgres/connect-kubernetes-engine#service-account-key-file)。

所以…工作负载身份…👷‍♀️

# 工作量身份…啊？

您可以在此阅读有关工作负载身份的信息:

[](https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity#concepts) [## 使用工作负载标识| Kubernetes 引擎文档

### 本页解释了推荐您的 Google Kubernetes 引擎(GKE)应用程序使用服务的方式…

cloud.google.com](https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity#concepts) 

如果你正在使用 [Google Kubernetes 引擎](https://cloud.google.com/kubernetes-engine)，首选方法是使用 GKE 的[工作负载身份](https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity)特性。这个方法允许您将一个 [Kubernetes 服务帐户(KSA)](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/) 绑定到一个 Google 服务帐户(GSA)。然后，GSA 将可供使用匹配 KSA 的应用程序访问。

Google 服务帐户(GSA)是一个 IAM 身份，代表您在 Google Cloud 中的应用程序。类似地，Kubernetes 服务帐户(KSA)是在 Google Kubernetes 引擎集群中代表您的应用程序的身份。

工作负载身份将一个 KSA 绑定到一个 GSA，使得任何使用该 KSA 的部署在与 Google Cloud 交互时都被认证为 GSA。

**请记住术语** : GKE、KSA、GSA。

# 创建工作负荷标识

1.  请在集群中启用工作负荷标识:

[](https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity#enable_on_cluster) [## 使用工作负载标识| Kubernetes 引擎文档

### 本页解释了推荐您的 Google Kubernetes 引擎(GKE)应用程序使用服务的方式…

cloud.google.com](https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity#enable_on_cluster) 

验证是否已启用 IAM 服务帐户凭据 API:

*   请单击“启用 IAM 凭据 API”。

(您不需要“创建凭证”)

*   要在现有集群中启用工作负载标识，请使用以下命令修改集群:

```
gcloud container clusters update CLUSTER_NAME \
  --workload-pool=PROJECT_ID.svc.id.goog
```

*   要查找您的 PROJECT_ID，请访问:

![](img/909b877f97e852662dabc92f02b03144.png)

*   “ID”下面是您的项目 ID。

2.为您的应用程序创建 KSA:

[](https://cloud.google.com/sql/docs/postgres/connect-kubernetes-engine#workload-identity) [## 从 Google Kubernetes 引擎|云 SQL for PostgreSQL 连接

### 本页描述了如何建立从运行在 Google Kubernetes 引擎中的应用程序到云 SQL 的连接…

cloud.google.com](https://cloud.google.com/sql/docs/postgres/connect-kubernetes-engine#workload-identity) 

通常，每个应用程序都有自己的身份，由一对 KSA 和 GSA 表示。通过激活来创建它:

`kubectl apply -f NameOfYourFile.yaml`

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: <YOUR-KSA-NAME> # TODO(developer): replace these values
```

*   <your-ksa-name>=选择自己喜欢的名字(**请注意**)。</your-ksa-name>

![](img/fd61a8ead1755c0d0ef37afb2beb717d.png)

在我的例子中:

*   <your-ksa-name>=测试-ksa。</your-ksa-name>
*   转到“Configuration”并**注意您新创建的 KSA 的“Name”及其“Namespace”(**该名称可能略有不同，例如，[test-ksa-token-s](https://console.cloud.google.com/kubernetes/secret/us-central1-a/us-central1-composer-b80be761-gke/default/test-ksa-token-rgfj8?project=lateral-now-306513)something**)，**“Type”应该是 Secret，而“Namespace”是默认的。

3.启用您的-GSA-NAME 和您的-KSA-NAME 之间的 IAM 绑定:

[](https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity#authenticating_to) [## 使用工作负载标识| Kubernetes 引擎文档

### 本页解释了推荐您的 Google Kubernetes 引擎(GKE)应用程序使用服务的方式…

cloud.google.com](https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity#authenticating_to) 

(转到第 5 题。)

运行:

```
gcloud iam service-accounts add-iam-policy-binding \
  --role roles/iam.workloadIdentityUser \ 
--member "serviceAccount:PROJECT_ID.svc.id.goog[K8S_NAMESPACE/KSA_NAME]" \
  GSA_NAME@PROJECT_ID.iam.gserviceaccount.com
```

K8S 代表 Kubernetes，8 在“K”和“S”之间交替八个字母。8="ubernete "。

不知道他们为什么要写 K8S_NAMESPACE。它应该写成 KSA 命名空间，在我们的例子中，这意味着默认。

4.添加注释以完成绑定:

运行:

```
kubectl annotate serviceaccount \
  --namespace K8S_NAMESPACE \
  KSA_NAME \
  iam.gke.io/gcp-service-account=GSA_NAME@PROJECT_ID.iam.gserviceaccount.com
```

*   这里的 KSA 名字应该是你给它的原始名字，而不是在它被改变之后。

例如:在我的情况下，原始名称是 *test-ksa。*

5.确保为 k8s 对象指定服务帐户，并将代理作为 sidecar 运行:

[](https://cloud.google.com/sql/docs/postgres/connect-kubernetes-engine#running_the_proxy_as_a_sidecar) [## 从 Google Kubernetes 引擎|云 SQL for PostgreSQL 连接

### 本页描述了如何建立从运行在 Google Kubernetes 引擎中的应用程序到云 SQL 的连接…

cloud.google.com](https://cloud.google.com/sql/docs/postgres/connect-kubernetes-engine#running_the_proxy_as_a_sidecar) 

运行:

`kubectl apply -f NameOfYourFile.yaml`

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: <YOUE-DEPLOYMENT-NAME>
spec:
  selector:
    matchLabels:
      app:  <YOUR-APPLICATION-NAME>
  template:
    metadata:
      labels:
        app:  <YOUR-APPLICATION-NAME>
    spec:
      serviceAccountName: <YOUR-KSA-NAME>
      containers:
      - name: cloud-sql-proxy
        image: gcr.io/cloudsql-docker/gce-proxy:1.17
        env:
        - name: DB_USER
          valueFrom:
            secretKeyRef:
               name: <YOUR-DB-SECRET>
               key: username
        - name: DB_PASS
          valueFrom:
            secretKeyRef:
               name: <YOUR-DB-SECRET>
               key: password
        - name: DB_NAME
          valueFrom:
            secretKeyRef:
              name: <YOUR-DB-SECRET>
              key: database
        command:
        - "/cloud_sql_proxy"
        - "-instances=<INSTANCE_CONNECTION_NAME>=tcp:<DB_PORT>"
        securityContext:
          runAsNonRoot: true
```

*   <your-deployment-name>=给它一个合适的名称(您可以在“Kubernetes Engine”下的“Workloads”中看到您的部署)。</your-deployment-name>
*   <your-application-name>其实就是你的<gke-name>意思是<cluster_name>。</cluster_name></gke-name></your-application-name>
*   <your-ksa-name>(你给它起的原名)。</your-ksa-name>
*   你可以随意调用“cloud-sql-proxy”。既然我们创建了一个代理，那么称之为“cloud-sql-proxy”是有意义的。
*   <your-db-secret>(您在为数据库信息创建秘密对象一节中创建了它)。</your-db-secret>
*   <instance_connection_name>是一个 SQL 实例的连接名。</instance_connection_name>
*   因为我们使用 PostgreSQL，所以<db_port>是 5432。</db_port>
*   转到“Kubernetes 引擎”下的“工作负载”。
*   您的最后一个部署应该是您刚刚创建的部署。
*   单击您的部署名称。
*   向下滚动，直到看到“Managed pods ”,然后单击其链接。
*   向下滚动，直到你看到“容器”，下面你应该看到“云-SQL-代理”。

恭喜你！我们已经配置好了一切。现在，我们所要做的就是在 Airflow Webserver 中执行我们的代码。🐱‍🏍

# 执行您的代码

 [## air flow . contrib . example _ DAGs . example _ GCP _ SQL _ query-air flow 文档

### 编辑描述

airflow.apache.org](https://airflow.apache.org/docs/apache-airflow/1.10.9/_modules/airflow/contrib/example_dags/example_gcp_sql_query.html) 

请将下面的代码输入到 file.py 中(除了“可选的”，除非你想)。

```
*import* os
*from* os.path *import* expanduser
*from* datetime *import* timedelta*from* six.moves.urllib.parse *import* quote_plus*from* airflow *import* DAG
*from* airflow.models *import* Variable
*from* airflow.contrib.operators.gcp_sql_operator *import* CloudSqlQueryOperator
*from* airflow.operators.python_operator *import* PythonOperator
*from* airflow.utils.dates *import* days_ago*from* google.cloud *import* storage
*from* google.cloud.storage._helpers *import* _PropertyMixin
```

```
(this part is optional! it is called "Airflow variables" and its mainly usage is for hiding sensitive data information)dag_config=Variable.get("proxy_con_variables",deserialize_json=True)
GCP_PROJECT_ID_VARIABLE = dag_config["gcp_project_id"]
GCP_REGION_VARIABLE = dag_config["gcp_region"]
GCSQL_POSTGRES_INSTANCE_NAME_QUERY_VARIABLE=dag_config["gcsql_postgres_instance_name_query"]
GCSQL_POSTGRES_DATABASE_NAME_VARIABLE=dag_config["gcsql_postgres_database_name"]
GCSQL_POSTGRES_USER_VARIABLE = dag_config["gcsql_postgres_user"]
GCSQL_POSTGRES_PASSWORD_VARIABLE=dag_config["gcsql_postgres_password"]
GCSQL_POSTGRES_PUBLIC_IP_VARIABLE=dag_config["gcsql_postgres_public_ip"]
GCSQL_POSTGRES_PUBLIC_PORT_VARIABLE=dag_config["gcsql_postgres_public_port"](this part is optional! it is called "Airflow variables" and its mainly usage is for hiding sensitive data information)GCP_PROJECT_ID=os.environ.get('GCP_PROJECT_ID',GCP_PROJECT_ID_VARIABLE)
GCP_REGION = os.environ.get('GCP_REGION', GCP_REGION_VARIABLE)
GCSQL_POSTGRES_INSTANCE_NAME_QUERY=os.environ.get('GCSQL_POSTGRES_INSTANCE_NAME_QUERY', GCSQL_POSTGRES_INSTANCE_NAME_QUERY_VARIABLE)
GCSQL_POSTGRES_DATABASE_NAME=os.environ.get('GCSQL_POSTGRES_DATABASE_NAME', GCSQL_POSTGRES_DATABASE_NAME_VARIABLE)
GCSQL_POSTGRES_USER = os.environ.get('GCSQL_POSTGRES_USER',GCSQL_POSTGRES_USER_VARIABLE)GCSQL_POSTGRES_PASSWORD = os.environ.get('GCSQL_POSTGRES_PASSWORD', GCSQL_POSTGRES_PASSWORD_VARIABLE)GCSQL_POSTGRES_PUBLIC_IP = os.environ.get('GCSQL_POSTGRES_PUBLIC_IP', GCSQL_POSTGRES_PUBLIC_IP_VARIABLE)GCSQL_POSTGRES_PUBLIC_PORT = os.environ.get('GCSQL_POSTGRES_PUBLIC_PORT', GCSQL_POSTGRES_PUBLIC_PORT_VARIABLE)
```

要了解更多信息，我推荐以下指南:

```
SQL = [
    'CREATE TABLE IF NOT EXISTS TABLE_TEST (I INTEGER)',
    'CREATE TABLE IF NOT EXISTS TABLE_TEST (I INTEGER)',
    'INSERT INTO TABLE_TEST VALUES (0)',
    'CREATE TABLE IF NOT EXISTS TABLE_TEST2 (I INTEGER)',
    'DROP TABLE TABLE_TEST',
    'DROP TABLE TABLE_TEST2',
]default_args = {
    'email': ['enter your email',],
    'email_on_retry': False,
    'email_on_failure': False,
    'retries': 1,
    'retry_delay': timedelta(minutes=5),
    'depends_on_past': False,
    'start_date': days_ago(1),
}HOME_DIR = expanduser("~")def get_absolute_path(path):
    *if* path.startswith("/"):
        *return* path
    *else*:
        *return* os.path.join(HOME_DIR, path)postgres_kwargs = dict(
    user=quote_plus(GCSQL_POSTGRES_USER),
    password=quote_plus(GCSQL_POSTGRES_PASSWORD),
    public_port=GCSQL_POSTGRES_PUBLIC_PORT,
    public_ip=quote_plus(GCSQL_POSTGRES_PUBLIC_IP),
    project_id=quote_plus(GCP_PROJECT_ID),
    location=quote_plus(GCP_REGION),
    instance=quote_plus(GCSQL_POSTGRES_INSTANCE_NAME_QUERY),
    database=quote_plus(GCSQL_POSTGRES_DATABASE_NAME),
)
```

*   这里找到的都应该是字符串，像这样("")，除了 public_port = 5432 是数字。

GCSQL_POSTGRES_USER 是云中的 SQL 用户。GCSQL_POSTGRES_PASSWORD 是同一用户的密码。CSQL_POSTGRES_PUBLIC_IP 是 SQL 云的公共 IP 地址。GCP 地区是您选择的“位置”。例如，“美国中心 1”。GCP 项目标识是我们已经提到的项目标识。GCSQL _ POSTGRES _ INSTANCE _ NAME _ QUERY 是 SQL cloud 实例的**原始名称**，也就是您在更改前赋予它的名称。GCSQL_POSTGRES_DATABASE_NAME 是云中的 SQL 数据库。GCSQL_POSTGRES_PUBLIC_PORT 是 5432。

```
os.environ['AIRFLOW_CONN_PROXY_POSTGRES_TCP'] = \
    "gcpcloudsql://{user}:{password}@{public_ip}:{public_port}/{database}?" \
    "database_type=postgres&" \
    "project_id={project_id}&" \
    "location={location}&" \
    "instance={instance}&" \\
    "use_proxy=True&" \
    "sql_proxy_use_tcp=True".format(**postgres_kwargs)connection_names = [
    "proxy_postgres_tcp",
]dag = DAG(
    'con_SQL',
    default_args=default_args,
    description='A DAG that connect to the SQL server.',
    schedule_interval=timedelta(days=1),
)def print_client(ds, **kwargs):
    client = storage.Client()
    print(client)print_task = PythonOperator(
    task_id='print_the_client',
    provide_context=True,
    python_callable=print_client,
    dag=dag,
)*for* connection_name *in* connection_names:
    task = CloudSqlQueryOperator(
         gcp_cloudsql_conn_id=connection_name,
         task_id="example_gcp_sql_task_" + connection_name,
         sql=SQL,
         dag=dag
    )
print_task >> task
```

总之:

有两项任务:

第一个任务是 task_id="print_the_client "，第二个任务是 task _ id = " example _ GCP _ SQL _ task _ "+connection _ name，称为" example _ GCP _ SQL _ task _ proxy _ postgres _ TCP "。

“print_the_client”是一个简单的任务，它仅打印客户端详细信息，以查看 DAG 是否正常工作，但它也可以是任何其他简单且容易工作的任务。

如果“print_the_client”不工作，请解决问题以继续第二项任务。这可能就是“example _ GCP _ SQL _ task _ proxy _ postgres _ TCP”不会工作的原因(DAG 结构的问题)。

“example _ GCP _ sql _ task _ proxy _ postgres _ TCP”连接到 SQL cloud 并执行我们的 SQL。这就是我们在这里的原因。

太好了！我们制作了文件，现在我们需要将它上传到 Airflow 服务器。👌

# Airflow 服务器

[](https://cloud.google.com/composer/docs/how-to/accessing/airflow-web-interface) [## Airflow web 界面| Cloud Composer |谷歌云

### Apache Airflow 包括一个 web 界面，您可以使用它来管理工作流(Dag)、管理气流环境…

cloud.google.com](https://cloud.google.com/composer/docs/how-to/accessing/airflow-web-interface) 

进入“导航菜单”->“Composer”，选择 DAGs 文件夹下的链接(会带你去云存储，给你看一个我们已经连接过的桶)。

请选择“上传文件”，并上传新创建的 file.py。返回“Composer”并选择 Airflow 服务器下的链接。

等待大约 1–2 分钟，刷新页面，您将看到您的 DAG 名称“con_SQL”。

输入您的 Dag 并“触发 DAG”来运行您的代码。

不断刷新页面，看看有没有变化。对于每个问题，选择任务的方格并“查看日志”。

任务完成！👏🙌🎉

请继续关注更新和您可能需要的任何其他信息。请对流程中出现的任何问题发表评论。

祝你好运！

# 感谢:

[](/@ariklevliber/connecting-to-gcp-composer-tasks-to-cloud-sql-7566350c5f53) [## 将 GCP 合成器任务连接到云 SQL

### 从本地开发机器和 GCP Composer 连接到 GCP SQL 实例所需的步骤

medium.com](/@ariklevliber/connecting-to-gcp-composer-tasks-to-cloud-sql-7566350c5f53) [](/@diggingfork/how-to-connect-cloud-sql-on-cloud-composer-via-proxy-31202191a55c) [## 如何通过代理连接 Cloud Composer 上的云 SQL

### 有些情况下，我们希望在 Cloud Composer 的 PythonOperator 中转换数据，然后连接并抛出…

medium.com](/@diggingfork/how-to-connect-cloud-sql-on-cloud-composer-via-proxy-31202191a55c) 

它们都是解决这种连接问题的主要场所。所以谢谢大家！