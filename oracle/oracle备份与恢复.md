#Oracle 备份与恢复
## 备份
### 1.物理备份
物理备份是数据库文件从一处拷贝到另一处的备份过程，通常是从磁盘到磁盘。物理备份有分为冷备份和热备份，冷和热是相对数据库的开和关来说的。

- 物理备份之冷备份
1. 关闭数据库
2. 拷贝相关文件到安全区域
3. 重启数据库
- 物理备份之热备份
1. 设置数据库为归档模式
2. 拷贝相关归档文件到安全区域
3. 重启数据库


### 2.逻辑备份
逻辑备份是数据库抽取数据并存与二进制文件的过程。逻辑备份分为三种模式：表备份、用户备份、完全备份。工具有exp/imp或expdp/impdp(10g以后)

- 逻辑备份之exp
  - 表模式
    ```bash
    exp {username}/{password}@{dbname} OWNER={username} TABLES={tablename[,tablename]} FILE={filepath} [query=\"{querystring}\"];
    ```
  - 用户模式
  ```bash
  exp {username}/{password}@{dbname} OWNER={username} FILE={filepath};
  ```
  - 完全模式
  ```bash
  exp system/{password}@{dbname} FILE={filepath} FULL=Y;
  ```

- 逻辑备份之expdp
导出方案
`expdp system/{password} DIRECTORY={dir} DUMPFILE={filename} TABLES={tablename[, tablename]};`
导出表空间
`expdp system/{password} DIRECTORY={dir} DUMPFILE={filename} SCHEMAS={username};`
导出数据库
`expdp system/{password} DIRECTORY={dir} DUMPFILE={filename} FULL=Y;`

#### 恢复
** 一.物理恢复 **

	1.关闭数据库
	2.将备份文件拷贝到原系统目录中
	3.重启数据库，恢复数据库

** 二.逻辑恢复之imp **
1.表模式
`imp {username}/{password}@{dbname} OWNER={username} TABLES={tablename[,tablename]} FILE={filepath};`
2.用户模式
`imp {username}/{password}@{dbname} FROMUSER={username} TOUSER={username} FILE={filepath};`
3.完全模式
`imp system/{password}@{dbname} FULL=Y FILE={filepath};`


