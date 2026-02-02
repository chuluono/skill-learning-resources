# Python编程教程案例

## 基础教程案例

### 1. 猜数字游戏

- **功能描述**：
  - 计算机生成一个1-100之间的随机数
  - 用户输入猜测的数字
  - 计算机提示猜大了还是猜小了
  - 直到猜对为止
  - 统计猜测次数

- **核心代码**：
  ```python
  import random

  def guess_number():
      number = random.randint(1, 100)
      guesses = 0
      
      print("猜数字游戏：1-100之间的数字")
      
      while True:
          guess = int(input("请输入你的猜测："))
          guesses += 1
          
          if guess < number:
              print("猜小了！")
          elif guess > number:
              print("猜大了！")
          else:
              print(f"恭喜你猜对了！用了{guesses}次猜测")
              break

  if __name__ == "__main__":
      guess_number()
  ```

- **学习要点**：
  - 随机数生成
  - 输入输出
  - 循环和条件判断
  - 变量使用

### 2. 待办事项列表

- **功能描述**：
  - 添加待办事项
  - 查看所有待办事项
  - 标记待办事项为已完成
  - 删除待办事项
  - 退出程序

- **核心代码**：
  ```python
  def todo_list():
      todos = []
      
      while True:
          print("\n待办事项列表")
          print("1. 添加待办事项")
          print("2. 查看待办事项")
          print("3. 标记完成")
          print("4. 删除待办事项")
          print("5. 退出")
          
          choice = input("请选择操作：")
          
          if choice == "1":
              task = input("请输入待办事项：")
              todos.append({"task": task, "completed": False})
              print("待办事项已添加")
          
          elif choice == "2":
              if not todos:
                  print("没有待办事项")
              else:
                  for i, todo in enumerate(todos, 1):
                      status = "[✓]" if todo["completed"] else "[ ]"
                      print(f"{i}. {status} {todo['task']}")
          
          elif choice == "3":
              if not todos:
                  print("没有待办事项")
              else:
                  index = int(input("请输入要标记的待办事项编号：")) - 1
                  if 0 <= index < len(todos):
                      todos[index]["completed"] = True
                      print("待办事项已标记为完成")
                  else:
                      print("无效的编号")
          
          elif choice == "4":
              if not todos:
                  print("没有待办事项")
              else:
                  index = int(input("请输入要删除的待办事项编号：")) - 1
                  if 0 <= index < len(todos):
                      todos.pop(index)
                      print("待办事项已删除")
                  else:
                      print("无效的编号")
          
          elif choice == "5":
              print("退出程序")
              break
          
          else:
              print("无效的选择")

  if __name__ == "__main__":
      todo_list()
  ```

- **学习要点**：
  - 列表操作
  - 字典使用
  - 循环和条件判断
  - 函数定义
  - 用户输入处理

### 3. 简单计算器

- **功能描述**：
  - 支持基本算术运算（加、减、乘、除）
  - 处理用户输入
  - 显示计算结果
  - 处理错误输入

- **核心代码**：
  ```python
  def calculator():
      print("简单计算器")
      print("支持的操作：+ (加), - (减), * (乘), / (除)")
      
      try:
          num1 = float(input("请输入第一个数字："))
          operation = input("请输入操作符：")
          num2 = float(input("请输入第二个数字："))
          
          if operation == "+":
              result = num1 + num2
              print(f"结果：{num1} + {num2} = {result}")
          elif operation == "-":
              result = num1 - num2
              print(f"结果：{num1} - {num2} = {result}")
          elif operation == "*":
              result = num1 * num2
              print(f"结果：{num1} * {num2} = {result}")
          elif operation == "/":
              if num2 == 0:
                  print("错误：除数不能为零")
              else:
                  result = num1 / num2
                  print(f"结果：{num1} / {num2} = {result}")
          else:
              print("错误：无效的操作符")
      except ValueError:
          print("错误：请输入有效的数字")

  if __name__ == "__main__":
      calculator()
  ```

- **学习要点**：
  - 函数定义
  - 输入验证
  - 异常处理
  - 条件判断
  - 算术运算

## 中级教程案例

### 1. 网络爬虫

- **功能描述**：
  - 爬取网站内容
  - 解析HTML
  - 提取链接和文本
  - 保存结果到文件

