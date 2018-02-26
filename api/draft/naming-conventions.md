命名约定

为了许多API提供一致的开发人员体验，API使用的所有名称应为：
- 简单（simple）
- 直观（intuitive）
- 具有一致性（consistent）

这包括接口，资源，集合，方法和消息的名称。

由于我们的开发人员不是以英语为母语的人，因此这些命名约定的一个目标是确保大多数开发人员能够轻松理解API。 它通过鼓励在命名方法和资源时使用简单，一致和小的词汇来实现这一点。
- 为了简洁起见，可以使用常用的短格式或长词的缩写。 例如，API比Application Programming Interface更受欢迎。
- 尽可能使用直观，熟悉的术语。 例如，在描述删除（和销毁）资源时，delete优先于erase
- 为相同的概念使用相同的名称或术语，包括跨API共享的概念(concepts)。
- 避免名字重载(overloading)。为不同的概念使用不同的名称。
- 避免在API和更大的Google API生态系统环境中含糊不清的过于笼统的名称。
- 审慎使用可能与常见编程语言中的关键字冲突的名称。

## Product Names
## Service Names
服务是一组API  
## Package Names
## Collection IDs  
Collection IDs should use plural form   
## Method Names
## Message Names  
方法的请求和响应消息应该分别以具有后缀Request和Response的方法名称命名，除非方法请求或响应类型为：
  - 一条空的消息（使用google.protobuf.Empty ），
  - 资源类型，或
  - 表示操作的资源

## Enum Names  
枚举类型必须使用UpperCamelCase名称。枚举值必须使用CAPITALIZED_NAMES_WITH_UNDERSCORES。第一个值应该被命名为ENUM_TYPE_UNSPECIFIED，因为在未明确指定枚举值时会返回该值。
```
enum FooBar {
  // The first value represents the default and must be == 0.
  FOO_BAR_UNSPECIFIED = 0;
  FIRST_VALUE = 1;
  SECOND_VALUE = 2;
}
```
## 字段名称（Field names）  
字段定义必须使用lower_case_underscore_separated_names  
字段名称应避免介词（例如“for”，“during”，“at”），例如：  
  * reason_for_error应该是error_reason
  * cpu_usage_at_time_of_failure应改为failure_time_cpu_usage   

字段名称还应该避免使用后置形容词（放在名词后面的修饰符），例如：
  * items_collected应改为items_collected
  * objects_imported应该改为imported_objects

### 重复的字段名称（Repeated field names）  
API中的重复字段必须使用适当的复数形式。这符合外部开发人员的共同期望。

### 时间和持续时间（Time and Duration）
- 要表示与时区或日历无关的时间点，[Timestamp](https://github.com/google/protobuf/blob/master/src/google/protobuf/timestamp.proto)类型,字段名称应以time结束，例如start_time和end_time

JSON格式中，Timestamp类型被编码为字符串[RFC 3339]（https://www.ietf.org/rfc/rfc3339.txt）格式。  那就是格式为“{year}-{month}-{day}T{hour}：{min}：{sec} [.{frac_sec}]Z”。其中：  
  - {year}总是使用四位数字表示，而{month}，{day}，{hour}，{min}和{sec}分别用'0'填充至两位数。
  - frac_sec（fractional seconds）：小数秒数，可以达到9位数字（即最高1纳秒分辨率），是可选的。  
  - “Z”后缀表示时区（“UTC”）,zero UTC offset;  时区是必须的。   

例如，2017年1月15日的UTC 01:30 过去15.01秒： “2017-01-15T01：30：15.01Z”

在JavaScript中，可以使用标准的[toISOString()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toISOString)方法将Date对象转换为这种格式。  

- 如果时间引用一个活动，则字段名称应该具有verb_time的形式，例如create_time ， update_time 。 避免对动词使用过去时，比如created_time或last_updated_time 。  
- 要表示两个时间点之间的时间跨度，而不受任何日历和概念（如“日”或“月”）的影响，请使用[google.protobuf.Duration](https://github.com/google/protobuf/blob/master/src/google/protobuf/duration.proto)  
  - 持续时间表示一个有符号的固定长度的时间跨度，以秒数和纳秒解析度的秒数小数来表示。它独立于任何日历和概念，如“日”或“月”。范围大约是+/-10，000年。
在JSON格式中，Duration类型被编码为一个字符串而不是一个对象，其中字符串以后缀“s”结尾（指示秒），小数部分表示纳秒（nanoseconds）。以JSON格式表示：
    - 3秒和0纳秒：“3s”
    - 3秒和1纳秒：“3.000000001s”
    - 3秒和1微秒；“3.000001s”

- 如果由于传统或兼容性原因（包括挂钟时间，持续时间，延迟和延迟） 必须使用整数类型来表示与时间相关的字段，则字段名称必须具有以下格式：xxx_{time|duration|delay|latency}_{seconds|millis|micros|nanos}
  int64 send_time_millis = 1;   
  int64 receive_time_millis = 2;

### Date and Time of Day
对于与时区和一天中的时间无关的日期， 应该使用google.type.Date并且它应该具有后缀_date 。 如果日期必须以字符串表示，则应采用ISO 8601日期格式YYYY-MM-DD，例如2014-07-30。

代表整个日历日期，例如出生日期。一天的时间和时区在别处指定或不重要。日期是相对于普列格里历日历（Proleptic Gregorian Calendar）。  
  - day可能是0，代表当天不重要的年份和月份，例如信用卡截止日期。  
  - year可能是0，代表一个独立于年份的月份和日子，例如周年日期。  

### 数量（Quantities）
用整数类型表示的数量必须包含测量单位。  
如果数量是计数（number of items)，则该字段应具有后缀_count ，例如node_count 。  

### 列表过滤器字段
filter

### 列表响应（List response）
响应消息中包含资源List的字段的名称必须是资源名称本身的复数形式。
```
message ListEventsResponse {
  repeated Event events = 1;
  string next_page_token = 2;
}
```

##  Camel case
除了字段名称和枚举值之外， .proto文件中的所有定义必须使用[Google Java Style](https://google.github.io/styleguide/javaguide.html#s5.3-camel-case)定义的UpperCamelCase名称

## 名称缩写（Name abbreviation）
对于软件开发人员中众所周知的名称缩写（如config和spec），应在API定义中使用缩写而不是全部拼写。 这将使源代码易于读取和写入。在正式文件中应使用完整的拼写。 例如：
- config (configuration)
- id (identifier)
- spec (specification)
- stats (statistics)



return collections:
- results
- values
