# 在多个帐户中为一个 IAM 用户轮换 cloud bearing 访问密钥！

> 原文：<https://medium.com/nerd-for-tech/rotate-cloudendure-access-keys-for-an-iam-user-in-multiple-accounts-b21563d23d0d?source=collection_archive---------0----------------------->

## 一个完全自动化的指南+ Lambda 代码，用于在多个 AWS 帐户之间轮换 IAM 密钥。

![](img/5eb7830a6d09e7e7f521a352e8e51b7a.png)

[https://unsplash.com/@brookelark](https://unsplash.com/@brookelark)

## 背景

如果您在 EC2 上运行需要访问 AWS 服务的应用程序，AWS 强烈建议定期更改访问密钥(由访问密钥 ID 和秘密访问密钥组成)。这是一个众所周知的安全最佳实践，因为它缩短了访问密钥的有效时间，因此减少了它们被破坏时的业务影响。

因此，请定期更换访问密钥，并确保您帐户中的所有 IAM 用户都这样做。这样，如果访问密钥在您不知情的情况下遭到破坏，您就限制了凭据可用于访问您的资源的时间。您还可以选择他们必须这样做的频率。

也就是说，角色使用自动过期和自动更新的临时安全凭证，因此您不必担心访问密钥轮换——AWS 会为您完成这项工作。**但是，如果您在 EC2 之外的地方运行应用程序，您应该将访问密钥轮换添加到您的应用程序管理过程中。**

这就是麻烦的开始。因为要为一个 EC2 手动执行此操作，您需要遵循以下步骤:

1.  除了正在使用的访问密钥之外，创建第二个访问密钥。
2.  更新您的所有应用程序以使用新的访问密钥，并验证应用程序正在工作。
3.  将先前访问键的状态更改为非活动状态。
4.  验证您的应用程序是否仍按预期工作。
5.  删除不活动的访问键。

想象一下，对 1000 个 ec2 和多个帐户执行上述所有操作！

**自动化就是答案！**

令人欣慰的是，cloud bearing 提供了 API 来编排创建、列出和更新现有键的整个过程。这个文档[这里](https://console.cloudendure.com/api_doc/apis.html)有详细的东西解释。这些 API 托管在 web 服务器上，以 JSON 格式存储所有数据。

**代码在哪里？**

在我们进入编码部分之前，让我们先来看看我是如何实现的。我用 postman 做了几天的实验，才得出最终的解决方案。从**开始，在**登录:

```
session = requests.Session()
session.headers.update({'Content-type': 'application/json', 'Accept': 'text/plain'})login_data = {'userApiToken': api_token}
resp = session.post(url=HOST+ENDPOINT.format('login'),
data = json.dumps(login_data))
```

我使用请求库中的会话来持久化 cookies，这样我就可以再次使用同一个会话进行多次请求。通过这样做，我们可以持久化令牌

```
session.headers['X-XSRF-TOKEN'] = session.cookies.get('XSRF-TOKEN')
```

因此，为了更新和管理两个不同的控制台，我用登录 api 创建了两个单独的会话，如下所示。

```
a_session = _login(userapi_token['a'])
b_session = _login(userapi_token['b'])
```

然后使用 **cloudCredentials** 来获取每个 cloud bearing 项目的 creds id。

```
url = "https://console.cloudendure.com/api/latest/cloudCredentials"
a_response = a_session.get(url).json()['items']
b_response = b_session.get(url).json()['items']for item in a_response:
    item.update({"ce_session":'a'})for item in b_response:
    item.update({"ce_session":'b'})return a_response + b_response
```

但是想法是将它们合并在一起，创建一个单一的 json。为了稍后识别哪个会话将用于更新，我添加了一个名为 **ce_session** 的额外键来识别它。

最初的 json 结果是这样的:

```
[
            {
                "accountIdentifier": "xxx1",
                "cloud": "xxx-xxx-xxx",
                "id": "abcde",
                "publicKey": "XXXX"
            },
            {
                "accountIdentifier": "xxx2",
                "cloud": "xxx-xxx-xxx",
                "id": "vwxyz",
                "publicKey": "YYYY"
            }
]
```

请注意，云 id 在此范围内保持不变，因为我只使用了 AWS 帐户。

**账户标识符**是账户 id。
**云**是分配给云的 id。AWS/AZURE/GCP

有了这些信息后，我创建了一个需要访问的 AWS 帐户列表。

下一步是创造新的信用。为此，我循环了之前的 AWS 帐户列表，并为每个帐户创建了一个会话。请注意，这是一个 **boto3** 会话。

在这个方法的第一部分，对于每个帐户，我列出了 IAM 密钥，检查它们是否超过 90 天，删除它们，并创建新的。

```
aws_creds = {}
    new_creds = []
    failed_accounts = []
    for account in accounts_in_ce:
        aws_session = create_session(account)
        user = 'YourUserName'
        user_iam_details, iam_client=list_access_key(aws_session, user)
        if user_iam_details != None:
            for _ in user_iam_details:
                if _['days'] >= 90:
                    temp_creds_item = {}
                    disable_key(iam_client=iam_client, access_key = _['AccessKeyId'], username=_['UserName'])
                    delete_key(iam_client=iam_client, access_key = _['AccessKeyId'], username=_['UserName'])
                    access_key, secret_access_key = create_key(iam_client=iam_client, username=_['UserName'])
                    temp_creds_item['access_key'] = access_key
                    temp_creds_item['secret_access_key'] = secret_access_key
                    aws_creds[account]=temp_creds_item
```

所有这些新的凭证都存储在一个 dict 结构中，以便进一步访问。

下一部分将它们添加回 cloud bearing 的早期 json 中，以保持内容的一致性。

```
for item in ce_creds:
        temp_item = {}
        temp_item['accountIdentifier'] = item['accountIdentifier']
        temp_item['cloud'] = item['cloud']
        temp_item['id'] = item['id']
        temp_item['old_access_key'] = item['publicKey']
        temp_item['ce_session'] = item['ce_session']
        try:
            temp_item['access_key'] = aws_creds[item['accountIdentifier']]['access_key']
            temp_item['secret_access_key'] = aws_creds[item['accountIdentifier']]['secret_access_key']
            new_creds.append(temp_item)
        except:
            print("Possible Key Error. Check List Access Keys for {}".format(item['accountIdentifier']))
            failed_accounts.append(item['accountIdentifier'])
            continue

return new_creds, list(set(failed_accounts))
```

现在，我有了**新的信用记录**和一组**失败的账户**，我可以将它们报告给另一个团队进行纠正。

因此，有了新的信用凭证，我这样做了:

```
for item in new_creds:
        if item['ce_session']=='a':
            set_cloudEndureCredentials(a_session, item['access_key'], item['secret_access_key'], item['id'], item['cloud'])
        if item['ce_session']=='b':
            set_cloudEndureCredentials(b_session, item['access_key'], item['secret_access_key'], item['id'], item['cloud'])
```

因为在更新两个不同的控制台时，它需要通过正确的会话。这就是我添加会话标识符的原因。( **ce_session** )在代码的前面。

set_cloudEndureCredentials 使用补丁请求来更新 Cloud Endure Console 中的 creds。

```
creds = {
        "cloudId":cloud_id,
        "privateKey":base64.b64encode(bytes(private_key, 'utf-8')).decode('utf-8'),
        "publicKey":public_key,
        "id":creds_id
    }

    resp = session.patch(url=HOST+ENDPOINT.format('cloudCredentials/{}'.format(creds_id)), data=json.dumps(creds))

    if resp.status_code != 200:
        print('Failed setting credentails for project. Creds id - {}'.format(id))
    else:
        print('Credentials for project were set successfully')
```

这个请求需要 creds_id 作为端点和有效负载数据。使用保持不变的 **CloudId** 、作为秘密访问密钥的 **privateKey** 、 **publicKey** 和 **id** 即 creds_id 来构造有效载荷数据。

就是这样。如果您计划每 30 天使用一次，这将自动为 CloudEndure 的多个帐户轮换 IAM 密钥。

我知道 a 使用了很多小函数，我到现在还没有解释，因为它们是不言自明的。下面是完整的 lambda 代码。

**大码**

```
import boto3
from boto3.session import Session
from botocore.exceptions import ClientError
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
import datetime
import json
import requests
import base64HOST = '[https://console.cloudendure.com'](https://console.cloudendure.com')
ENDPOINT = '/api/latest/{}'def list_access_key(session, user, secret_access_key=None):
    iam_client = session.client('iam')
    account_id = session.client('sts').get_caller_identity().get('Account')
    try:
        keydetails = iam_client.list_access_keys(UserName=user)
    except Exception as e:
        print("Not able to list keys for {}".format(account_id))
        print(e)
        return None, None

    key_details = {}
    user_iam_details = []
    # Some user may have 2 access keys. So iterating over them and listing the details of active access key.
    for keys in keydetails['AccessKeyMetadata']:
        days=time_diff(keys['CreateDate'])
        if days >= 0:
            key_details['UserName']=keys['UserName']
            key_details['AccessKeyId']=keys['AccessKeyId']
            key_details['SecretAccessKeyId']=secret_access_key
            key_details['days']=days
            key_details['status']=keys['Status']
            user_iam_details.append(key_details)
            key_details={}

    return user_iam_details, iam_clientdef time_diff(keycreatedtime):
    now=datetime.datetime.now(datetime.timezone.utc)
    diff=now-keycreatedtime
    return diff.daysdef create_key(iam_client, username):
    access_key_metadata = iam_client.create_access_key(UserName=username)
    access_key = access_key_metadata['AccessKey']['AccessKeyId']
    secret_key = access_key_metadata['AccessKey']['SecretAccessKey']
    return access_key,secret_keydef disable_key(iam_client, access_key, username):
    try:
        iam_client.update_access_key(UserName=username, AccessKeyId=access_key, Status="Inactive")
        print(access_key + " has been disabled.")
    except ClientError as e:
        print("The access key with id %s cannot be found" % access_key)def delete_key(iam_client, access_key, username):
    try:
        iam_client.delete_access_key(UserName=username, AccessKeyId=access_key)
        print (access_key + " has been deleted.")
    except ClientError as e:
        print("The access key with id %s cannot be found" % access_key)def _login(api_token):global ENDPOINTsession = requests.Session()
 session.headers.update({'Content-type': 'application/json', 'Accept': 'text/plain'})
    #  print ( 'Logging in...' )login_data = {'userApiToken': api_token}
 resp = session.post(url=HOST+ENDPOINT.format('login'),
      data=json.dumps(login_data))if resp.status_code != 200 and resp.status_code != 307:
  print ( 'Could not login!' )# check if need to use a different API entry point
 if resp.history:
  print ( 'URL Redirected...' )
  ENDPOINT = '/' + '/'.join(resp.url.split('/')[3:-1]) + '/{}'
  resp = session.post(url=HOST+ENDPOINT.format('login'),
      data=json.dumps(login_data))if session.cookies.get('XSRF-TOKEN'):
  session.headers['X-XSRF-TOKEN'] = session.cookies.get('XSRF-TOKEN')return sessiondef get_cloudEndureCredentials(a_session, b_session):
    url = "[https://console.cloudendure.com/api/latest/cloudCredentials](https://console.cloudendure.com/api/latest/cloudCredentials)"
    a_response = a_session.get(url).json()['items']
    b_response = b_session.get(url).json()['items']

    for item in a_response:
        item.update({"ce_session":'a'})for item in b_response:
         item.update({"ce_session":'b'})

    return a_response + b_response

def set_cloudEndureCredentials(session, public_key, private_key, creds_id, cloud_id):
    creds = {
        "cloudId":cloud_id,
        "privateKey":base64.b64encode(bytes(private_key, 'utf-8')).decode('utf-8'),
        "publicKey":public_key,
        "id":creds_id
    }

    resp = session.patch(url=HOST+ENDPOINT.format('cloudCredentials/{}'.format(creds_id)), data=json.dumps(creds))

    if resp.status_code != 200:
        print('Failed setting credentails for project. Creds id - {}'.format(id))
    else:
        print('Credentials for project were set successfully')def create_session(target_account_id):

    sts_client = boto3.client('sts')
    target_role_name = "target_role_name"

    resp = sts_client.assume_role(
        RoleArn=f"arn:aws:iam::{target_account_id}:role/{target_role_name}",
        RoleSessionName="CE"+str(target_account_id)
    )

    # Now we have a session under assumed role
    session = Session(
        aws_access_key_id=resp['Credentials']['AccessKeyId'],
        aws_secret_access_key=resp['Credentials']['SecretAccessKey'],
        aws_session_token=resp['Credentials']['SessionToken']
    )

    return sessiondef get_accounts(ce_creds_json):
    accounts = []
    for item in ce_creds_json:
        accounts.append(item['accountIdentifier'])

    return list(set(accounts))def create_new_creds(accounts_in_ce, ce_creds):
    aws_creds = {}
    new_creds = []
    failed_accounts = []
    for account in accounts_in_ce:
        aws_session = create_session(account)
        user = 'YourUserName'
        user_iam_details, iam_client=list_access_key(aws_session, user)
        if user_iam_details != None:
            for _ in user_iam_details:
                if _['days'] >= 90:
                    temp_creds_item = {}
                    disable_key(iam_client=iam_client, access_key = _['AccessKeyId'], username=_['UserName'])
                    delete_key(iam_client=iam_client, access_key = _['AccessKeyId'], username=_['UserName'])
                    access_key, secret_access_key = create_key(iam_client=iam_client, username=_['UserName'])
                    temp_creds_item['access_key'] = access_key
                    temp_creds_item['secret_access_key'] = secret_access_key
                    aws_creds[account]=temp_creds_item

    for item in ce_creds:
        temp_item = {}
        temp_item['accountIdentifier'] = item['accountIdentifier']
        temp_item['cloud'] = item['cloud']
        temp_item['id'] = item['id']
        temp_item['old_access_key'] = item['publicKey']
        temp_item['ce_session'] = item['ce_session']
        try:
            temp_item['access_key'] = aws_creds[item['accountIdentifier']]['access_key']
            temp_item['secret_access_key'] = aws_creds[item['accountIdentifier']]['secret_access_key']
            new_creds.append(temp_item)
        except:
            print("Possible Key Error. Check List Access Keys for {}".format(item['accountIdentifier']))
            failed_accounts.append(item['accountIdentifier'])
            continue

    return new_creds, list(set(failed_accounts))

def lambda_handler(event, context):
    userapi_token = {'a':'xxx', 'b':'yyy'}
    a_session = _login(userapi_token['a'])
    b_session = _login(userapi_token['b']) 

    cloud_endure_credentials = get_cloudEndureCredentials(a_session, b_session)

    all_accounts_in_ce = get_accounts(cloud_endure_credentials)    

    new_creds, failed_accounts = create_new_creds(all_accounts_in_ce, cloud_endure_credentials)

    for item in new_creds:
        if item['ce_session']=='a':
            set_cloudEndureCredentials(a_session, item['access_key'], item['secret_access_key'], item['id'], item['cloud'])
        if item['ce_session']=='b':
            set_cloudEndureCredentials(b_session, item['access_key'], item['secret_access_key'], item['id'], item['cloud'])
```

使用您需要的代码，sts 承担访问多个帐户的角色，允许列出、创建、删除和禁用访问密钥。

仅此而已。我希望你自己尝试一下，然后按下拍手键。如果您有任何问题，请随时写在下面。我会很乐意解决像我的其他职位。