- **核心代码**：
  ```python
  import requests
  from bs4 import BeautifulSoup

  def web_crawler(url):
      try:
          # 发送HTTP请求
          response = requests.get(url)
          response.raise_for_status()  # 检查请求是否成功
          
          # 解析HTML
          soup = BeautifulSoup(response.text, 'html.parser')
          
          # 提取标题
          title = soup.title.string if soup.title else "无标题"
          print(f"网页标题：{title}")
          
          # 提取链接
          links = []
          for a in soup.find_all('a', href=True):
              link = a['href']
              # 处理相对链接
              if not link.startswith('http'):
                  link = requests.compat.urljoin(url, link)
              links.append(link)
          
          print(f"\n找到 {len(links)} 个链接：")
          for i, link in enumerate(links[:10], 1):  # 只显示前10个链接
              print(f"{i}. {link}")
          
          if len(links) > 10:
              print(f"... 还有 {len(links) - 10} 个链接未显示")
          
          # 保存结果到文件
          with open('crawler_results.txt', 'w', encoding='utf-8') as f:
              f.write(f"网页标题：{title}\n\n")
              f.write("链接列表：\n")
              for link in links:
                  f.write(f"{link}\n")
          
          print("\n结果已保存到 crawler_results.txt")
          
      except requests.exceptions.RequestException as e:
          print(f"网络错误：{e}")
      except Exception as e:
          print(f"错误：{e}")

  if __name__ == "__main__":
      url = input("请输入要爬取的网址：")
      web_crawler(url)
  ```

- **学习要点**：
  - HTTP请求
  - HTML解析
  - 异常处理
  - 文件操作
  - 第三方库使用

### 2. 数据可视化

- **功能描述**：
  - 生成随机数据
  - 创建折线图
  - 添加标题和标签
  - 保存图表

- **核心代码**：
  ```python
  import matplotlib.pyplot as plt
  import numpy as np

  def data_visualization():
      # 生成随机数据
      x = np.linspace(0, 10, 100)
      y1 = np.sin(x)
      y2 = np.cos(x)
      
      # 创建图表
      plt.figure(figsize=(10, 6))
      
      # 绘制折线图
      plt.plot(x, y1, label='sin(x)', color='blue', linewidth=2)
      plt.plot(x, y2, label='cos(x)', color='red', linewidth=2, linestyle='--')
      
      # 添加标题和标签
      plt.title('正弦和余弦函数', fontsize=16)
      plt.xlabel('x轴', fontsize=12)
      plt.ylabel('y轴', fontsize=12)
      
      # 添加图例
      plt.legend()
      
      # 添加网格
      plt.grid(True, linestyle='--', alpha=0.7)
      
      # 保存图表
      plt.savefig('sin_cos_plot.png', dpi=150, bbox_inches='tight')
      
      # 显示图表
      plt.show()
      
      print("图表已保存到 sin_cos_plot.png")

  if __name__ == "__main__":
      data_visualization()
  ```

- **学习要点**：
  - 数据生成
  - 图表绘制
  - 第三方库使用
  - 数据可视化
  - 文件保存

### 3. 面向对象的银行账户

- **功能描述**：
  - 创建银行账户
  - 存款
  - 取款
  - 查看余额
  - 账户信息

- **核心代码**：
  ```python
  class BankAccount:
      def __init__(self, account_holder, initial_balance=0.0):
          self.account_holder = account_holder
          self.balance = initial_balance
      
      def deposit(self, amount):
          if amount > 0:
              self.balance += amount
              print(f"成功存入 {amount} 元")
          else:
              print("存款金额必须大于零")
      
      def withdraw(self, amount):
          if amount > 0:
              if amount <= self.balance:
                  self.balance -= amount
                  print(f"成功取出 {amount} 元")
              else:
                  print("余额不足")
          else:
              print("取款金额必须大于零")
      
      def check_balance(self):
          print(f"账户余额：{self.balance} 元")
          return self.balance
      
      def get_account_info(self):
          print(f"账户持有人：{self.account_holder}")
          print(f"账户余额：{self.balance} 元")

  def bank_account_demo():
      print("银行账户系统")
      
      # 创建账户
      name = input("请输入账户持有人姓名：")
      initial = float(input("请输入初始存款金额："))
      account = BankAccount(name, initial)
      
      while True:
          print("\n1. 存款")
          print("2. 取款")
          print("3. 查看余额")
          print("4. 查看账户信息")
          print("5. 退出")
          
          choice = input("请选择操作：")
          
          if choice == "1":
              amount = float(input("请输入存款金额："))
              account.deposit(amount)
          elif choice == "2":
              amount = float(input("请输入取款金额："))
              account.withdraw(amount)
          elif choice == "3":
              account.check_balance()
          elif choice == "4":
              account.get_account_info()
          elif choice == "5":
              print("退出系统")
              break
          else:
              print("无效的选择")

  if __name__ == "__main__":
      bank_account_demo()
  ```

