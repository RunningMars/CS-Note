# mysql 是怎样运行的



采用TCP/IP连接：
<img src="MySql是怎样运行的.assets/image-20240326上午113141517.png" alt="image-20240326上午113141517" style="zoom: 50%;" />****

<img src="MySql是怎样运行的.assets/image-20240326下午14611493.png" alt="image-20240326下午14611493" style="zoom:50%;" /> 

小贴士：

MySQL 7.2 开始，不推荐使用查询缓存 ，MySQL8.0 中直接将其删除.

## 启动选项和系统变量

GLOBAL （全局范围）

SESSION （会话范围）

![image-20230726180744230](MySql.assets/image-20230726180744230.png)



## 字符集和比较规则

![image-20230726180322556](MySql.assets/image-20230726180322556.png)



![image-20230726180429156](MySql.assets/image-20230726180429156.png)



![image-20230726180456392](MySql.assets/image-20230726180456392.png)



![image-20230726180509722](MySql.assets/image-20230726180509722.png)

<img src="MySql是怎样运行的.assets/image-20240326下午22339155.png" alt="image-20240326下午22339155" style="zoom:50%;" />

## 从一条记录说起 -- InnoDB 录存储结构 

<img src="MySql是怎样运行的.assets/image-20240326下午22626359.png" alt="image-20240326下午22626359" style="zoom:50%;" />

记录的额外信息：

InnoDB 储存NULL值，其实只用到了一个bit的长度，再记录头中将所有的允许null的字段，储存值是否为null，0不是，1是。都记录在这个额外信息中。

<img src="MySql是怎样运行的.assets/image-20240326下午32918173.png" alt="image-20240326下午32918173" style="zoom:50%;" />

<img src="MySql是怎样运行的.assets/image-20240326下午33838558.png" alt="image-20240326下午33838558" style="zoom:50%;" />

<img src="MySql是怎样运行的.assets/image-20240326下午33856723.png" alt="image-20240326下午33856723" style="zoom:50%;" />

记录的真实数据：

<img src="MySql是怎样运行的.assets/image-20240326下午34052467.png" alt="image-20240326下午34052467" style="zoom:50%;" />

<img src="MySql是怎样运行的.assets/image-20240326下午34836584.png" alt="image-20240326下午34836584" style="zoom:50%;" />

![image-20230726174407215](MySql.assets/image-20230726174407215.png)









## 盛放记录的大盒子-- InnoDB数据页结构

### InnoDB 数据页结构

![image-20230726180039203](MySql.assets/image-20230726180039203.png)

页内的记录，以链表的方式连接在一起。

<img src="MySql是怎样运行的.assets/image-20240328下午45607613.png" alt="image-20240328下午45607613" style="zoom:50%;" />

![image-20230726180150117](MySql.assets/image-20230726180150117.png)







## B+树索引

![image-20230721135805036](MySql.assets/image-20230721135805036.png)



![image-20230721143028013](MySql.assets/image-20230721143028013.png)



#### MyISAM 需要至少执行一次回表

![image-20230721144705040](MySql.assets/image-20230721144705040.png)

![image-20230721144758451](MySql.assets/image-20230721144758451.png)

## B+树索引的使用



![image-20230721144954715](MySql.assets/image-20230721144954715.png)

![image-20230726142922904](MySql.assets/image-20230726142922904.png)

![image-20230726142620359](MySql.assets/image-20230726142654255.png)

![image-20230726142602123](MySql.assets/image-20230726142602123.png)



![image-20230726142523378](MySql.assets/image-20230726142523378.png)





#### 				回表的代价

![image-20230726144058569](MySql.assets/image-20230726144058569.png)

![image-20230726144110221](MySql.assets/image-20230726144110221.png)



![image-20230726144350202](MySql.assets/image-20230726144350202.png)



![image-20230726144741674](MySql.assets/image-20230726144741674.png)



![image-20230726145018802](MySql.assets/image-20230726145018802.png)

尽可能选择 小的数据类型 更有利于查询效率，不单单只是节约存储空间那点好处。往往几个int型的大小相差4倍及以上，选小的类型，在二级索引扫描时可以较少倍数级的磁盘I/O次数





