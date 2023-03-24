# 带有 Vue.js 和 GO 的社交应用

> 原文：<https://medium.com/nerd-for-tech/social-application-with-vue-js-and-go-64978f7c381f?source=collection_archive---------2----------------------->

![](img/4cdfb643ef90df60dcd8aaa1429ba169.png)

标志归功于 vuejs.org 和 golang.org

## 使用 vue.js 和 golang 创建和服务一个类似 twitter 的应用程序第 8 部分:基于令牌的认证

这是本系列的第八部分。在这里检查所有零件:

*   [第一部分:设置](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-4e4db0cdde64)
*   [第二部分:VUE 入门](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-64b3adee8dac)
*   [第三部分:组件&插槽](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-24a1d1e7137d)
*   [第 4 部分:Vuex 首次设置](/nerd-for-tech/social-application-with-vue-js-and-go-3a11d506fc38)
*   [第 5 部分:Vuex 终结](/nerd-for-tech/social-application-with-vue-js-and-go-ef364b572422)
*   [第六部分:Vuex 中的表格和数据](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-a22a1afb76eb)
*   [第七部分:与 golang 服务器的连接](/nerd-for-tech/social-application-with-vue-js-and-go-d9e563466b66)
*   第 8 部分:基于令牌的认证(This)
*   [第 9 部分:存储索引为 DB 的认证令牌](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-4d0caa37ddac)

在本课中，我们将在客户端 vue 应用程序和 go 服务器中添加一个身份验证层。另外，我们还将添加端点来获得单个用户，该用户将用于 vue 页面中的“用户”页面。

我们将在本次会议中更新前端和后端，您可以在这里找到代码:

