# 设计文档：

**数据库实体如下：**

```ruby
rails generate model User name:string email:string password_digest:string
rails db:migrate

rails generate model Product name:string description:text price:decimal sales_count:integer image_path:string color_id:integer
rails db:migrate

rails generate model Color rgb_value:string name:string
rails db:migrate

rails generate model Cart user:references
rails db:migrate

rails generate model CartItem cart:references product:references quantity:integer
rails db:migrate

rails generate model Favorite user:references product:references
rails db:migrate

rails generate model Order user:references recipient_name:string address:string phone:string total_price:decimal status:string
rails db:migrate

rails generate model OrderItem order:references product:references quantity:integer price:decimal
rails db:migrate
```

**后期还为一些实体添加了一些新的属性**

# 实体的简单说明：

* **首先是登录和注册的实现，采用用户名和邮箱注册的方式，通过邮箱和密码来登录**

* **商品的属性选择了一些常规的属性，名字，图片等属性，同时设计了一个color的实体作为商品的一个属性，具体实现就是创建一个product,填写该product的一些属性，就可以完成创建，同时在主页进行展示，为用户提供选择。**
* **color实体没有写创建color的界面，主要觉得不好看以及乱填`rgb`值导致颜色错误，所以我自己写了一点颜色实体放在数据库中，创建商品时可以提供选择。**

* **然后是购物车的实体，大概的功能就是每个用户都有唯一的购物车实体，购物车里面会有多个商品和该商品的数量，也就是`CartItem`,用户可以在主页选择商品和对应的数量放在购物车中，进到购物车进行价格的结算。**
* **进行结算时，就涉及到订单实体，点击结算时，要求填写该订单的`recipient_name:string address:string phone:string`,最终和购物车形成一个完整的订单。形成订单后购物车回清空。**
* **还有一个收藏的实体，就是对商品进行收藏，收藏夹可以实现对商品加入购物车的操作。**



# 使用手册(新属性说明)：

1. **首先就是该商城不同的角色会有不同的权限，本来打算只有一个`admin`,但是这样写的话只有登录特定的账户才可以有所有的权限，感觉展示不方便，于是就改为注册时可以选择自己的`role`,所以新添了一个`role`，有三个身份:`customer, seller, admin`。**
2. **对于不同的身份会有不同的权限，`seller, admin`可以新增商品以及修改现有的商品的属性。**
3. **只有`admin`可以查看所有的订单，而其他身份只能查看自己的订单。`admin`还有对订单进行发货的权限，也就为订单新引入一个属性:`ship`，其实也没什么太大是实际意义，就是修改订单是否发货，感觉没啥作用，没有联系到其他的实体属性。**
4. **最后就是只有`admin`可以对所有用户的`name,email,role`进行修改，其他用户没有权限，所有的链接我都放出来了，要是点不进去就是没有权限，会给个提示。**