![image-20230726152955023](MySql.assets/image-20230726152955023.png)





## MySQL 的数据目录

![image-20230726163127352](MySql.assets/image-20230726163127352.png)

![image-20230726163233246](MySql.assets/image-20230726163233246.png)



## InnoDB 的表空间

![image-20230726163700497](MySql.assets/image-20230726163700497.png)



![image-20230726164154595](MySql.assets/image-20230726164154595.png)



![image-20230726165204747](MySql.assets/image-20230726165204747.png)







![image-20230726181031393](MySql.assets/image-20230726181031393.png)

![image-20230726181325695](MySql.assets/image-20230726181325695.png)

![image-20230726181006435](MySql.assets/image-20230726181006435.png)







### InnoDB 数据字典 , 元数据

![image-20230731140959798](MySql.assets/image-20230731140959798.png)



![image-20230731141116343](MySql.assets/image-20230731141116343.png)



![image-20230731141522054](MySql.assets/image-20230731141522054.png)











## 单表访问方法

MySQLSe凹町 有一个称为优化器的模块. MySQL Server 在对一条查询语

句进行语法解析之后，就会将其交给优化器来优化，优化的结果就是生成一个所谓的执行计划~.

这个执行计划表明了应该使用哪些索引进行查询、表之间的连接顺序是啥样的 等等.最后会

按照执行计划中的步骤调用存储引擎提供的接口来真正地执行查询，并将查询结果返给客户端。

### const

![image-20230731142233957](MySql.assets/image-20230731142233957.png)



设计 MySQL 的大叔认为，通过主键或者唯一二级索引列与常数的等值比较来定位 条记录是像坐火箭 样快的 ，所以他们把这种通过主键或者唯一二级索号|列来定位 条记录的访问方法定义为 const (意思是常数级别的 代价是可以忽略不计的) 不过这种 const 访问方法只能在 键列或者唯一二级索引列与一个常数进行等值比较时才有效.

### ref

![image-20230731142728313](MySql.assets/image-20230731142728313.png)

### ref_or_null

![image-20230731160313708](MySql.assets/image-20230731160313708.png)

值为 NUL 的记录会被 在索引 的最左边.

### range

![image-20230731160507761](MySql.assets/image-20230731160507761.png)

### index

![image-20230731160643719](MySql.assets/image-20230731160643719.png)

### all

![image-20230731160830268](MySql.assets/image-20230731160830268.png)



### 索引合并



![image-20230731161716943](MySql.assets/image-20230731161716943.png)

![image-20230731161704153](MySql.assets/image-20230731161704153.png)



![image-20230731162355497](MySql.assets/image-20230731162355497.png)



![image-20230731162429740](MySql.assets/image-20230731162429740.png)

![image-20230803105036300](MySql.assets/image-20230803105036300.png)





![image-20230803110109225](MySql.assets/image-20230803110109225.png)



## 两个者的亲密接触 -- 连接的原理

![image-20230803181015124](MySql.assets/image-20230803181015124.png)



![image-20230804142922338](MySql.assets/image-20230804142922338.png)



![image-20230804162932120](MySql.assets/image-20230804162932120.png)

![image-20230804162942316](MySql.assets/image-20230804162942316.png)























## 基于成本的优化

![image-20230808162839829](MySql.assets/image-20230808162839829.png)

![image-20230809110101832](MySql.assets/image-20230809110101832.png)



![image-20230810164520350](MySql.assets/image-20230810164520350.png)



![image-20230810164827621](MySql.assets/image-20230810164827621.png)



![image-20230810164940070](MySql.assets/image-20230810164940070.png)



![image-20230810172008635](MySql.assets/image-20230810172008635.png)



![image-20230816093532235](MySql.assets/image-20230816093532235.png)



![image-20230816105640896](MySql.assets/image-20230816105640896.png)

![image-20230816163927861](MySql.assets/image-20230816163927861.png)

















## 兵马未动，粮草先行 -- InnoDB 统计数据是如何收的



![image-20230816172311495](MySql.assets/image-20230816172311495.png)



