# 项目三：WoLong-cFinance-Master

开发部分：

![image-20220726170955498](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220726170955498.png)

已开发完毕：

![image-20220726165626510](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220726165626510.png)

正在开发：

![image-20220726171112726](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220726171112726.png)

启动顺序：

1、WlDiscoveryApplication

2、WlConfigApplication

3、WlGatewayApplication

后面随便

![image-20220726170446212](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220726170446212.png)

## 实体类

### 1、用户信息实体类

```java
@Table(name = "user_info")
public class UserInfo implements Serializable{
    @Id
    @GeneratedValue(generator = "JDBC")
    private Integer id;

    /**
     * 用户名
     */
    @Column(name = "user_name")
    private String userName;

    /**
     * 密码
     */
    @Column(name = "pass_word")
    private String passWord;

    /**
     * 姓名
     */
    @Column(name = "nick_name")
    private String nickName;

    /**
     * 问题数
     */
    private Integer question;

    /**
     * 模块
     */
    @Column(name = "model_type")
    private String modelType;
```

## 枚举类

货币枚举类

```java
public enum CurEnum {
    RMB(1, "人民币"),
    EUR(2, "欧元"),
    USA(3, "美元"),
    YB(4, "英镑");

    private int code = 0;

    private String desc = "";

    CurEnum(int code, String desc) {
        this.code = code;
        this.desc = desc;
    }
```

公司类型枚举类

```java
public enum DeptTypeEnum {

    EC(1, "电机及驱动控制板块"),
    OTHER(2, "其他制造业板块"),
    BJ(3, "制造业本级"),
    NOPD(4, "非制造业");

    private int code = 0;

    private String desc = "";

    DeptTypeEnum(int code, String desc) {
        this.code = code;
        this.desc = desc;
    }
```

工厂资金数据类型枚举类

```java
public enum FactoryDataTypeEnum {

    XJ(1, "现金"),
    YHCD(2, "银行承兑"),
    SYCD(3, "商业承兑");


    private int code = 0;

    private String desc = "";

    FactoryDataTypeEnum(int code, String desc) {
        this.code = code;
        this.desc = desc;
    }
```

资金报表项实体类

```java
public enum FinanceReportItemNum {

    XSPTYDHLMX(1, "销售平台月度回笼明细报表"),
    MXB(2, "明细表(优先级)"),
    JYXZJHZB(3, "经营性资金汇总表"),
    HLHLBB(4, "货款回笼报表"),
    HLHLDYJLJSJYJHBJB(5, "货款回笼当月及累计实绩与计划比较表"),
    DYZJJHYSJZXB(6, "当月资金计划与实际执行比"),
    FYJYXZIJQKB(7, "分月经营性资金情况表"),
    CYGDJH(8, "次月滚动计划");

    private int code = 0;

    private String desc = "";

    FinanceReportItemNum(int code, String desc) {
        this.code = code;
        this.desc = desc;
    }
```

填报类型

```java
public enum FinanceTypeEnum {

    PLAN(1, "计划填报"),
    TRUE(2, "实绩填报");

    private int code = 0;

    private String desc = "";

    FinanceTypeEnum(int code, String desc) {
        this.code = code;
        this.desc = desc;
    }
```

## 注解

### 1、@Table

当实体类的名字与对应的数据库表名不同使，使用@Table标注说明。

@Table有四个选项：name、catalog、schema、uniqueConstraints。

**最经常用的选项：name用于指定数据库表名的名称**，若不指定表名，则生成的数据库表DDL是实体类名的小写开头名字。

若不指定则以实体类名称作为表名

```java
@Table
@Entity
public class Customer {
    private Integer id;
    private String name;
    private String email;
    private int age;
    ......
}
123456789
```

生成的数据库表DDL：

