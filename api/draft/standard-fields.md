标准的消息字段定义，当需要类似的概念时应该使用这些定义。 这将确保相同的概念在不同的API中具有相同的名称和语义。

- create_time Timestamp
- update_time Timestamp
- delete_time Timestamp
- expire_time Timestamp
- start_time  Timestamp
- end_time  Timestamp
- time_zone  string
时区名称。 它应该是IANA TZ的名称，例如“America / Los_Angeles”。 有关更多信息，请参阅https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
- region_code string  
一个位置的Unicode国家/地区代码（CLDR），例如“US”和“419”。 有关更多信息，请参阅http://www.unicode.org/reports/tr35/#unicode_region_subtag。
- language_code string
BCP-47语言代码，例如“en-US”或“sr-Latn”。 有关更多信息，请参阅http://www.unicode.org/reports/tr35/#Unicode_locale_identifier。
- mime_type  string  
IANA发布的MIME类型（也称为媒体类型）。 有关更多信息，请参阅https://www.iana.org/assignments/media-types/media-types.xhtml。  
- filter  string
- query string
如果应用于搜索方法，则与filter相同（即:search ）
- page_token  string
List请求中的分页标记。
- page_size int32
List请求中的分页大小。
- total_size  int32
列表中的项目总数不论分页。
- next_page_token string
列表响应中的下一个分页标记。 它应该作为page_token用于以下请求。 一个空值意味着没有更多的结果。
- order_by  string
指定List请求的结果排序。
- request_id  string
用于检测重复请求的唯一字符串ID。
- deleted bool
如果某个资源允许取消删除行为，则必须有一个deleted字段，指示该资源已被删除。
- show_deleted  bool
如果资源允许show_deleted删除行为，则相应的List方法必须具有show_deleted字段，以便客户端可以发现已删除的资源。
- update_mask [FieldMask](https://github.com/google/protobuf/blob/master/src/google/protobuf/field_mask.proto)
它用于Update请求消息，以便对资源执行部分更新。 该掩码是相对于资源而不是请求消息。
- validate_only bool
如果为true，则表示只能验证给定的请求，而不能执行。
