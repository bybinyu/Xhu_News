根据你的接口和任务要求，以下是你应该编写的 `Service` 类及其功能划分。每个 `Service` 类对应一个主要模块，并封装了与 `Mapper` 交互的逻辑。

------

### 1. **`NewsService`**

负责处理新闻相关的业务逻辑。

#### **主要功能：**

1. **添加新闻**
   - 方法名：`addNews(news_table news)`
   - 调用 `news_mapper.addNews`，实现新闻的新增。
2. **更新新闻**
   - 方法名：`updateNews(news_table news)`
   - 调用 `news_mapper.updateNews`，实现新闻的修改。
3. **删除新闻**
   - 方法名：`deleteNews(String newsId)`
   - 调用 `news_mapper.deleteNews`，实现新闻的删除。
4. **查询新闻**
   - 方法名：`getNewsById(String newsId)`
   - 调用 `news_mapper.getNewsById`，返回新闻详细信息。
5. **更新点赞数**
   - 方法名：`updateLikeCount(String newsId, int increment)`
   - 调用 `news_mapper.updateLikeCount`，更新新闻点赞数。
6. **添加新闻图片**
   - 方法名：`addImage(String imgId, String newsId, String imgPath)`
   - 调用 `news_mapper.addImage`，上传新闻图片。
7. **查询新闻图片**
   - 方法名：`getImagesByNewsId(String newsId)`
   - 调用 `news_mapper.getImagesByNewsId`，获取新闻图片列表。
8. **添加新闻评论**
   - 方法名：`addComment(account_com comment)`
   - 调用 `news_mapper.addComment`，新增评论。
9. **查询新闻评论**
   - 方法名：`getCommentsByNewsId(String newsId)`
   - 调用 `news_mapper.getCommentsByNewsId`，返回评论列表。
10. **删除评论**
    - 方法名：`deleteComment(String comId)`
    - 调用 `news_mapper.deleteComment`，删除指定评论。

------

### 2. **`AccountService`**

负责用户管理和操作相关的业务逻辑。

#### **主要功能：**

1. **用户注册**
   - 方法名：`registerAccount(account_table account)`
   - 调用 `account_mapper.register_Account`，注册新用户。
2. **用户登录验证**
   - 方法名：`validateLogin(String account, String password)`
   - 调用 `account_mapper.validate_Login`，验证登录信息。
3. **用户信息维护**
   - 方法名：`setAccountInfo(account_inform accountInform)`
   - 调用 `account_mapper.setAccount_Info`，插入或更新用户信息。
4. **查询用户信息**
   - 方法名：`getAccountInfo(String account)`
   - 调用 `account_mapper.getAccountInfo`，获取用户详细信息。
5. **用户点赞操作**
   - 添加点赞：
     - 方法名：`addAccountFav(String account, String newsId)`
     - 调用 `account_mapper.addAccount_fav`，新增点赞记录。
   - 取消点赞：
     - 方法名：`removeAccountFav(String account, String newsId)`
     - 调用 `account_mapper.removeAccount_fav`，删除点赞记录。

------

### 3. **`AdminService`**

负责管理员的业务逻辑，主要包括敏感词过滤和审核功能。

#### **主要功能：**

1. **敏感词过滤**
   - 方法名：`filterSensitiveWords(String content)`
   - 逻辑：
     1. 查询所有敏感词。
     2. 通过关键词替换（如用 `*` 替换敏感词）处理输入内容。
2. **审核新闻评论**
   - 方法名：`reviewComment(String comId)`
   - 调用 `news_mapper.getCommentsByNewsId`，结合敏感词过滤逻辑，返回审核结果。
3. **审核新闻内容**
   - 方法名：`reviewNews(String newsId)`
   - 调用 `news_mapper.getNewsById`，结合敏感词过滤逻辑，返回审核结果。

------

### 4. **`SpiderService`**

负责爬取外部新闻内容。

#### **主要功能：**

1. **爬取指定网站新闻**
   - 方法名：`crawlNews(String url)`
   - 逻辑：
     1. 爬取指定 URL 下的新闻内容。
     2. 解析后调用 `news_mapper.addNews` 插入数据库。
2. **爬取关键字相关新闻**
   - 方法名：`crawlByKeyword(String keyword)`
   - 逻辑：
     1. 调用爬虫工具，根据关键字爬取相关新闻。
     2. 解析后调用 `news_mapper.addNews` 插入数据库。

------

### 服务与接口映射关系

| **Service 类**   | **调用的 Mapper 接口**          | **功能模块**           |
| ---------------- | ------------------------------- | ---------------------- |
| `NewsService`    | `news_mapper`                   | 新闻的管理与交互       |
| `AccountService` | `account_mapper`                | 用户的注册、登录与互动 |
| `AdminService`   | `news_mapper`, `account_mapper` | 敏感词过滤与审核       |
| `SpiderService`  | `news_mapper`                   | 爬取新闻并保存到数据库 |

------

### 示例代码（以 `NewsService` 为例）

```java
@Service
public class NewsService {
    @Autowired
    private news_mapper newsMapper;

    public int addNews(news_table news) {
        return newsMapper.addNews(news);
    }

    public int updateNews(news_table news) {
        return newsMapper.updateNews(news);
    }

    public int deleteNews(String newsId) {
        return newsMapper.deleteNews(newsId);
    }

    public news_table getNewsById(String newsId) {
        return newsMapper.getNewsById(newsId);
    }

    public int updateLikeCount(String newsId, int increment) {
        return newsMapper.updateLikeCount(newsId, increment);
    }

    public int addImage(String imgId, String newsId, String imgPath) {
        return newsMapper.addImage(imgId, newsId, imgPath);
    }

    public List<String> getImagesByNewsId(String newsId) {
        return newsMapper.getImagesByNewsId(newsId);
    }

    public int addComment(account_com comment) {
        return newsMapper.addComment(comment);
    }

    public List<account_com> getCommentsByNewsId(String newsId) {
        return newsMapper.getCommentsByNewsId(newsId);
    }

    public int deleteComment(String comId) {
        return newsMapper.deleteComment(comId);
    }
}
```