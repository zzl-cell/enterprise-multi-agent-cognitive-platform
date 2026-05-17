"""
项目整体架构设计
基于监控采集 + 指标暴露 + 故障注入 核心模块
可独立创建 architecture 分支存放
"""
import threading
import time
import psutil
from flask import Flask

# --------------------------
# 1. 核心数据采集模块
# --------------------------
class MetricsCollector:
    def __init__(self):
        self.interval = 5
        self.cpu_usage = 0
        self.mem_usage = 0
        self.system_load = 0

    def collect(self):
        """持续采集系统指标"""
        while True:
            self.cpu_usage = psutil.cpu_percent(interval=1)
            self.mem_usage = psutil.virtual_memory().percent
            self.system_load = round(psutil.getloadavg()[0], 2)
            time.sleep(self.interval)

# --------------------------
# 2. Prometheus 指标暴露模块
# --------------------------
class MetricsExporter:
    def __init__(self, port=9100):
        self.port = port
        self.app = Flask(__name__)
        self.collector = None

    def set_collector(self, collector):
        self.collector = collector

    def run(self):
        """启动HTTP服务暴露指标"""
        @self.app.route("/metrics")
        def metrics():
            return (
                f"cpu_usage {self.collector.cpu_usage}\n"
                f"mem_usage {self.collector.mem_usage}\n"
                f"system_load {self.collector.system_load}\n"
            )
        self.app.run(host="0.0.0.0", port=self.port)

# --------------------------
# 3. 混沌工程故障注入模块
# --------------------------
class FaultInjector:
    @staticmethod
    def cpu_stress():
        """CPU满载故障注入"""
        import os
        os.popen("stress-ng --cpu 0 -q")

# --------------------------
# 4. 项目总架构（服务调度）
# --------------------------
class ProjectArchitecture:
    def __init__(self):
        # 初始化核心组件
        self.collector = MetricsCollector()
        self.exporter = MetricsExporter(port=9100)
        self.exporter.set_collector(self.collector)
        self.injector = FaultInjector()

    def start(self):
        """启动全架构服务"""
        # 多线程并发运行
        threading.Thread(target=self.collector.collect, daemon=True).start()
        threading.Thread(target=self.exporter.run, daemon=True).start()
        print("✅ 项目架构启动成功")
        
        # 保持服务运行
        while True:
            time.sleep(1)

if __name__ == "__main__":
    arch = ProjectArchitecture()
    arch.start()