```java
CREATE TABLE `customer` (
  `ID` int(11) NOT NULL AUTO_INCREMENT,
  `Age` int(11) DEFAULT NULL,
  `Email` varchar(255) DEFAULT NULL,
  `Name` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`ID`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

**catalog属性：** 
catalog属性用于指定数据库实例名，一般来说persistence.xml文件中必须指定数据库url，url中将包含数据库实例

```java
<property name="javax.persistence.jdbc.url" value="jdbc:mysql://localhost:3306/jpa" />
1
```

当catalog属性不指定时，新创建的表将出现在url指定的数据库实例中 
当catalog属性设置名称时，若数据库存在和指定名称一致的实例，新创建的表将出现在该实例中，

```java
@Table(name="CUSTOMERS",catalog="hibernate")
@Entity
public class Customer {
    private Integer id;
    private String name;
    private String email;
    private int age;
    ......
}
```

**schema属性：** 
作用与catalog属性作用一致

**@uniqueConstraints属性**

uniqueConstraints 用来批量命名唯一键 
其作用等同于多个：@Column(unique = true)

![image-20220725093045958](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220725093045958.png)

### 2、@Column

@Column:当实体的属性与其映射的数据库表的列不同名时需要使用@Column 标注说明，该属性通常置于实体的属性声明语句之前(置于属性的getter方法之前),还可与 @Id 标注一起使用。
@Column(insertable = false,updatable = false)

@Column 标注的常用属性是 name，用于设置映射数据库表的列名。此外，该标注还包含其它多个属性，如：unique 、nullable、length 等。

​	**unique**表示该字段是否为唯一标识，默认为false。如果表中有一个字段需要唯一标识，则既可以使用该标记，也可以使用@Table标记中的@UniqueConstraint。

​	**nullable**表示该字段是否可以为null值，默认为true。

​	**insertable** 表示在使用“INSERT”脚本插入数据时，是否需要插入该字段的值。

​	**updatable**表示在使用“UPDATE”脚本插入数据时，是否需要更新该字段的值。insertable和updatable属性一般多用于只读的属性，例如主键和外键等。这些字段的值通常是自动生成的。

​	**columnDefinition**（大多数情况，几乎不用）表示创建表时，该字段创建的SQL语句，一般用于通过Entity生成表定义时使用。（也就是说，如果DB中表已经建好，该属性没有必要使用。）

​	**table**表示当映射多个表时，指定表的表中的字段。默认值为主表的表名。

​	**length**表示字段的长度，当字段的类型为varchar时，该属性才有效，默认为255个字符。

​	**precision和scale** precision属性和scale属性表示精度，当字段类型为double时，precision表示数值的总长度，scale表示小数点所占的位数。
​	**name** 定义了被标注字段在数据库表中所对应字段的名称；

### 3、@Id

**@Id**: 标注用于声明一个实体类的属性映射为数据库的**主键**列。该属性通常置于属性声明语句之前，可与声明语句同行，也可写在单独行上,但一个**实体类里面必须有一个**。 @Id标注也可置于属性的getter方法之前。

**作为符合主键类，要满足以下几点要求**
➢必须实现Serializable接口。
➢必须有默认的public无参数的构造方法。
➢必须覆盖equals和hashCode方法。equals方 法用于判断两个对象是否相同。
hashCode方法返回当前对象的哈希码，生 成的hashCode相同的概率越小越好，
算法可以进行优化

### 4、@GeneratedValue

@GeneratedValue:主要就是为一个实体生成一个唯一标识的主键、@GeneratedValue提供了主键的生成策略。@GeneratedValue注解有两个属性,分别是strategy和generator

generator属性的值是一个字符串,默认为"",其声明了主键生成器的名称
(对应于同名的主键生成器@SequenceGenerator和@TableGenerator)。

![image-20220725095138994](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220725095138994.png)

默认情况下，jpa 自动选择一个最适合底层数据库的主键生成策略：
strategy属性：提供四种值:

IDENTITY：采用数据库 ID自增长的方式来自增主键字段，Oracle 不支持这种方式；(SqlServer 对应 )

AUTO： JPA自动选择合适的策略，是默认选项；(MySQL 对应)

SEQUENCE：通过序列产生主键，通过 @SequenceGenerator 注解指定序列名，MySql 不支持这种方式

TABLE：通过表产生主键，框架借由表模拟序列产生主键，使用该策略可以使应用更易于数据库移植。

### 5、@ApiOperation

​	@ApiOperation 注解不是Spring 自带的，它是swagger里的com.wordnik.swagger.annotations.ApiOperation;
​	注解@ApiOperation 和@ApiParam是用来构建Api 文档的
​	@ApiOperation(value="接口说明"，httpMethod="接口请求方式"，response="接口返回参数类型"，notes="接口发布说明")；其他参数可参考源码
​	@ApiParam(required ="是否必须参数"，name="参数名称",value="参数具体描述")
​	@ApiOperation: 用在请求的方法上，说明方法的用途、作用

​	value="说明方法的用途、作用"
​	notes="方法的备注说明"

## 控制层

**1、FactoryCompilesController**

```java
/**
 * @Description
 * @Author RyanZhao
 * @Date 11:11 2022/7/8
 * @Version 1.0
 **/