![image-20230818111723139](MySql.assets/image-20230818111723139.png)





## 基于规则的优化(内含查询优化二三事）



### 外连接消除

![image-20230818135305453](MySql.assets/image-20230818135305453.png)

### 子查询优化

![image-20230818140513907](MySql.assets/image-20230818140513907.png)



![image-20230818142231971](MySql.assets/image-20230818142231971.png)





![image-20230818142952158](MySql.assets/image-20230818142952158.png)



![image-20230818143329763](MySql.assets/image-20230818143329763.png)

### In 子查询优化

![image-20230818143540468](MySql.assets/image-20230818143540468.png)

![image-20230818150402905](MySql.assets/image-20230818150402905.png)



![image-20230818150417718](MySql.assets/image-20230818150417718.png)











## 查询优化的百科全书 -- EXPLAIN 详解



![image-20230818171916198](MySql.assets/image-20230818171916198.png)



![image-20230818172212181](MySql.assets/image-20230818172212181.png)



### select type

![image-20230818180312026](MySql.assets/image-20230818180312026.png)

![image-20230821112418365](MySql.assets/image-20230821112418365.png)



### type 访问方法

![image-20230821132100254](MySql.assets/image-20230821132100254.png)

#### system

![image-20230821132453515](MySql.assets/image-20230821132453515.png)

#### const 

![image-20230821132818377](MySql.assets/image-20230821132818377.png)

#### eq_ref 

![image-20230821140143166](MySql.assets/image-20230821140143166.png)

#### ref

![image-20230821143114130](MySql.assets/image-20230821143114130.png)

#### fulltext 

![image-20230821143124983](MySql.assets/image-20230821143124983.png)

#### ref_or_null

![image-20230821151408322](MySql.assets/image-20230821151408322.png)

#### index_merge

![image-20230821153646944](MySql.assets/image-20230821153646944.png)

#### unique_subquery

![image-20230821155600905](MySql.assets/image-20230821155600905.png)

#### index_subquery 

![image-20230821160534387](MySql.assets/image-20230821160534387.png)

#### range 

![image-20230821160731687](MySql.assets/image-20230821160731687.png)

#### index 

![image-20230821160912156](MySql.assets/image-20230821160912156.png)

![image-20230821161125553](MySql.assets/image-20230821161125553.png)

#### ALL

![image-20230821161226060](MySql.assets/image-20230821161226060.png)





![image-20230821161420028](MySql.assets/image-20230821161420028.png)

![image-20230821164205296](MySql.assets/image-20230821164205296.png)

![image-20230821164229821](MySql.assets/image-20230821164229821.png)

![image-20230821164310838](MySql.assets/image-20230821164310838.png)



![image-20230821165650747](MySql.assets/image-20230821165650747.png)

![image-20230821165807997](MySql.assets/image-20230821165807997.png)



![image-20230821171503104](MySql.assets/image-20230821171503104.png)





Extra



![image-20230821171702614](MySql.assets/image-20230821171702614.png)



![image-20230821172739506](MySql.assets/image-20230821172739506.png)

![image-20230821172940831](MySql.assets/image-20230821172940831.png)

![image-20230821173124800](MySql.assets/image-20230821173124800.png)



![image-20230821173250099](MySql.assets/image-20230821173250099.png)

![image-20230821173444264](MySql.assets/image-20230821173444264.png)

![image-20230821175957209](MySql.assets/image-20230821175957209.png)

![image-20230821180210125](MySql.assets/image-20230821180210125.png)



## 神兵利器 -- optimizer trace 的神奇功效

```sql
SET OPTIMIZER_TRACE="enabled=on";


explain SELECT * FROM test.single_table WHERE 
key1 > 'z' AND 
key2 < 1000000 AND 
key3 IN ('a','b','c') AND 
common_field= 'abc';

select * from information_schema.OPTIMIZER_TRACE;

SET OPTIMIZER_TRACE="enabled=off";
```



