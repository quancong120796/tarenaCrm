

#####  一、业务场景

- CRM定时任务每天凌晨0点从EHR系统获取当前时间的EHR在岗员工数，并根据BU标签匹配实际中心后将数据同步至crm系统(**主要作为月报员工数取数用**)
- 相关对接业务老师：EHR-臧玥老师，财务-马霓、王楠楠老师

----

##### 二、 接口地址、参数

- 【crm代码文件名】：PersionnelCountHandler.java
- 【定时任务执行器名称】：personnelCountHandler
- 【参数配置文件】:  *ehr.properties*
- 【正式环境接口地址】： http://hr.tedu.cn/services/HrGzDataSyncService
- 【测试环境接口地址】： http://192.168.191.225/services/HrGzDataSyncService
- 【方法名】:  syncToCrmSalaryCount
- 其他参数(*infotype*|*username*|*password*)见配置文件

----

##### 三、返回数据结构示例与说明

- 返回数据示例 与各个标签对应指标备注

  ``` xml
  <aacz0>2020-05-01</aacz0> 						<!--调用接口日期-->
  <a0122 code="0000210201"> 						<!--编制中心code(对应crm中的中心编码)-->
      <a01ag code="">								<!--EHR以及BU标签-->
  	    <a01ah code="">							<!--EHR二级BU标签(用于匹配实际中心)
  			<a0104 code="0229">					<!--公司编码code-->
  			    <e02aw code="11">				<!--岗位编码-->
                      <aadae>39128.00</aadae>		<!--工资-->
                      <aaaa0>9896.00</aaaa0>		<!--奖金、绩效-->
                      <aa8ap>5891.23</aa8ap>		<!--社保-->
                      <aa9aa>3000.00</aa9aa>		<!--公积金-->
                      <aadb9>0.00</aadb9>			<!--年终奖-->
                      <aadai>0.00</aadai>			<!--离职补偿-->
                      <pcount>4</pcount>			<!--在岗人数-->
  				</e02aw>
  			</a0104>
  		</a01ah>
  	</a01ag>
  </a0122>
  ```


##### 四、接口返回数据处理

- 集团指定中心(字典**ehr_match_center**)需要根据二级BU标签匹配实际成本中心(已通过字典**bu_center_relation**配置，如有修改需要调整)：

- ![软件项目](..\文档附件\bu\软件项目.png)

  ![总部线上中心](..\文档附件\bu\总部线上中心.png)

  ![童程童美事业部](..\文档附件\bu\童程童美事业部.png)

  ![AI实验室](..\文档附件\bu\AI实验室.png)

- 插入crm数据库**rpt_mon_staff**表

----

##### 五、数据字典说明

- **ehr_match_center** EHR同步需匹配bu的中心，选项值为需匹配实际业务中心的编码(对应EHR的10位中心编码)
- **bu_center_relation** EHR二级Bu 及和实际成本中心匹配关系，其中字典选项的扩展字段1为对应的中心编码

----

##### 六、系统查询入口

- 可在"**EHR在岗员工数**"功能查同步到crm的在岗员工数

- 月报中每月的在岗员工数可通过日期条件为次月1号查询,例:

- 10月月报在岗员工数，日期查询条件如下

  <img src="..\文档附件\cutImages\在岗员工数.png" alt="在岗员工数" style="zoom:150%;" />

​    

