# 带有 Vue.js 和 GO 的社交应用

> 原文：<https://medium.com/nerd-for-tech/social-application-with-vue-js-and-go-d9e563466b66?source=collection_archive---------1----------------------->

![](img/4cdfb643ef90df60dcd8aaa1429ba169.png)

标志归功于 vuejs.org 和 golang.org

## 用 vue.js 和 golang 创建和服务一个类似 twitter 的应用程序第 7 部分:与 GOLANG 服务器的连接

这是本系列的第七部分。在这里检查所有零件:

*   [第一部分:设置](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-4e4db0cdde64)
*   [第二部分:VUE 入门](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-64b3adee8dac)
*   [第三部分:组件&插槽](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-24a1d1e7137d)
*   [第 4 部分:Vuex 首次设置](/nerd-for-tech/social-application-with-vue-js-and-go-3a11d506fc38)
*   [第 5 部分:Vuex 终结](/nerd-for-tech/social-application-with-vue-js-and-go-ef364b572422)
*   [第六部分:Vuex 中的表格和数据](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-a22a1afb76eb)
*   第七部分:连接 golang 服务器(this)
*   [第 8 部分:基于令牌的认证](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-64978f7c381f)
*   [第 9 部分:存储索引为 DB 的认证令牌](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-4d0caa37ddac)

在这一课中，我们的简单应用程序开始最终与 golang 后端服务器进行对话，我们将开始在通信的前端和数据存储的后端编写逻辑。

我们将在本次会议中更新前端和后端，您可以在这里找到代码:

