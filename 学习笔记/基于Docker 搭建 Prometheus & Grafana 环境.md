[基于Docker 搭建 Prometheus & Grafana 环境_docker安装prometheus和grafana-CSDN博客](https://blog.csdn.net/qq_15603633/article/details/153815073)

```bash
mkdir prometheus-grafana-stack
cd prometheus-grafana-stack
```

1. 创建prometheus.yml文件

> _host.docker.internal:8l23_表示请求宿主机自己的服务的后端（类似于localhost但是如果直接请求  
local host请求的是容器内部)，如果部署到服务器上就改成服务器对应的地址
>

```yaml
# Prometheus 配置文件
global:
  scrape_interval: 15s      # 全局抓取间隔
  evaluation_interval: 15s  # 规则评估间隔

# 告警管理器配置 (可选)
alerting:
  alertmanagers:
    - static_configs:
        - targets:
          # - alertmanager:9093

# 规则文件配置
rule_files:
# - "alert_rules.yml"  # 可以添加告警规则

# 抓取配置
scrape_configs:
  # Prometheus 自身监控
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  # Spring Boot 应用监控
  - job_name: 'yu-ai-code-mother'
    metrics_path: '/api/actuator/prometheus'  # Spring Boot Actuator 端点
    static_configs:
      - targets: ['host.docker.internal:8123']  # 应用服务器地址
    scrape_interval: 10s  # 每 10 秒抓取一次
    scrape_timeout: 10s   # 抓取超时时间

```

2. 下载运行镜像

```bash
docker run -d --name prometheus -p 9090:9090 -v ./prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus

```

3. http://localhost:9090

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2025/png/51149297/1766311265421-cfb893b9-efe9-439a-a4f2-f35adc7da47f.png)

4. grafana

```bash
docker run -d --name grafana  -p 3000:3000  grafana/grafana-enterprise
```

5. localhost:3000/login
6. admin | admin

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2025/png/51149297/1766311392837-6c4cf84e-64f9-4c2b-9f94-4c8a79f17766.png)

7. 添加连接，新链接

