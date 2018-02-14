# JSON指南
JSON指RFC 7159 (which updates RFC 4627)，application/json媒体类型

- 属性名称必须为ASCII snake_case (never camelCase)`^[a-z_][a-z_0-9]*$`.  
属性名称必须为ASCII字符串，第一个字符必须是字母或下划线，随后的字符可以是字母、下划线或数字。
- 数组名称应该使用复数
- Boolean型属性值不能为null.如果null值是有意义的，建议将boolean型改成枚举类型。
- 如果是null值，应该将该字段移除。 但在某些情况下，null值可能是有意义的。例如，JSON Merge Patch RFC 7382使用null来指示属性删除。
- Empty array values can unambiguously be represented as the empty list, [].
- 枚举值用字符串表示
- 日期值应该遵守[RFC 3339](https://tools.ietf.org/html/rfc3339#section-5.6)规范  
对于“日期”使用字符串匹配date-fullyear "-" date-month "-" date-mday，例如：2015-05-28  
对于“日期-时间”使用的字符串匹配full-date "T" full-time，例如2015-05-28T14:07:17Z  
在请求和相应中可以使用区域便宜（zone offset），但鼓励将日期限制使用UTC。没有偏移。例如2015-05-28T14:07:17Z，而不是2015-05-28T14:07:17+00:00。从以往的经验我们了解到，区偏移是不容易理解，往往不能正确处理。  
日期的本地化需要通过服务接口完成。  
日期的存储：  所有的日期应始终存放在UTC（没有区偏移）

- May: Standards could be used for Language, Country and Currency [128]
  - [ISO 3166-1-alpha2 country](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)
  - (It’s "GB", not "UK", even though "UK" has seen some use at Zalando)
  - [ISO 639-1 language code](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)
  - [BCP-47](https://tools.ietf.org/html/bcp47) (based on ISO 639-1) for language variants
  - [ISO 4217 currency codes](https://en.wikipedia.org/wiki/ISO_4217)