- **学习要点**：
  - 类的定义
  - 构造函数
  - 实例方法
  - 面向对象编程
  - 封装

## 高级教程案例

### 1. 简单的Web服务器

- **功能描述**：
  - 创建一个简单的HTTP服务器
  - 处理GET请求
  - 返回HTML响应
  - 处理不同的路径

- **核心代码**：
  ```python
  import http.server
  import socketserver

  class MyHTTPRequestHandler(http.server.SimpleHTTPRequestHandler):
      def do_GET(self):
          # 处理根路径
          if self.path == '/':
              self.send_response(200)
              self.send_header('Content-type', 'text/html')
              self.end_headers()
              
              response = '''
              <html>
              <head><title>简单Web服务器</title></head>
              <body>
              <h1>欢迎访问我的Web服务器</h1>
              <p>这是一个用Python创建的简单Web服务器</p>
              <p><a href="/about">关于</a></p>
              </body>
              </html>
              '''
              
              self.wfile.write(response.encode('utf-8'))
          
          # 处理/about路径
          elif self.path == '/about':
              self.send_response(200)
              self.send_header('Content-type', 'text/html')
              self.end_headers()
              
              response = '''
              <html>
              <head><title>关于</title></head>
              <body>
              <h1>关于本服务器</h1>
              <p>这是一个使用Python的http.server模块创建的简单Web服务器</p>
              <p><a href="/">返回首页</a></p>
              </body>
              </html>
              '''
              
              self.wfile.write(response.encode('utf-8'))
          
          # 处理其他路径
          else:
              self.send_response(404)
              self.send_header('Content-type', 'text/html')
              self.end_headers()
              
              response = '''
              <html>
              <head><title>404 Not Found</title></head>
              <body>
              <h1>404 Not Found</h1>
              <p>请求的页面不存在</p>
              <p><a href="/">返回首页</a></p>
              </body>
              </html>
              '''
              
              self.wfile.write(response.encode('utf-8'))

  def start_server():
      PORT = 8000
      
      with socketserver.TCPServer(("", PORT), MyHTTPRequestHandler) as httpd:
          print(f"服务器已启动，监听端口 {PORT}")
          print(f"请在浏览器中访问 http://localhost:{PORT}")
          print("按 Ctrl+C 停止服务器")
          
          try:
              httpd.serve_forever()
          except KeyboardInterrupt:
              print("\n服务器正在停止...")
              httpd.shutdown()
              print("服务器已停止")

  if __name__ == "__main__":
      start_server()
  ```

- **学习要点**：
  - HTTP服务器创建
  - 请求处理
  - 响应生成
  - 异常处理
  - 网络编程

### 2. 机器学习模型

- **功能描述**：
  - 生成分类数据
  - 训练一个简单的分类模型
  - 评估模型性能
  - 可视化决策边界