@Controller
@RequestMapping(value = "/wl/funds/factory_compiles")
public class FactoryCompilesController {


    @Resource
    private WlFundsFactoryCompilesService baseService;
    @Resource
    private WlFundsFactoryReportService factoryExcelService;

    @RequestMapping("list")
    public String list() {
        return "funds/compiles/funds_compiles_factory_list";
    }

    /**
     * 查询数据
     *
     * @param bo
     * @param page
     * @return
     */
    @RequestMapping("json/list")
    @ResponseBody
    ReType json_list(WlFundsCompilesBO bo, LayuiPage page) {
        //获取当前的登录的用户
        ShiroUser shiroUser = ShiroUtil.getCurrentUser();
        bo.setDeptId(bo.getYsDept());
        bo.setCreateBy(shiroUser.getId());
        //传入查询的参数的封装对象bo，分页的参数，返回json对象给前端
        return baseService.show(bo, page.getPage(), page.getLimit());
    }

    /**
     * 导入提交
     *
     * @param file
     * @param bo
     * @return
     */
    @RequestMapping(value = "add/upload", method = RequestMethod.POST)
    @ResponseBody
    JsonUtil fileUpLoad(@RequestParam MultipartFile file, WlFundsCompilesBO bo) {
        ShiroUser shiroUser = ShiroUtil.getCurrentUser();
        JsonUtil jsonUtil = new JsonUtil();
        //传入需要上传的excel文件、工厂平台数据对象、json工具类对象、用户id和用户昵称，导入保存excel
        baseService.addByExcel(file, bo, jsonUtil, shiroUser.getId(), shiroUser.getNickName());
        //返回导入excel文件后的json对象状态信息。
        return jsonUtil;
    }

    /**
     * 导出导入数据(后期实现)
     *
     * @param response
     * @param bo
     */
    @RequestMapping("/excel/out")
    public void excelOut(HttpServletResponse response, WlFundsCompilesBO bo) {
        factoryExcelService.excelOut(response, bo);
    }
}
```

#### 2、FundsPlanReportController(资金计划报表)

```java
@Controller
@RequestMapping(value = "/wl/funds/funds_report")
public class FundsPlanReportController {

    @Resource
    private WlFundsPlanReportService fundsPlanReportService;

    /**
     * 资金执行计划报表
     *
     * @return
     */
    //点击资金报表跳转到资金报表页面
    @RequestMapping("sale_month_details")
    String saleMonthDetails() {
        return "funds/compiles/report/funds_plan_report";
    }