```json
{
  "steps": [
    {
      "join_preparation": {
        "select#": 1,
        "steps": [
          {
            "IN_uses_bisection": true
          },
          {
            "expanded_query": "/* select#1 */ select `test`.`single_table`.`id` AS `id`,`test`.`single_table`.`key1` AS `key1`,`test`.`single_table`.`key2` AS `key2`,`test`.`single_table`.`key3` AS `key3`,`test`.`single_table`.`key_part1` AS `key_part1`,`test`.`single_table`.`key_part2` AS `key_part2`,`test`.`single_table`.`key_part3` AS `key_part3`,`test`.`single_table`.`common_field` AS `common_field` from `test`.`single_table` where ((`test`.`single_table`.`key1` > 'z') and (`test`.`single_table`.`key2` < 1000000) and (`test`.`single_table`.`key3` in ('a','b','c')) and (`test`.`single_table`.`common_field` = 'abc'))"
          }
        ]
      }
    },
    {
      "join_optimization": {
        "select#": 1,
        "steps": [
          {
            "condition_processing": {
              "condition": "WHERE",
              "original_condition": "((`test`.`single_table`.`key1` > 'z') and (`test`.`single_table`.`key2` < 1000000) and (`test`.`single_table`.`key3` in ('a','b','c')) and (`test`.`single_table`.`common_field` = 'abc'))",
              "steps": [
                {
                  "transformation": "equality_propagation",
                  "resulting_condition": "((`test`.`single_table`.`key1` > 'z') and (`test`.`single_table`.`key2` < 1000000) and (`test`.`single_table`.`key3` in ('a','b','c')) and (`test`.`single_table`.`common_field` = 'abc'))"
                },
                {
                  "transformation": "constant_propagation",
                  "resulting_condition": "((`test`.`single_table`.`key1` > 'z') and (`test`.`single_table`.`key2` < 1000000) and (`test`.`single_table`.`key3` in ('a','b','c')) and (`test`.`single_table`.`common_field` = 'abc'))"
                },
                {
                  "transformation": "trivial_condition_removal",
                  "resulting_condition": "((`test`.`single_table`.`key1` > 'z') and (`test`.`single_table`.`key2` < 1000000) and (`test`.`single_table`.`key3` in ('a','b','c')) and (`test`.`single_table`.`common_field` = 'abc'))"
                }
              ]
            }
          },
          {
            "substitute_generated_columns": {
            }
          },
          {
            "table_dependencies": [
              {
                "table": "`test`.`single_table`",
                "row_may_be_null": false,
                "map_bit": 0,
                "depends_on_map_bits": [
                ]
              }
            ]
          },
          {
            "ref_optimizer_key_uses": [
            ]
          },
          {
            "rows_estimation": [
              {
                "table": "`test`.`single_table`",
                "range_analysis": {
                  "table_scan": {
                    "rows": 1,
                    "cost": 2.45
                  },
                  "potential_range_indexes": [
                    {
                      "index": "PRIMARY",
                      "usable": false,
                      "cause": "not_applicable"
                    },
                    {
                      "index": "uk_key2",
                      "usable": true,
                      "key_parts": [
                        "key2"
                      ]
                    },
                    {
                      "index": "idx_key1",
                      "usable": true,
                      "key_parts": [
                        "key1",
                        "id"
                      ]
                    },
                    {
                      "index": "idx_key3",
                      "usable": true,
                      "key_parts": [
                        "key3",
                        "id"
                      ]
                    },
                    {
                      "index": "idx_key_part",
                      "usable": false,
                      "cause": "not_applicable"
                    }
                  ],
                  "setup_range_conditions": [
                  ],
                  "group_index_range": {
                    "chosen": false,
                    "cause": "not_group_by_or_distinct"
                  },
                  "skip_scan_range": {
                    "potential_skip_scan_indexes": [
                      {
                        "index": "uk_key2",
                        "usable": false,
                        "cause": "query_references_nonkey_column"
                      },
                      {
                        "index": "idx_key1",
                        "usable": false,
                        "cause": "query_references_nonkey_column"
                      },
                      {
                        "index": "idx_key3",
                        "usable": false,
                        "cause": "query_references_nonkey_column"
                      }
                    ]
                  },
                  "analyzing_range_alternatives": {
                    "range_scan_alternatives": [
                      {
                        "index": "uk_key2",
                        "ranges": [
                          "NULL < key2 < 1000000"
                        ],
                        "index_dives_for_eq_ranges": true,
                        "rowid_ordered": false,
                        "using_mrr": false,
                        "index_only": false,
                        "in_memory": 1,
                        "rows": 1,
                        "cost": 0.61,
                        "chosen": true
                      },
                      {
                        "index": "idx_key1",
                        "ranges": [
                          "'z' < key1"
                        ],
                        "index_dives_for_eq_ranges": true,
                        "rowid_ordered": false,
                        "using_mrr": false,
                        "index_only": false,
                        "in_memory": 1,
                        "rows": 1,
                        "cost": 0.61,
                        "chosen": false,
                        "cause": "cost"
                      },
                      {
                        "index": "idx_key3",
                        "ranges": [
                          "key3 = 'a'",
                          "key3 = 'b'",
                          "key3 = 'c'"
                        ],
                        "index_dives_for_eq_ranges": true,
                        "rowid_ordered": false,
                        "using_mrr": false,
                        "index_only": false,
                        "in_memory": 1,
                        "rows": 3,
                        "cost": 1.81,
                        "chosen": false,
                        "cause": "cost"
                      }
                    ],
                    "analyzing_roworder_intersect": {
                      "usable": false,
                      "cause": "too_few_roworder_scans"
                    }
                  },
                  "chosen_range_access_summary": {
                    "range_access_plan": {
                      "type": "range_scan",
                      "index": "uk_key2",
                      "rows": 1,
                      "ranges": [
                        "NULL < key2 < 1000000"
                      ]
                    },
                    "rows_for_plan": 1,
                    "cost_for_plan": 0.61,
                    "chosen": true
                  }
                }
              }
            ]
          },
          {
            "considered_execution_plans": [
              {
                "plan_prefix": [
                ],
                "table": "`test`.`single_table`",
                "best_access_path": {
                  "considered_access_paths": [
                    {
                      "rows_to_scan": 1,
                      "access_type": "range",
                      "range_details": {
                        "used_index": "uk_key2"
                      },
                      "resulting_rows": 1,
                      "cost": 0.71,
                      "chosen": true
                    }
                  ]
                },
                "condition_filtering_pct": 100,
                "rows_for_plan": 1,
                "cost_for_plan": 0.71,
                "chosen": true
              }
            ]
          },
          {
            "attaching_conditions_to_tables": {
              "original_condition": "((`test`.`single_table`.`key1` > 'z') and (`test`.`single_table`.`key2` < 1000000) and (`test`.`single_table`.`key3` in ('a','b','c')) and (`test`.`single_table`.`common_field` = 'abc'))",
              "attached_conditions_computation": [
              ],
              "attached_conditions_summary": [
                {
                  "table": "`test`.`single_table`",
                  "attached": "((`test`.`single_table`.`key1` > 'z') and (`test`.`single_table`.`key2` < 1000000) and (`test`.`single_table`.`key3` in ('a','b','c')) and (`test`.`single_table`.`common_field` = 'abc'))"
                }
              ]
            }
          },
          {
            "finalizing_table_conditions": [
              {
                "table": "`test`.`single_table`",
                "original_table_condition": "((`test`.`single_table`.`key1` > 'z') and (`test`.`single_table`.`key2` < 1000000) and (`test`.`single_table`.`key3` in ('a','b','c')) and (`test`.`single_table`.`common_field` = 'abc'))",
                "final_table_condition   ": "((`test`.`single_table`.`key1` > 'z') and (`test`.`single_table`.`key2` < 1000000) and (`test`.`single_table`.`key3` in ('a','b','c')) and (`test`.`single_table`.`common_field` = 'abc'))"
              }
            ]
          },
          {
            "refine_plan": [
              {
                "table": "`test`.`single_table`",
                "pushed_index_condition": "(`test`.`single_table`.`key2` < 1000000)",
                "table_condition_attached": "((`test`.`single_table`.`key1` > 'z') and (`test`.`single_table`.`key3` in ('a','b','c')) and (`test`.`single_table`.`common_field` = 'abc'))"
              }
            ]
          }
        ]
      }
    },
    {
      "join_execution": {
        "select#": 1,
        "steps": [
        ]
      }
    }
  ]
}
```