*   [后端](https://github.com/idalmasso/go-vue-tutorial-backend/releases/tag/v0.8)
*   [前端](https://github.com/idalmasso/go-vue-tutorial-frontend/releases/tag/v0.8)

## 管理简单的基于令牌的身份验证

让我们开始添加一个文件，我们将在其中管理关于身份验证的路由。添加一个文件“*/endpoints/auth . go”*。在这里，我们将为用户插入注册、登录和注销操作的所有逻辑。

密码散列的生成和检查将使用标准库*bcrypt*(“*golang.org/x/crypto/bcrypt*”)来完成，而对于 jwt 令牌的创建和检查，我们将使用*dgrijalva/jwt-go*(“*github.com/dgrijalva/jwt-go*”)，因此只需导入它们。

请注意，令牌的创建需要使用服务器端机密，因此我们可以创建一个函数来从环境变量中获取它，如下所示(在" *endpoints/utils.go* "):

```
func getSecret() string{
 secret:=os.Getenv("ACCESS_SECRET")
 if secret==""{
  //That's surely a big secret this way...
  secret="sdmalncnjsdsmf"
 }
 return secret
}
```

很明显，如果这个秘密是空的，一个真实的服务器应该会在我们的真实世界中惊慌失措并抛出一个错误，但是这对于我们现在的情况来说是可以的。

现在，在 auth.go 中创建一个结构，用于将用户名/密码数据传递到端点，以便创建用户和登录:

```
type User struct{
  Username string `json:"username"`
  Password string `json:"password"`
}
```

然后创建两个端点(注意，我们还创建了一个用户数组作为“内存中”数据库，现在)和一个创建新令牌的方法:

```
var users map[string][]byte = make(map[string][]byte)
var idxUsers int =0

//getTokenUserPassword returns a jwt token for a user if the //password is ok
func getTokenUserPassword(w http.ResponseWriter, r *http.Request) {
  var u User
  err:=json.NewDecoder(r.Body).Decode(&u)
  if err!=nil{
    http.Error(w, "cannot decode username/password struct",http.StatusBadRequest)
    return
  }
  //here I have a user!
  //Now check if exists 
  passwordHash, found:= users[u.Username]
  if !found{
    http.Error(w, "Cannot find the username", http.StatusNotFound)
  }	
  err=bcrypt.CompareHashAndPassword(passwordHash, []byte(u.Password))
  if err!=nil{
    return
  }
  token, err:=createToken(u.Username)
  if err!=nil{
    http.Error(w, "Cannot create token", http.StatusInternalServerError)
    return
  }
  sendJSONResponse(w, struct {Token string `json:"token"`}{ token })	
}

func createUser(w http.ResponseWriter, r *http.Request){
  var u User
  err := json.NewDecoder(r.Body).Decode(&u)
  if err!=nil{
    http.Error(w, "Cannot decode request", http.StatusBadRequest)
    return
  }
  if _, found:= users[u.Username]; found{
    http.Error(w,"User already exists", http.StatusBadRequest)
    return
  }
  //If I'm here-> add user and return a token
  value, err := bcrypt.GenerateFromPassword([]byte(u.Password), bcrypt.DefaultCost)
  users[u.Username]=value
  token, err:=createToken(u.Username)
  if err!=nil{
    http.Error(w, "Cannot create token", http.StatusInternalServerError)
    return
  }	
  sendJSONResponse(w, struct {Token string `json:"token"`}{ token })
}func createToken(username string) (string, error) {
  var err error
  //Creating Access Token  
  atClaims := jwt.MapClaims{}
  atClaims["authorized"] = true
  atClaims["username"] = username
  atClaims["exp"] = time.Now().Add(time.Minute * 15).Unix()
	at := jwt.NewWithClaims(jwt.SigningMethodHS256, atClaims)
	secret:= getSecret()
  token, err := at.SignedString([]byte(secret))
  if err != nil {
     return "", err
  }
  return token, nil
}
```

“createToken”函数获取一个字符串 username，并创建一个带有一些声明的令牌，例如，该令牌应该完全由该用户使用，过期时间为 15 分钟。然后，该函数返回一个使用应用程序秘密创建的令牌，该令牌可用于 15 分钟的身份验证(但我们仍需实现这一点)。

其他两个函数实际上非常相似:都将请求体解码为我们之前创建的 json 对象类型，然后 *createUser* 尝试将其添加到“数据库”中，而 *getTokenUserPassword* 实际上检查用户是否存在。然后，如果所有这些检查都成功返回，这些函数将返回一个带有新创建的令牌的“JSONified”对象。

这两个函数实际上是站点登录和注册函数的处理程序，所以只需在 utils.go 文件的 *AddRouterEndpoints* 函数中添加下面两行，将它们绑定到实际的路由:

```
r.HandleFunc("/api/auth/login",
             getTokenUserPassword).Methods("POST") r.HandleFunc("/api/auth/create-user", createUser).Methods("POST")
```

我们现在有了一种实际注册和登录用户的方法。这其实是没有用的，直到我们屏蔽了服务器端的某些部分的访问。

让我们解决这个问题，将这两个方法添加到 *utils.go* 文件中:

```
func checkTokenHandler(next http.HandlerFunc) http.HandlerFunc{
  return func(w http.ResponseWriter, r *http.Request) {
    header := r.Header.Get("Authorization")
    bearerToken := strings.Split(header, " ")
    if len(bearerToken)!=2{
      http.Error(w, "Cannot read token", http.StatusBadRequest)
      return
    }
    if bearerToken[0] != "Bearer"{
      http.Error(w, "Error in authorization token. it needs to be in form of 'Bearer <token>'", http.StatusBadRequest)
      return
    }		
    token, ok :=checkToken(bearerToken[1]); 
    if !ok{
      http.Error(w, "Unauthorized", http.StatusUnauthorized)
      return
    }
    claims, ok := token.Claims.(jwt.MapClaims)
    if ok && token.Valid {	
      username, ok := claims["username"].(string)
      if !ok {
        http.Error(w, "Unauthorized", http.StatusUnauthorized)
        return 
      }
      //check if username actually exists
      if _, ok := users[username]; !ok{
        http.Error(w, "Unauthorized, user not exists", http.StatusUnauthorized)
        return 
      } 
      //Set the username in the request, so I will use it in check after!
      context.Set(r, "username", username)
    }
    next(w, r)
  }
}

func checkToken (tokenString string) (*jwt.Token, bool) {
  token, err := jwt.Parse(tokenString, func(token *jwt.Token) (interface{}, error) {
    if _, ok := token.Method.(*jwt.SigningMethodHMAC); !ok {
      return nil, fmt.Errorf("unexpected signing method: %v", token.Header["alg"])
    }
    return []byte(getSecret()), nil
  })
  if err!=nil{
    return nil, false
  }
  if _, ok := token.Claims.(jwt.Claims); !ok && !token.Valid {
    return nil, false
  }
  return token, true
}
```

函数获取一个字符串令牌作为输入，并返回一个 jwt。验证了其默认值后，返回令牌对象。

相反， *checkTokenHandler* 返回一个实际的 http 处理函数，它可以在路由的实际处理程序“之前”使用。在这个处理程序中，它从"*授权*"报头中获取令牌，它可以很容易地检查这个报头的格式(它必须是"无记名<令牌>")。然后，它得到 jwt。来自 *checkToken* 函数的令牌对象，并对数据库中令牌中用户名的存在进行额外的控制。然后，它在请求的上下文中设置用户名，因此我们也可以在实际请求中使用它，并调用我们想要调用的实际 http 处理程序，该处理程序被传递给 *checkTokenHandler* 函数。

请注意，如果任何检查失败，则不会调用真正的路由，并且会向客户端返回一个错误。我们现在只需使用这种方法，在 *addRouterEndpoints* 中修饰所有我们想要通过认证过程“保护”的路由，以获得一些保护:

```
func AddRouterEndpoints(r *mux.Router) *mux.Router {
...
  r.HandleFunc("/api/posts",checkTokenHandler(addPost))
    .Methods("POST")
  r.HandleFunc("/api/posts/{POST_ID}",checkTokenHandler(deletePost))
    .Methods("DELETE")
  r.HandleFunc("/api/posts/{POST_ID}/comments",
    checkTokenHandler(addComment)).Methods("POST")
...
```

这三个 api 现在实际上受到令牌认证保护。

用户将不得不使用这个 api 发送一个包含有效用户的令牌，以便实际添加帖子。这里还有一个小问题，如果一个用户“A”用作者用户“B”发送了一个帖子，系统仍然接受它，所以让我们也做这个小修正，在 posts.go 文件中，添加以下函数:

```
func isUsernameContextOk(username string, r *http.Request) bool {
	usernameCtx, ok:=context.Get(r, "username").(string)
	if !ok{
		return false
	}
	if usernameCtx!=username{
		return false
	}
	return true
}
```

这只是从请求中获取用户名，并将其与我们想要检查的用户名进行比较。我们可以在帖子(或评论)的反序列化之后，在“ *addPost* ”、“ *deletePost* ”和“ *addComment* ”这三个函数中调用这个函数，以检查帖子的用户是否是请求中的实际用户，如下所示:

```
if !isUsernameContextOk(post.Username, r){
  http.Error(w, "Cannot manage post for another user", http.StatusUnauthorized)
  return 
}
```

关于身份验证，可以做的最后一件事是提供一种方法，如果令牌快要过期，可以通过使用旧令牌获取新令牌来刷新令牌:

```
func getTokenByToken(w http.ResponseWriter, r *http.Request){
  //Here I already have the token checked... Just get the username from Request context
  username, ok :=context.Get(r,"username").(string)
  if !ok{
    http.Error(w, "Cannot check username",  http.StatusInternalServerError)
    return
  }
  token, err:=createToken(username)
  if err!=nil{
    http.Error(w, "Cannot create token", http.StatusInternalServerError)
    return
  }
  sendJSONResponse(w, struct {Token string}{ token })
}
```

并在 *addRouterEndpoints* 中实际绑定它

```
r.HandleFunc("/api/auth/token",checkTokenHandler(getTokenByToken))
  .Methods("GET")
```

显然，这实际上不是真实世界的场景。实际上，这应该与双令牌身份验证/授权风格一起使用，因此一个具有长到期时间的令牌用于身份验证，另一个令牌用于获得具有短到期时间的授权令牌。这只是关于如何做的一段演示性代码。

作为本教程章节的最后一件事，我们可以添加一个受令牌认证保护的 api 端点，以获取用户数据显示在用户页面中。

只需添加一个新文件' */endpoints/users.go* ，并在那里设置端点的代码:

```
func getUser(w http.ResponseWriter, r *http.Request) {
  log.Println("getuser called")
  vars := mux.Vars(r)
  user, ok := vars["USERNAME"]
  if !ok {
    http.Error(w, "Cannot find username in request",http.StatusBadRequest)
    return
  }
  if _, ok :=users[user]; ok{
    sendJSONResponse(w, 
    struct{Username string `json:"username"`; 
       Description string `json:"description"` }{user ,  ""})
    return
  }
  http.Error(w, "Cannot find user", http.StatusNotFound)
}
```

并使用我们之前使用的修饰处理程序来检查 *addRouterEnpoints* 函数中的认证

```
r.HandleFunc("/api/users/{USERNAME}",
  checkTokenHandler(getUser)).Methods("GET")
```

现在我们必须更新客户端 vue 应用程序来使用这些新功能。

## 客户端应用程序中的身份验证

让我们开始更新客户机应用程序，以便在服务器上使用身份验证。首先编辑"*store/authStore/index . js*"文件，删除实际的默认用户名，并在状态中添加一个新的令牌空字符串。然后，我们将不得不在登录和注销突变中操纵这两种状态，如下所示:

```
LOGIN(state, { username, token }) {
   state.user.loggedIn = true;
   state.user.username = username;
   state.user.token = token;
},
LOGOUT(state) {
   state.user.loggedIn = false;
   state.user.username = "";
   state.user.token = "";
}
```

然后，我们只需更新实际操作，并确保调用正确的 API(注意:注销操作实际上不会使服务器端的令牌无效，事实上，我们现在并没有管理服务器中的“令牌状态”,所以它只是从内存中删除令牌，并在客户端应用程序中清理用户)

```
async login(context, { username, password }) {
    return fetch("http://localhost:3000/api/auth/login", {
      method: "POST",
      body: JSON.stringify({
                             username: username, 
                             password: password })
      })
      .then(response => {
        if (!response.ok) {
          throw new Error("Cannot login!");
        }
        return response.json();
      }).then(data => {
         context.commit("LOGIN",
              { username: username, token: data.token });
      }).catch(error => {
          context.commit("LOGOUT");
          throw error;
        });
    },
async logout(context) {
    context.commit("LOGOUT");
},
async signup(context, { username, password }) {
    return fetch("http://localhost:3000/api/auth/create-user", {
      method: "POST",
      body: JSON.stringify(
          { username: username, password: password })
      }).then(response => {
        if (!response.ok) {
          throw new Error("Cannot signup!");
        }
        return response.json();
      }).then(data => {
        context.commit("LOGIN", 
              { username: username, token: data.token });
      }).catch(error => {
        context.commit("LOGOUT");
        error.read().then((data, done) => {
            throw Error(data);
        });
      });
    }
```

请注意，现在任何操作都调用正确的 API，如果成功，并且响应正常，它将使用调用返回的数据调用正确的变异，否则将抛出一个错误(因此操作的调用者可以相应地采取行动)。

最后，添加一个额外的 getter，用于其他商店中的其他 api:

```
getTokenHeader(state) {
  return "Bearer " + state.user.token;
}
```

使用这个 getter，其他商店将能够为调用设置正确的令牌。让我们对 posts store 这样做:这实际上非常简单，在我们保护的三个 api 中，只需更新 fetch 的 headers 参数，如下所示:

```
headers: {
     "Content-Type": "application/json",
     Authorization: context.rootGetters["auth/getTokenHeader"]        },
```

现在让我们也更新用户存储，删除状态中的 users 数组的初始化，并创建一个可以实际调用 ADD_USER 变异的操作

```
state: {
    loadedUsers: []
  },
  .....
  actions: {
    async addUser(context, { username }) {
      return fetch("http://localhost:3000/api/users/" + username, {
        headers: {
          Authorization: context.rootGetters["auth/getTokenHeader"]
        }
      })
        .then(response => {
          if (!response.ok) throw new Error("Cannot get user");
          return response.json();
        })
        .then(data => {
          context.commit("ADD_USER", data);
        })
        .catch(error => {
          console.log(error);
          throw error;
        });
    }
```

这实际上与我们对帖子所做的一样。

这些更新将自行工作，几乎不需要对组件逻辑进行更新。现在，让我们将登录页面更改为实际的登录/注册工作页面。像这样更新 *views/Login.vue* 组件:

```
<template>
  <div class="content">
    <base-card>
      <form @submit.prevent>
        <label for="username">Username</label>
        <input id="username" type="text" v-model="username" />
        <label for="password">Password</label>
        <input id="password" type="password" v-model="password" />
        <button @click="loginButtonClicked">{{ buttonString }}</button>
        <p v-if="error">{{ error }}</p>
        <a href="#" @click.prevent="toggleLogin">{{ textLoginString }}</a>
      </form>
    </base-card>
  </div>
</template>

<script>
import { mapActions } from "vuex";
import BaseCard from "../components/UI/BaseCard.vue";
export default {
  components: { BaseCard },
  data() {
    return {
      loginSelected: true,
      username: "",
      password: "",
      error: ""
    };
  },
  methods: {
    ...mapActions({ login: "auth/login", signup: "auth/signup" }),
    loginButtonClicked() {
      if (this.loginSelected) {
        this.login({ username: this.username, password: this.password })
          .then(() => {
            this.$router.push({ name: "Posts" });
          })
          .catch(error => {
            this.error = error;
          });
      } else {
        this.signup({ username: this.username, password: this.password })
          .then(() => {
            this.$router.push({ name: "Posts" });
          })
          .catch(error => {
            this.error = error;
          });
      }
    },
    toggleLogin() {
      this.loginSelected = !this.loginSelected;
    }
  },
  computed: {
    buttonString() {
      if (this.loginSelected) {
        return "LOGIN";
      } else {
        return "SIGNUP";
      }
    },
    textLoginString() {
      if (this.loginSelected) {
        return "Signup instead";
      } else {
        return "Login instead";
      }
    }
  }
};
</script>
```

该页面将显示一个登录或注册请求，我们将把这个选择存储在由“toggleLogin”方法设置的“loginSelected”变量中。登录和注册操作实际上与 mapActions 绑定在一起，按钮单击事件实际上触发了其中一个，查看实际的“loginSelected”值。现在，当用户注册时，实际的应用程序身份验证存储调用 API 并创建新用户。这同样适用于登录。

在 User.vue 视图中，我们可以调用组件挂接中的 addUser:

```
mounted() {
    this.$store.dispatch("users/addUser", {username: this.userid});  
}
```

当组件以用户 id 打开时，它将调用 api 获取该用户的详细信息，并将其添加到存储中。

同样，在 SinglePost 组件中，只需更新 delete 按钮，使其只显示经过身份验证的实际用户的帖子，如下所示:

```
<button        
    v-if="loggedIn && currentUser.username === post.username"
    class="delete-button"
    @click.prevent="deletePost">
```

最后一个更新，让我们稍微改变一下应用程序栏，这样当注销按钮被点击时，它会自动重定向到登录页面:

```
logoutButtonClicked() {
      this.logout().then(() => {
        this.$router.push({ name: "Login" });
      });
    }
```

现在应用程序开始成形，任何用户都可以注册，添加一些帖子，然后删除自己的帖子，并看到其他用户。此外，我们还设置了一些条件，当用户可以执行一些操作时，以及当他没有被授权时(即，我们决定任何人都可以看到帖子列表，但只有登录的用户可以看到其他用户)。

请注意，实际上每 15 分钟就会有一个用户被注销，解决这个问题的一个好方法是在客户端中定期自动更新令牌。

下一个重要步骤是实际使用数据库而不是服务器端的阵列来存储我们的数据，然后我们的应用程序将几乎完全正常工作，但在此之前，我们将在用户浏览器的 indexedDB 中保存身份验证数据。