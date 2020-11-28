# elk 记录

## docker-compose 启动

```yaml

version: "3"
services:
  elk:
    image: sebp/elk
    restart: always
    ports:
      - "5601:5601"
      - "9200:9200"
      - "5044:5044"
    environment:
      - "ES_MIN_MEM=128m"
      - "ES_MAX_MEM=1024m"

```

## elaticsearch window Docker 需要调整参数 vm.max_map_count

[调整相关参数](https://github.com/elastic/elasticsearch/pull/58153/files)

```
wsl -d docker-desktop
sysctl -w vm.max_map_count=262144
```

## 调整filebeat 中管道配置

filebeat 免费版请用oss版本，不然无法启动

调整moudle/nginx/access/ingest/pipeline目录下的

```

description: Pipeline for parsing Nginx access logs. Requires the geoip and user_agent
  plugins.
processors:
- set:
    field: event.ingested
    value: '{{_ingest.timestamp}}'
- grok:
    field: message
    patterns:
    - (%{NGINX_HOST} )?"?(?:%{NGINX_ADDRESS_LIST:nginx.access.remote_ip_list}|%{NOTSPACE:source.address})
      - (-|%{DATA:user.name}) \[%{HTTPDATE:nginx.access.time}\] "%{DATA:nginx.access.info}"
      %{NUMBER:http.response.status_code:long} %{NUMBER:http.response.body.bytes:long}
      "(-|%{DATA:http.request.referrer})" "(-|%{DATA:user_agent.original})" %{NOTSPACE:nginx.access.request_time} %{NOTSPACE:nginx.access.upstream_response_time}
    pattern_definitions:
      NGINX_HOST: (?:%{IP:destination.ip}|%{NGINX_NOTSEPARATOR:destination.domain})(:%{NUMBER:destination.port})?
      NGINX_NOTSEPARATOR: "[^\t ,:]+"
      NGINX_ADDRESS_LIST: (?:%{IP}|%{WORD})("?,?\s*(?:%{IP}|%{WORD}))*
    ignore_missing: true
- convert:
    field: nginx.access.request_time
    type: float
    ignore_missing: true
    on_failure: 
    - set:
        field: nginx.access.request_time
        value: -1.0
- convert:
    field: nginx.access.upstream_response_time
    type: float
    ignore_missing: true
    on_failure: 
    - set:
        field: nginx.access.upstream_response_time
        value: -1.0  
- grok:
    field: nginx.access.info
    patterns:
    - '%{WORD:http.request.method} %{DATA:url.original} HTTP/%{NUMBER:http.version}'
    - ""
    ignore_missing: true
- remove:
    field: nginx.access.info
- split:
    field: nginx.access.remote_ip_list
    separator: '"?,?\s+'
    ignore_missing: true
- split:
    field: nginx.access.origin
    separator: '"?,?\s+'
    ignore_missing: true
- set:
    field: source.address
    if: ctx.source?.address == null
    value: ""
- script:
    if: ctx.nginx?.access?.remote_ip_list != null && ctx.nginx.access.remote_ip_list.length > 0
    lang: painless
    source: >-
      boolean isPrivate(def dot, def ip) {
        try {
          StringTokenizer tok = new StringTokenizer(ip, dot);
          int firstByte = Integer.parseInt(tok.nextToken());
          int secondByte = Integer.parseInt(tok.nextToken());
          if (firstByte == 10) {
            return true;
          }
          if (firstByte == 192 && secondByte == 168) {
            return true;
          }
          if (firstByte == 172 && secondByte >= 16 && secondByte <= 31) {
            return true;
          }
          if (firstByte == 127) {
            return true;
          }
          return false;
        }
        catch (Exception e) {
          return false;
        }
      }
      try {
        ctx.source.address = null;
        if (ctx.nginx.access.remote_ip_list == null) {
          return;
        }
        def found = false;
        for (def item : ctx.nginx.access.remote_ip_list) {
          if (!isPrivate(params.dot, item)) {
            ctx.source.address = item;
            found = true;
            break;
          }
        }
        if (!found) {
          ctx.source.address = ctx.nginx.access.remote_ip_list[0];
        }
      }
      catch (Exception e) {
        ctx.source.address = null;
      }
    params:
      dot: .
- remove:
    field: source.address
    if: ctx.source.address == null
- grok:
    field: source.address
    patterns:
    - ^%{IP:source.ip}$
    ignore_failure: true
- remove:
    field: message
- rename:
    field: '@timestamp'
    target_field: event.created
- date:
    field: nginx.access.time
    target_field: '@timestamp'
    formats:
    - dd/MMM/yyyy:H:m:s Z
    on_failure:
    - append:
        field: error.message
        value: '{{ _ingest.on_failure_message }}'
- remove:
    field: nginx.access.time
- user_agent:
    field: user_agent.original
    ignore_missing: true
- geoip:
    field: source.ip
    target_field: source.geo
    ignore_missing: true
- geoip:
    database_file: GeoLite2-ASN.mmdb
    field: source.ip
    target_field: source.as
    properties:
    - asn
    - organization_name
    ignore_missing: true
- rename:
    field: source.as.asn
    target_field: source.as.number
    ignore_missing: true
- rename:
    field: source.as.organization_name
    target_field: source.as.organization.name
    ignore_missing: true
- set:
    field: event.kind
    value: event
- append:
    field: event.category
    value: web
- append:
    field: event.type
    value: access
- set:
    field: event.outcome
    value: success
    if: "ctx?.http?.response?.status_code != null && ctx.http.response.status_code < 400"
- set:
    field: event.outcome
    value: failure
    if: "ctx?.http?.response?.status_code != null && ctx.http.response.status_code >= 400"
- append:
    field: related.ip
    value: "{{source.ip}}"
    if: "ctx?.source?.ip != null"
- append:
    field: related.ip
    value: "{{destination.ip}}"
    if: "ctx?.destination?.ip != null"
- append:
    field: related.user
    value: "{{user.name}}"
    if: "ctx?.user?.name != null"
on_failure:
- set:
    field: error.message
    value: '{{ _ingest.on_failure_message }}'

```

在这个patterns后，新增的配置

```yaml
- grok:
    field: message
    patterns:
```

自定义匹配的正则表达式

```yaml
 %{NOTSPACE:nginx.access.request_time} %{NOTSPACE:nginx.access.upstream_response_time}

```


自定义响应时间格式转换


```yaml
- convert:
    field: nginx.access.request_time
    type: float
    ignore_missing: true
    on_failure: 
    - set:
        field: nginx.access.request_time
        value: -1.0
- convert:
    field: nginx.access.upstream_response_time
    type: float
    ignore_missing: true
    on_failure: 
    - set:
        field: nginx.access.upstream_response_time
        value: -1.0  
```

## Kibana删除日志

```yaml
GET _all/_settings

DELETE _ingest/pipeline/filebeat*

DELETE filebeat*
```