![image-20230823105626622](MySql.assets/image-20230823105626622.png)









## 调节磁 CPU 的矛盾  -- InnoDB Buffer Pool



![image-20230823154541704](MySql.assets/image-20230823154541704.png)

![image-20230823154721798](MySql.assets/image-20230823154721798.png)



### free链表

![image-20230824165417430](MySql.assets/image-20230824165417430.png)

![image-20230824165427951](MySql.assets/image-20230824165427951.png)



![image-20230824170123464](MySql.assets/image-20230824170123464.png)

![image-20230824170657286](MySql.assets/image-20230824170657286.png)

### LRU链表

![image-20230825131925306](MySql.assets/image-20230825131925306.png)



![image-20230825141256502](MySql.assets/image-20230825141256502.png)



![image-20230829171836679](MySql.assets/image-20230829171836679.png)

![image-20230829172119425](MySql.assets/image-20230829172119425.png)









## 事务简介



![image-20230829174028788](MySql.assets/image-20230829174028788.png)

![image-20230829174519966](MySql.assets/image-20230829174519966.png)





![image-20230829174836016](MySql.assets/image-20230829174836016.png)











## 说过的话就一定要做到 -- redo 日志

![image-20230829181732202](MySql.assets/image-20230829181732202.png)



![image-20230830140507708](MySql.assets/image-20230830140507708.png)