- **核心代码**：
  ```python
  import numpy as np
  import matplotlib.pyplot as plt
  from sklearn.datasets import make_classification
  from sklearn.model_selection import train_test_split
  from sklearn.linear_model import LogisticRegression
  from sklearn.metrics import accuracy_score, confusion_matrix

  def machine_learning_demo():
      # 生成分类数据
      X, y = make_classification(
          n_samples=200, n_features=2, n_informative=2, 
          n_redundant=0, n_classes=2, random_state=42
      )
      
      # 划分训练集和测试集
      X_train, X_test, y_train, y_test = train_test_split(
          X, y, test_size=0.2, random_state=42
      )
      
      # 训练逻辑回归模型
      model = LogisticRegression()
      model.fit(X_train, y_train)
      
      # 预测测试集
      y_pred = model.predict(X_test)
      
      # 评估模型
      accuracy = accuracy_score(y_test, y_pred)
      conf_matrix = confusion_matrix(y_test, y_pred)
      
      print(f"模型准确率：{accuracy:.2f}")
      print("混淆矩阵：")
      print(conf_matrix)
      
      # 可视化数据和决策边界
      plt.figure(figsize=(10, 6))
      
      # 绘制训练数据
      plt.scatter(X_train[:, 0], X_train[:, 1], c=y_train, cmap='coolwarm', edgecolors='k', s=50, label='训练数据')
      
      # 绘制测试数据
      plt.scatter(X_test[:, 0], X_test[:, 1], c=y_test, cmap='coolwarm', edgecolors='k', s=100, marker='*', label='测试数据')
      
      # 绘制决策边界
      x_min, x_max = X[:, 0].min() - 1, X[:, 0].max() + 1
      y_min, y_max = X[:, 1].min() - 1, X[:, 1].max() + 1
      xx, yy = np.meshgrid(np.arange(x_min, x_max, 0.01), np.arange(y_min, y_max, 0.01))
      Z = model.predict(np.c_[xx.ravel(), yy.ravel()])
      Z = Z.reshape(xx.shape)
      plt.contourf(xx, yy, Z, alpha=0.3, cmap='coolwarm')
      
      # 添加标题和标签
      plt.title('逻辑回归分类器', fontsize=16)
      plt.xlabel('特征 1', fontsize=12)
      plt.ylabel('特征 2', fontsize=12)
      plt.legend()
      
      # 保存图表
      plt.savefig('logistic_regression.png', dpi=150, bbox_inches='tight')
      
      # 显示图表
      plt.show()
      
      print("图表已保存到 logistic_regression.png")

  if __name__ == "__main__":
      machine_learning_demo()
  ```

- **学习要点**：
  - 数据生成
  - 模型训练
  - 模型评估
  - 数据可视化
  - 机器学习库使用

### 3. 异步编程

- **功能描述**：
  - 演示异步IO
  - 并发执行任务
  - 比较同步和异步执行时间

- **核心代码**：
  ```python
  import asyncio
  import time

  async def async_task(name, duration):
      print(f"任务 {name} 开始")
      await asyncio.sleep(duration)
      print(f"任务 {name} 完成")
      return f"任务 {name} 结果"

  def sync_task(name, duration):
      print(f"任务 {name} 开始")
      time.sleep(duration)
      print(f"任务 {name} 完成")
      return f"任务 {name} 结果"

  async def async_main():
      print("\n=== 异步执行 ===")
      start_time = time.time()
      
      # 并发执行多个任务
      tasks = [
          async_task("A", 2),
          async_task("B", 1),
          async_task("C", 3)
      ]
      
      results = await asyncio.gather(*tasks)
      
      end_time = time.time()
      print(f"异步执行时间：{end_time - start_time:.2f} 秒")
      print(f"结果：{results}")

  def sync_main():
      print("\n=== 同步执行 ===")
      start_time = time.time()
      
      # 顺序执行多个任务
      results = [
          sync_task("A", 2),
          sync_task("B", 1),
          sync_task("C", 3)
      ]
      
      end_time = time.time()
      print(f"同步执行时间：{end_time - start_time:.2f} 秒")
      print(f"结果：{results}")

  if __name__ == "__main__":
      sync_main()
      asyncio.run(async_main())
  ```

- **学习要点**：
  - 异步IO
  - 协程
  - 并发执行
  - 时间比较
  - 异步编程模式

## 总结

这些教程案例涵盖了从基础到高级的Python编程技能，每个案例都包含了核心代码和学习要点。通过实践这些案例，你可以：

1. **巩固基础知识**：通过基础案例练习Python的核心语法和数据结构
2. **提升编程能力**：通过中级案例学习文件操作、网络编程和面向对象编程
3. **掌握高级特性**：通过高级案例了解Web服务器、机器学习和异步编程

建议按照难度顺序学习这些案例，每个案例都尝试自己实现，然后与提供的代码对比，找出改进的空间。实践是学习编程的最佳方式！