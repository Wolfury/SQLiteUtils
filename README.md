# SQLiteUtils
一个简单的基于Android的Sqlite数据库的操作封装，可以让你通过操作对象来操作表。

# 简单的示例代码:

1. 定义表对应的对象，对于基本类型，请使用对象类型来声明类成员，如int用Integer。以免在update的时候出现意想不到的情况。
```java
@Table(name="t_user")
public class UserModel {
	@Table.Column(name="user_id",type=Column.TYPE_INTEGER,isPrimaryKey=true)
	public Integer userId;
	
	@Table.Column(name="user_name",type=Column.TYPE_STRING,isNull=false)
	public String userName;
	
	@Table.Column(name="born_date",type=Column.TYPE_TIMESTAMP)
	public Date bornDate;
	
	@Table.Column(name="pictrue",type=Column.TYPE_BLOB)
	public byte[] pictrue;
	
	@Table.Column(name="is_login",type=Column.TYPE_BOOLEAN)
	public Boolean isLogin;
	
	@Table.Column(name="weight",type=Column.TYPE_DOUBLE)
	public Double weight;
}
```
2. 增删查改:
初始化对象:
```java
SQLiteDatabase db = context.openOrCreateDatabase("test.db", Context.MODE_PRIVATE, null);
DbSqlite dbSqlite = new DbSqlite(db);
IBaseDao<UserModel> userDAO = DaoFactory.createGenericDao(dbSqlite, UserModel.class);
```
创建Table:
```java
userDAO.createTable();
```
insert 一条记录：
```java
UserModel user = new UserModel();
user.userName = "darcy";
user.isLogin = true;
user.weight = 60.5;
user.bornDate = new Date();
byte[] picture = {0x1,0x2,0x3,0x4};
user.pictrue = picture;
userDAO.insert(user);
```
update 记录:
```java
UserModel user = new UserModel();
user.weight = 88.0;
userDAO.update(user, "user_name=?", "darcy");
```
查询结果:
```java
UserModel user = userDAO.queryFirstRecord("user_name=?", "darcy");

List<UserModel> userList = userDAO.queryByCondition("user_name=? and weight > ?", "darcy" , "60");
```