![image-20230830164938258](MySql.assets/image-20230830164938258.png)



![image-20230830173046511](MySql.assets/image-20230830173046511.png)





![image-20230830181058027](MySql.assets/image-20230830181058027.png)



![image-20230831095927579](MySql.assets/image-20230831095927579.png)



![image-20230831103606777](MySql.assets/image-20230831103606777.png)

### innodb_flush_log_at_trx_commit

![image-20230831105025347](MySql.assets/image-20230831105025347.png)

-- 0 commit 时,redo日志不刷新到磁盘
-- 1 commit 时,redo日志刷新到磁盘
-- 2 commit 时,redo日志刷新到操作系统缓冲区（用操作系统来半持久化）

show VARIABLES like 'innodb_flush_log_at_trx_commit';

![image-20230831143410977](MySql.assets/image-20230831143410977.png)

![image-20230831143424287](MySql.assets/image-20230831143424287.png)













## 后悔了怎么办 -- undo 日志





![image-20230831143935587](MySql.assets/image-20230831143935587.png)





![image-20230831145024819](MySql.assets/image-20230831145024819.png)

### insert 类型的undo日志

![image-20230831161841673](MySql.assets/image-20230831161841673.png)

### delete 类型的undo日志

![image-20230831161753075](MySql.assets/image-20230831161753075.png)

![image-20230831161940175](MySql.assets/image-20230831161940175.png)





### UPDATE ，不改主键

![image-20230831164939370](MySql.assets/image-20230831164939370.png)

![image-20230831165028537](MySql.assets/image-20230831165028537.png)

### UPDATE ，改主键

![image-20230831164921594](MySql.assets/image-20230831164921594.png)



![image-20230831165047071](MySql.assets/image-20230831165047071.png)

### 对二级索引的影响

![image-20230831165150653](MySql.assets/image-20230831165150653.png)

### 通用链表结构

![image-20230831165521003](MySql.assets/image-20230831165521003.png)

![image-20230831165801323](MySql.assets/image-20230831165801323.png)





![image-20230831170017250](MySql.assets/image-20230831170017250.png)





### Undo页面链表



![image-20230831170127782](MySql.assets/image-20230831170127782.png)



![image-20230831170607061](MySql.assets/image-20230831170607061.png)





![image-20230831170821471](MySql.assets/image-20230831170821471.png)

### 段的概念



![image-20230831170919907](MySql.assets/image-20230831170919907.png)



![image-20230831171701839](MySql.assets/image-20230831171701839.png)

![image-20230831171720140](MySql.assets/image-20230831171720140.png)

