# mybatis之动态SQL

 * if
 * choose\when\otherwise
 * where
 * set
 * foreach
   * list
   * array
   * map

  

## 一、if

### 1.基本用法
   
   ```xml
     <select id="findUsers" resultType="User">
       SELECT * FROM user
       <where>
         <if test="name != null">
          AND name = #{name}
         </if>
         <if test="age != null">
          AND age = #{age}
         </if>
     </where>
    </select>
   ```
***`<if>`标签的test属性指定了判断条件，如果该条件成立，就会将`<if>`标签内的SQL语句包含到生成的SQL语句中***

### 5.注意事项

在使用<if>标签时，需要注意以下几点：

* SQL语句中的AND和OR关键字需要根据实际情况添加或删除，否则可能会导致语法错误。
* test属性中的变量名需要与JavaBean中的属性名一致，否则会抛出异常。
* 当test属性中的变量为null时，<if>标签内的SQL语句不会被包含到生成的SQL语句中。
* 当test属性中的变量为Boolean类型时，如果为false，则<if>标签内的SQL语句不会被包含到生成的SQL语句中。
* 当test属性中的变量为集合类型时，如果集合为空，则<if>标签内的SQL语句不会被包含到生成的SQL语句中。
  
综上所述，if标签在MyBatis中是一个非常常用的动态SQL标签，能够方便地根据条件生成SQL语句，提高SQL语句的灵活性和可维护性