*   [后端](https://github.com/idalmasso/go-vue-tutorial-backend/releases/tag/v0.7)
*   [前端](https://github.com/idalmasso/go-vue-tutorial-frontend/releases/tag/v0.7)

## 在后端创建帖子逻辑

让我们开始在后端创建一些逻辑，使我们能够操纵帖子。创建一个文件夹“*<back end _ source _ folder>/endpoints*”(这实际上会翻译成创建一个同名的包)，并在其中创建两个文件: *utils.go* 和 *posts.go* 。

在 utils.go 文件中插入以下代码:

```
package endpoints;import (
 "encoding/json"
 "log"
 "net/http"
 "github.com/gorilla/mux"
)//AddRouterEndpoints add the actual endpoints for api
func AddRouterEndpoints(r *mux.Router) *mux.Router {
  r.HandleFunc("/api/posts", getPosts).Methods("GET")
  r.HandleFunc("/api/posts", addPost).Methods("POST")
  r.HandleFunc("/api/posts/{POST_ID}",deletePost)
      .Methods("DELETE")
  r.HandleFunc("/api/posts/{POST_ID}/comments",addComment)
      .Methods("POST")
  return r
}

func sendJSONResponse(w http.ResponseWriter, data interface{}) {
  body, err := json.Marshal(data)
  if err != nil {
    log.Printf("Failed to encode a JSON response: %v", err)
    w.WriteHeader(http.StatusInternalServerError)
    return
  }
  w.Header().Set("Content-Type", "application/json; charset=UTF-8")
  w.WriteHeader(http.StatusOK)
  _, err = w.Write(body)
  if err != nil {
    log.Printf("Failed to write the response body: %v", err)
    return
  }
}
```

这些函数非常容易理解:第一个是更新一个路由器，作为参数添加路由解析方法，用于添加、获取和删除帖子。此外，我们还添加了一个向其中插入注释的方法。我们使用 gorilla mux，它实际上是一个多路复用器，使管理路线的代码更加容易和有组织。

第一个函数的代码实际上只是说，如果我们的服务器的/api/posts 端点是用 http GET 方法调用的，那么它应该是带有 getPost 函数(我们还没有编写)的服务器，并且跟在其他函数后面。

第二个函数是一个助手函数，我们将在 routes 函数中使用。这将接受一个 http 作为输入。ResponseWriter 和一个空接口(和往常一样，它可以是 go 中的任何东西),然后用响应编写器编写对象，用 http OK 代码序列化为 json。注意，如果对象不是 json 可序列化的，它将返回一个错误。

有了这个设置，我们将在第二个文件中插入处理路由的实际函数。在文件的顶部，我们插入

```
package endpoints

import (
	"encoding/json"
	"log"
	"net/http"
	"strconv"
	"time"

	"github.com/gorilla/mux"
)
type comment struct {
	ID       int      `json:"id"`
	Username string   `json:"username"`
	Post string   	  `json:"post"`
	Date time.Time 	  `json:"date"`	
}
//post Struct is used as post structure...
type post struct {
	ID   		 int       `json:"id"`
	Username string    `json:"username"`
	Post string    		 `json:"post"`
	Date time.Time 		 `json:"date"`
	Comments []comment `json:"comments"`
} 
```

这是两个结构的定义，一个带有 name comment，另一个带有 name post，包含了 API 数据传输所需的字段。注意，字段名都是大写的，这是 json 序列化所需要的。我们现在不打算使用数据库，我们将在以后的课程中添加它，现在我们将创建一个帖子数组并在内存中使用它们:

```
var posts []post=make([]post, 0)
//need an index for the array... When I'll delete the posts the index will have to go on...
var index int=1 
```

在此之后，我们可以开始创建实际的处理函数:

```
//addPost will get in Body a post with ONLY username and post-> need to add the others and save it
func addPost(w http.ResponseWriter, r *http.Request) {
  log.Println("addPost called")
  var actualPost post
  err:=json.NewDecoder(r.Body).Decode(&actualPost)
  if err!=nil{
    http.Error(w, err.Error(), http.StatusBadRequest)
    return
  }
  actualPost.ID = index
  index++
  actualPost.Date=time.Now()
  if actualPost.Comments== nil{
    actualPost.Comments=make([]comment, 0)
  }
  posts=append(posts, actualPost)
  sendJSONResponse(w,actualPost)
}//deletePost removes the post that is being passed. Get the id from //the query
func deletePost(w http.ResponseWriter, r *http.Request) {
  log.Println("deletePost called")
  vars := mux.Vars(r)
  idString, ok := vars["POST_ID"]
  if !ok {
    http.Error(w, "Cannot find ID", http.StatusBadRequest)
    return
  }
  id, err := strconv.Atoi(idString)
  if err!=nil{
    http.Error(w, "Cannot convert the id value to string",http.StatusBadRequest)
    return
  }
  for i:=0;i<len(posts);i++{
    if posts[i].ID==id {
      posts[i]=posts[len(posts)-1]
      posts=posts[:len(posts)-1]
      w.WriteHeader(http.StatusOK)
      return
    }
  }
  http.Error(w, "Cannot find the requested id", http.StatusNotFound)
}
//addComment will get the comment in the body, and the id in the query
func addComment(w http.ResponseWriter, r *http.Request) {
  log.Println("addComment called")
  vars := mux.Vars(r)
  idString, ok := vars["POST_ID"]
  if !ok {
    http.Error(w, "Cannot find ID", http.StatusBadRequest)
    return
  }
  id, err := strconv.Atoi(idString)
  if err!=nil{
    http.Error(w, "Cannot convert the id value to string", http.StatusBadRequest)
    return
  }
  var actualComment comment
  err = json.NewDecoder(r.Body).Decode(&actualComment)
  if err!=nil{
    http.Error(w, err.Error(), http.StatusBadRequest)
    return
  }
  for i:=0;i<len(posts);i++{
    if posts[i].ID==id {
      //Now I have the post
      var commMax int=0
      for comm:=0;comm<len(posts[i].Comments);comm++{
        if commMax<posts[i].Comments[comm].ID{
          commMax=posts[i].Comments[comm].ID
        }
      }
      actualComment.ID=commMax+1
      actualComment.Date=time.Now()
      posts[i].Comments = append(posts[i].Comments, actualComment)
      sendJSONResponse(w, posts[i])
      return
    }
  }
  //If I'm here, there is no post with the id searched... 
  http.Error(w, "Cannot find a post with the selected id", http.StatusNotFound)
}
//getPosts will return all the posts actually in the array
func getPosts(w http.ResponseWriter, r *http.Request) {
  log.Println("Get post called")
  sendJSONResponse(w, posts)
}
```

让我们解释一下这段代码:

*   addPost 函数实际上只是尝试将请求体解码为 Post 对象。如果可以做到这一点，它会将这个对象(带有填充的 id 和日期)添加到我们的帖子“storage”中，然后返回它，更新后带有 ok 响应
*   deletePost 函数获取 POST_ID 查询参数，如果“存储”中有一篇具有该 ID 的文章，它将被删除。否则，它向客户端返回一个 NotFound 状态代码。
*   addComment 将一个 Comment 对象添加到由 POST_ID 查询参数指示的帖子中。事实上，查找 ID 的代码一点也不好，但是一旦我们开始使用数据库，我们就会替换它。
*   getPosts 只是将作为回答的帖子列表序列化为 json。

现在所有的对象都可以被调用了，最后要做的就是在服务器的主函数中调用之前在 utils 文件中编写的 *addRouterEndpoint* 方法。然后，为了管理 CORS 策略，我们只需编写一个简单的装饰器，在将它传递给 http 之前应用于路由器。手柄功能。简单来说就是这样做的:

```
func main(){
  r := mux.NewRouter()
  r=endpoints.AddRouterEndpoints(r)
  fs := http.FileServer(http.Dir("./dist"))
  r.PathPrefix("/").Handler(fs)

  http.Handle("/",&corsRouterDecorator{r})
  fmt.Println("Listening")	
  log.Panic(
    http.ListenAndServe(":3000", nil),
  )
}

type corsRouterDecorator struct {
  R *mux.Router
}

func (c *corsRouterDecorator) ServeHTTP(rw http.ResponseWriter, req *http.Request) {
  if origin := req.Header.Get("Origin"); origin != "" {
    rw.Header().Set("Access-Control-Allow-Origin", origin)
    rw.Header().Set("Access-Control-Allow-Methods", 
      "POST, GET, PUT, DELETE, PATCH")
    rw.Header().Add("Access-Control-Allow-Headers", 
      "Content-Type, Access-Control-Allow-Headers, Authorization, X-Requested-With")
  }
  // Stop here if its Preflighted OPTIONS request
  if req.Method == "OPTIONS" {
    return
  }
  c.R.ServeHTTP(rw, req)
}
```

就是这样！这样，我们就有了这个服务器创建和服务的 4 个想要的路由，然后它将操作帖子和评论对象。注意，在这一章中，我们不打算管理认证，所以任何将帖子发送到/api/posts 发布路径的用户都可以向我们的服务器添加新帖子。我们将在另一章中讨论这个问题。

## 更新前端

在前端，大部分工作将在存储操作中完成。请注意，我们在后端将“post.user”更改为“post.username ”,因此对于 semplicity，我们可以在任何地方进行更改。

然后，在 Post store 的 index.js 中，我们来做一些修改。首先，我们将不再有“ADD_COMMENT”变异，但是，因为路由返回给我们完整的 post 对象，我们可以有一个“SET_POST_COMMENTS”变异，如下所示:

```
SET_POST_COMMENTS(state, { postId, post }) {
  const oldPost = state.posts.find(post => post.id == postId);
  oldPost.comments = post.comments;
}
```

然后，ADD_POST 突变不再需要找 id，可以变得更简单。同样，让我们为初始化添加一个 SET_ALL_POSTS 变异:

```
ADD_POST(state, post) {
  state.posts.push(post);
},
SET_ALL_POSTS(state, posts) {
  state.posts = posts;
},
```

有了这个，我们改变并简化了我们的突变。我们现在可以更新动作来调用 api，并添加新的动作“getAllPosts”来完成我们需要的功能。这是通过以下代码完成的:

```
async addPost(context, post) {
      fetch("http://localhost:3000/api/posts", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(post)
      })
        .then(response => {
          if (!response.ok) {
            throw Error(response.body);
          }
          return response.json();
        })
        .then(data => {
          context.commit("ADD_POST", data);
        })
        .catch(error => {
          console.log(error);
        });
    },
    async deletePost(context, { post }) {
      fetch("http://localhost:3000/api/posts/" + post.id, {
        method: "DELETE"
      })
        .then(response => {
          if (response.ok) {
            console.log(response);
            context.commit("DELETE_POST", post.id);
            return;
          }
          throw Error(response);
        })
        .catch(error => {
          console.log(error);
        });
    },
    async addComment(context, { postId, comment }) {
      fetch("http://localhost:3000/api/posts/" + postId + "/comments", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(comment)
      })
        .then(response => {
          if (response.ok) {
            return response.json();
          } else throw Error(response.body);
        })
        .then(data => {
          context.commit("SET_POST_COMMENTS", { postId: postId, post: data });
        })
        .catch(error => {
          console.log(error);
        });
    },
    async getAllPosts(context) {
      fetch("http://localhost:3000/api/posts")
        .then(response => {
          if (response.ok) {
            return response.json();
          } else {
            throw Error(response.body);
          }
        })
        .then(data => {
          console.log(data);
          context.commit("SET_ALL_POSTS", data);
        })
        .catch(error => {
          console.log(error);
        });
    }
```

因此，所有这些操作实际上都是通过 fetch 调用我们在服务器上创建的新的相对 api。在对返回的承诺的解析中，如果响应是 ok 的，我们调用变异的提交，否则我们只记录错误。显然，当 promise resolution 有一个我们以后必须使用的对象参数时，我们必须对它进行反序列化，使用 response.json()方法的 promise resolution 很容易做到这一点。有了它，我们就可以将数据发送给我们必须进行的突变。

注意，我们从来没有为帖子添加删除按钮，所以现在让我们来做一下:在 SinglePost 组件中，在卡片的槽标题中添加一个普通的按钮，如

```
<button class="delete-button" @click.prevent="deletePost">        Delete
</button>
```

其背后的逻辑非常简单，我们必须用 Post 值调度 detetePost 操作:

```
deletePost() {
  this.$store.dispatch("posts/deletePost", { post: this.post });    }
```

现在我们只需要初始化文章存储。

我们可以在 Posts.vue 组件中这样做，使用 mounted()配置方法，如下所示:

```
mounted() {    
  this.$store.dispatch("posts/getAllPosts");
  }
```

就像这样，当 post 组件被挂载时，它调用存储库的 getAllPosts 操作，用来自服务器的所有帖子填充存储库。然后，vue 用户界面中的每个动作都调用商店的相关动作，商店的更新自动反映在组件中。

事实上，动作是异步的，允许在它们解决时做一些动作(例如，我们可以在用户点击 delete post 时设置一个等待的 gif 或动画，并在动作解决时删除动画)。

有了这个设置，我们就可以启动服务器，然后是 vue 客户端，然后一切都应该正常工作，帖子实际上被添加到后端存储。

我们实际上还有两个大问题要解决:

1.  帖子实际上保存在内存中，这将在下一课中解决，现在如果我们停止服务器，帖子实际上将永远丢失，但至少客户端现在是持久的，重新打开页面将再次显示数据。
2.  身份验证，现在任何用户都可以通过向服务器传递 post 对象来存储新的帖子。

在下一章中，我们将解决最后一个问题，因此只有经过认证的用户才能登录并使用实际的应用程序。