### 回滚段



## 一条记录的多副面孔 -- 事务隔离级别和 MVCC



![image-20230831180906804](MySql.assets/image-20230831180906804.png)

![image-20230831180933995](MySql.assets/image-20230831180933995.png)

![image-20230831181035500](MySql.assets/image-20230831181035500.png)



### 脏写

![image-20230831181847256](MySql.assets/image-20230831181847256.png)

### 脏读

![image-20230901111249314](MySql.assets/image-20230901111249314.png)

### 不可重复读

![image-20230901111855147](MySql.assets/image-20230901111855147.png)

### 幻读

![image-20230901112319292](MySql.assets/image-20230901112319292.png)



SQL标准中的四中隔离级别

![image-20230901112447798](MySql.assets/image-20230901112447798.png)

![image-20230901113527046](MySql.assets/image-20230901113527046.png)



![image-20230901113539768](MySql.assets/image-20230901113539768.png)

![image-20230901113832314](MySql.assets/image-20230901113832314.png)

![image-20230901161327414](MySql.assets/image-20230901161327414.png)

### MVCC原理 -- 版本链

![image-20230903112530688](MySql.assets/image-20230903112530688.png)



![image-20230903112700278](MySql.assets/image-20230903112700278.png)

![image-20230903112740409](MySql.assets/image-20230903112740409.png)

![image-20230903112812816](MySql.assets/image-20230903112812816.png)

![image-20230903112835420](MySql.assets/image-20230903112835420.png)

![image-20230903112849494](MySql.assets/image-20230903112849494.png)

![image-20230903112902545](MySql.assets/image-20230903112902545.png)



![image-20230903112916000](MySql.assets/image-20230903112916000.png)



![image-20230903112159588](MySql.assets/image-20230903112159588.png)

### REPEATABLE READ 总结：

在一个事务执行过程中，即使其他事务在当前事务执行过程当中提交了修改，当前事务多次查询的结果，依然是当前事务刚刚开始的值，不会受到其他事务在当前事务执行过程当中提交修改值的影响。



![image-20230903112943088](MySql.assets/image-20230903112943088.png)





![image-20230903114654326](MySql.assets/image-20230903114654326.png)





![image-20230903114750072](MySql.assets/image-20230903114750072.png)







## 工作面试老大难 -- 锁



![image-20230903152914579](MySql.assets/image-20230903152914579.png)

![image-20230904110444376](MySql.assets/image-20230904110444376.png)

![image-20230904110501023](MySql.assets/image-20230904110501023.png)

![image-20230904110617227](MySql.assets/image-20230904110617227.png)



![image-20230904110633610](MySql.assets/image-20230904110633610.png)

![image-20230904110656480](MySql.assets/image-20230904110656480.png)

![image-20230904110711019](MySql.assets/image-20230904110711019.png)

![image-20230904110730748](MySql.assets/image-20230904110730748.png)

![image-20230904110814540](MySql.assets/image-20230904110814540.png)



![image-20230904110851258](MySql.assets/image-20230904110851258.png)

### INNODB 的表级锁

![image-20230904110922881](MySql.assets/image-20230904110922881.png)

![image-20230904110951410](MySql.assets/image-20230904110951410.png)

![image-20230904111019276](MySql.assets/image-20230904111019276.png)



### INNODB 的行级锁

![image-20230904111109075](MySql.assets/image-20230904111109075.png)

![image-20230904111245118](MySql.assets/image-20230904111245118.png)

![image-20230904111259261](MySql.assets/image-20230904111259261.png)





### 信号量的实现

![image-20230903122302904](MySql.assets/image-20230903122302904.png)





### INNODB 锁的内存结构





![image-20230904111452059](MySql.assets/image-20230904111452059.png)

![image-20230904111701720](MySql.assets/image-20230904111701720.png)





### 死锁

![image-20230906172333265](MySql.assets/image-20230906172333265.png)

![image-20230906173342916](MySql.assets/image-20230906173342916.png)



![image-20230906173326441](MySql.assets/image-20230906173326441.png)















































![image-20230906173236040](MySql.assets/image-20230906173236040.png)











---