    /**
     * 在线查看报表
     *
     * @param model 和request差不多，model封装类返回前端的页面
     * @param bo 报表表单数据对象
     * @param deptIds 右侧勾选的填报单位(公司)的id
     * @return 跳转
     */
    @ApiOperation(value = "/query_model", notes = "查看预算报表")
    @Log(desc = "查看报表", type = Log.LOG_TYPE.ATHOR)
    @RequestMapping("report_view")
    public String reportView(Model model, WlFundsCompilesBO bo, Integer[] deptIds) {
        fundsPlanReportService.tableView(model, bo, deptIds);
        return "funds/compiles/report/title_table_view";
    }
}
```

资金报表页面：

![image-20220727102853246](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220727102853246.png)

## 方法

tableView方法：将excel表格转换成的html模型返回给前端页面

```java
/**
 * 1.在线查看
 *
 * @param model
 * @param bo
 * @param deptIds
 */
@Override
public void tableView(Model model, WlFundsCompilesBO bo, Integer[] deptIds) {
    List<TableModel> tableModelList = fundsPlanReportClient.queryModel(bo, deptIds);
    String title = "";
    //根据选择类型选择标题
    switch (bo.getQueryType()) {
        case 1:
            title = "销售平台月度回笼明细报表";
            break;
        case 2:
            title = "明细表(优先级)";
            break;
        case 3:
            title = "经营性资金汇总表";
            break;
        case 4:
            title = "货款回笼报表";
            break;
        case 5:
            title = "货款回笼当月及累计实绩与计划比较表";
            break;
        case 6:
            title = "当月资金计划与实际执行比";
            break;
        case 7:
            title = "分月经营性资金情况表";
            break;
        case 8:
            title = "次月滚动计划";
            break;
        default:
            break;
    }
   //将表格数据传给前端
    model.addAttribute("excelList", tableModelList);
    //将标传给前端
    model.addAttribute("title", title);
}
```

queryModel(bo, deptIds)方法：根据选择类型和部门id数组集合展示信息

```java
/**
 * 1.获取页面展示信息
 *
 * @param bo
 * @param deptIds
 * @return
 */
@Override
public List<TableModel> queryModel(WlFundsCompilesBO bo, Integer[] deptIds) {
    //对excel表进行操作的工具
    XSSFWorkbook wb = new XSSFWorkbook();
    //将传入的bu'me
    ReportUtil.setDeptIdStr(bo, deptIds);
    List<TableModel> tableModelList = null;
    switch (bo.getQueryType()) {
        case 1:

            tableModelList = BuidlTableModelListUtil.buildTableModelList(wb, 1, 1);
            break;
        case 2:
            //1.2.明细表（优先级），生成明细表
            Report02Work report02Work = create02HssfWork(wb, bo);
            //html表格模型
            tableModelList = BuidlTableModelListUtil.buildTableModelList(wb, report02Work.getDeptRow() + 3, 2);
            break;
        case 3:
            //1.3.经营性资金汇总表
            Report03Work report03Work = create03HssfWork(wb, bo);
            tableModelList = BuidlTableModelListUtil.buildTableModelList(wb, report03Work.getDeptRow() + 1, 1);
            break;
        case 4:
            //1.4.货款回笼报表
            create04HssfWork(wb, bo);
            tableModelList = BuidlTableModelListUtil.buildTableModelList(wb, 3, 2);
            break;
        case 5:
            //1.5.货款回笼当月及累计实绩与计划比较表

            tableModelList = BuidlTableModelListUtil.buildTableModelList(wb, 5, 2);
            break;
        case 6:

            tableModelList = BuidlTableModelListUtil.buildTableModelList(wb, 5, 2);
            break;
        case 7:

            tableModelList = BuidlTableModelListUtil.buildTableModelList(wb, 5, 2);
            break;
        case 8:

            tableModelList = BuidlTableModelListUtil.buildTableModelList(wb, 4, 2);
            break;
    }
    return tableModelList;
}
```
